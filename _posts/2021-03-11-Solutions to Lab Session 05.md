---
title: Solutions to Exercises from Lab Session 05
date: 2021-03-24 20:16:00 +0100
categories: [Solutions to Lab Exercises]
tags: [shell, bash, zsh, awk, sed, frequency, alphabet, alphabets, ordering, regex, regular_expressions, palindrome, roman, numerals]
pin: false
---

Remember to at least attempt the lab exercises by yourself before you take a peek at my proposed solutions below. <br>
The exercises to Lab Session 05 are available [here](https://ling123labs.com/posts/Lab-Session-05/). <br>
> ***[Link to relevant lecture notes](https://lingkurs.h.uib.no/webroot/index.php?page=scripting/palindromes&lang=en&course=ling123)***

*Disagree with my solutions, or have something to add? <br>
Leave a [comment](#post-extend-wrapper)!* <br>

---


### Exercise 1: Palindromes with Awk <br>
*a) Apply the
[palindromes script](https://lingkurs.h.uib.no/webroot/index.php?page=scripting/palindromes&lang=en&course=ling123)
to a large word list in order to find as many palindrome words as possible in a language.
Many systems contain a dictionary file /usr/share/dict/words. Alternatively, you can download
[The Unix Dictionary](http://wiki.puzzlers.org/pub/wordlists/unixdict.txt).* <br>

Using Ubuntu, I downloaded the Unix Dictionary from the web and saved it to a file `unixdict.txt`.
I then gave the file as input to the script `palindromes.sh`:

```shell
#/usr/bin/env bash
# palindromes.sh
rev $1 | paste $1 - | awk -F '\t' '{if ($1 == $2) print $1}'
```

```shell
# You can specify how many lines you want "head" to show with the flag "--lines=number"
$ source palindromes.sh unixdict.txt | head --lines=20
a
aaa
aba
ababa
ada
ala
ama
ana
anna
b
bib
bob
bub
c
cdc
civic
d
dad
deed
did
```

<br>


*b) The script selects all strict palindromes, but not phrase palindromes, such as "Anne var i Ravenna".
Extend the script so that the punctuation, spaces and capitalization are ignored.* <br>

This exercise can be solved by creating three columns/fields:
1. the original string
2. the normalized string
3. the reversed normalized string.
Then, if fields 2 and 3 are equal, field 1 is returned. <br>

The `paste` command can take three arguments, like this: `paste $1 $1 -`.
If you pass this to `awk`, you'll have three fields (columns) to work with. <br>

You can also use the AWK function [`gsub`](https://www.gnu.org/software/gawk/manual/html_node/String-Functions.html)
to perform string substitutions in `awk`, so we don't have to use `sed` here.

This is one way to do it, though I made it so that the script actually prints the result for each word
out to the terminal ("word" is NOT a palindrome / "word" is NOT a palindrome), which can be kind of annoying,
though useful to see if the script is working properly.
You should attempt to create your own version which only prints the palindromes and ignores the rest!

> PS: Comments in the code are denoted with `#`. Everything after a `#` on the same line is a comment.
> The comments are there to explain what the code is doing. Read them closely, and try to make sure that you
> understand what the code is doing at each step!

```shell
#!/usr/bin/env bash
# palindromes-improved.sh

# Reverse the file contents, then paste the result next to two columns of the original words.
# Pipe this into awk, using tab as the field separator.
rev $1 | paste $1 $1 - | awk -F'\t' '{
  # Substitution on field 2 and 3 with gsub()
  # to get a normalized version of the regular (field 2) and reversed (field 3) strings
  gsub(/[^[:alnum:]]/, "", $2);
  gsub(/[^[:alnum:]]/, "", $3);

  # Check whether the normalized string (field 2) is equal to the reversed normalized string (field 3).
  if (tolower($2)==tolower($3)) {
      # If yes, then we print out the result for the original string, field 1, which has not been normalized
      printf("  \"%s\" is a palindrome \n", $1)
     }

  # If the normalized string (field 2) is NOT equal to the reversed normalized string (field 3),
  # then we print the result for this as well
  else {
      printf("  \"%s\" is NOT a palindrome \n", $1)
    }
}'
```

I created a test file `test_palindromes.txt` with some palindromes, both strict palindromes and palindromes with typographical characters
such as exclamation points and commas. There are also a few words which aren't palindromes at all.

```text
palindrome
02.02.2020
0202-2020
radar
level
regninger
step on no pets
Anne var i Ravenna.
end
Agnes i senga
Høyspent
accent
Wow Bob, WOW!
Bad dab!
madam
not a palindrome
sentence
words and words
```

Running the `palindromes-improved.sh` script on the test file `test_palindromes.txt` gives us the following:

```shell
$ source palindromes-improved.sh test_palindromes.txt
  "palindrome" is NOT a palindrome
  "02.02.2020" is a palindrome
  "0202-2020" is a palindrome
  "radar" is a palindrome
  "level" is a palindrome
  "regninger" is a palindrome
  "step on no pets" is a palindrome
  "Anne var i Ravenna." is a palindrome
  "end" is NOT a palindrome
  "Agnes i senga" is a palindrome
  "Høyspent " is NOT a palindrome
  "accent " is NOT a palindrome
  "Wow Bob, WOW!" is a palindrome
  "Bad dab!" is a palindrome
  "madam" is a palindrome
  "not a palindrome" is NOT a palindrome
  "sentence" is NOT a palindrome
  "words and words" is NOT a palindrome
```

<br>
<br>

*c) Create a script that finds all words in a file that are reverse anagrams but not palindromes,
for instance, "rail" and "liar".*

For this exercise we will need to use AWK arrays, also known as "associative arrays".
The associative array in AWK is a data structure that works and looks a lot like the "dictionary"
data structure in Python. <br>
If you are unfamiliar with using AWK arrays, you should take a look at
[this tutorial](https://www.tutorialspoint.com/awk/awk_arrays.htm) before progressing further. <br>

We can solve the problem by applying our knowledge of `paste` and AWK arrays.
Look at the comments (indicated with `#`) in the script, which try to explain what the code does. <br>

```shell
#!/usr/bin/env bash
# find-anagrams.sh

# Give awk the original and reversed words as input. Set awks's field seperator to tab ('\t')
rev $1 | paste $1 - | awk -F'\t' '{
    # If the word is not a palindrome
    if ($1 != $2)
        # Lowercase both the reversed and original words, and add them to an associative array.
        # The original word is the index, the reversed word is the value.
        words[tolower($1)] = tolower($2);
}

# Loop through everything again, but this time check if each word has an anagram stored in the array.
{
     for (i in words)
          if (words[i] == tolower($1))
          # If yes, there is another word in the file which is the reversible anagram.
          # Print these words out to the terminal
                printf("%s <---> %s \n%s <---> %s\n", $1, i, i, $1);
}'
```

If you don't recognize the syntax or a function that is being used here or in exercise *1b*, you can go back to the
[lab session notes on AWK](https://ling123labs.com/posts/Lab-Session-04/)
or look online and see if you can find some examples and
explanations of the specifics in order to understand the whole program better.
You really just need to try to use AWK yourself, write some simple programs, do some more exercises,
and work from there. <br>

If you can understand the AWK code above, you are doing really well!
If not... No worries - you'll get there! <br>

Luckily, for this course, the programming doesn't really get more complicated than this :)

<br>
<br>


### Exercise 2: Character to word ratio <br>
*a) Look at the script in the
[lecture notes](https://lingkurs.h.uib.no/webroot/index.php?page=scripting/charwordratio&lang=en&course=ling123).
The script counts all characters, including spaces, punctuation, and so on, even if they are not part of words.
Assuming that there is about one space per word, subtract 1 from the ratio to come closer to the average word length.* <br>

We need to subtract one from the calculated ratio, so we simply add `- 1` to that part of the script.
Remember that this script only works in the Z-shell, and therefore needs to be run with `zsh`.

```shell
#!/usr/bin/env zsh
# charwordratio.sh
                                                                                                                        nrchars=$(wc -c $1 | awk '{print $1}')
nrwords=$(wc -w $1 | awk '{print $1}')

# Calculate the ratio, subtract 1 to get closer to the average word length
((ratio = $nrchars/$nrwords.0 - 1))

printf "The ratio of characters to words in $1 is %.2f.\n" $ratio

```
<br>


*b) To be even more exact, tokenize the text file first and then compute the character-to-word ratio.* <br>

We already know how to tokenize, and we already have a script for computing the character-to-word ratio.
So we simply tokenize the file first and save the output to a file `lofoten-tokenized.txt`, and run
`charwordratio.sh` with the Z-Shell interpreter `zsh` instead of `source` (which uses `bash` on my system. This might
be different on yours. On MacOS, `zsh` is usually set as the default).

```shell
$ source tokenize.sh lofoten.txt > lofoten-tokenized.txt
$ head lofoten-tokenized.txt
lofoten
has
an
abundant
selection
of
birds
we
meet
birds
$ zsh charwordratio.sh lofoten-tokenized.txt
The ratio of characters to words in lofoten-tokenized.txt is 4.86.
```

<br>

*c) Print more information, such as the number of words and characters.* <br>

We've already calculated the number of words and characters in order to calculate the ratio,
so the only thing we need to do is to print out these variables to the terminal along with some text.
In this case I use `printf` (formatted print), but you could also use `echo`.

```shell
#!/usr/bin/env zsh

nrchars=$(tr -cs 'a-zA-Z' < $1 | wc -c | awk '{print $1}')
nrwords=$(tr -cs 'a-zA-Z' < $1 | wc -w | awk '{print $1}')
((char_ratio = $nrchars/$nrwords.0))

printf "There are %i words in $1.\n" $nrwords
printf "There are %i characters, and the ratio of characters to words is %.2f.\n" $nrchars $char_ratio
```

```shell
$ zsh charwordratio-improved.sh lofoten-tokenized.txt
There are 111 words in lofoten-tokenized.txt.
There are 651 characters, and the ratio of characters to words is 5.86.
```

<br>

*d) Consider a way to compute the ratio of words to sentences.
This is more difficult because one needs to find sentence boundaries.* <br>

A very simple approach is to calculate the number of sentences by "grepping"
(which means to use `grep` or `egrep` to retrieve)
all punctuation, exclamation and question marks (and dashes, if we are working with dialogue-heavy texts like books) and
counting them with `wc -l`. It's not perfect, but it'll get you quite close to the real answer.

```shell
#!/usr/bin/env zsh
# wordsent-ratio.sh

nrwords=$(tr -cs 'a-zA-Z' '\n' < $1 | wc -l | awk '{print $1}')
nrsentences=$(egrep -o '(\.|\?|\!)' $1 | wc -l)
((sw_ratio = $nrwords/$nrsentences.0))

printf "The ratio of words to sentences is %.2f.\n" $sw_ratio
```

```shell
$ zsh word-sentence-ratio.sh ../lofoten.txt
The ratio of words to sentences is 18.17.
```

In truth, detecting sentence boundaries is a notoriously difficult task in natural language processing,
because there are so many different edge cases. There are a few libraries in Python which provide functions for sentence detection,
but even the most advanced of these struggle with cases such as "5 P.M. eastern time" or "Kahoot! is awesome!".
Most often, we have to tailor code from an existing Python library to the specific text or problem we are working with
in order to achieve the best results. Sometimes, we have to settle for a solution that is "good enough"!

<br>
<br>


### Exercise 3: Character frequencies with Awk <br>
*a) Try also `source encrypt.sh < chess.txt | source charfreq.sh | head`. This will give you a clue about which cipher
letter represents the most frequent characters.* <br>

```shell
$ source encrypt.sh < chess.txt | source charfreq.sh | head
79
52 t
36 z
29 k
25 q
22 s
22 o
22 l
22 f
18 u
```

If we compare the result to the output from running `charfreq.sh` on the unencrypted file, we get this:
```shell
$ source charfreq.sh < ../chess.txt | head
79
52 e
36 t
29 r
25 a
22 s
22 n
22 l
22 i
18 g
```

We can then compare the encrypted character frequency with the unencrypted frequencies to decipher the encrypted text.
Think about this: How could you use these methods to decrypt a text
(which is encrypted with this simple character-to-character shift cipher) when you don't have the
original unencrypted version?

> Hint: You might need a corpus.

<br>


*b) Extend the script from the
[lecture notes](https://lingkurs.h.uib.no/webroot/index.php?page=scripting/charfreq&lang=en&course=ling123) by first
changing all uppercase to lowercase letters.* <br>

We can use the `tolower()`-function to lowercase the characters before we count them.

```shell
#!/usr/bin/env bash
# charfreq.sh

awk '
  BEGIN { FS="" }
  { for (field=1; field <= NF; field++) {
    freq[tolower($field)]++ }
  }
  END { for (char in freq)
        { print freq[char], char } }' |
sort -nr
```

<br>
<br>


### Exercise 4: An interesting regular expressions problem
*Create a regular expression which matches all [roman numerals](https://www.britannica.com/topic/Roman-numeral).
It's okay if your expression also matches an empty string.* <br>

Before solving this problem, we need to familiarize ourselves with how Roman numerals work.
The syntax is quite different from the decimal system, but there are still patterns and structures we can exploit
when we want to create a regular expression. It is therefore important to look at all of the different
variations of numerals, and try to figure out how each variation can be captured in a single expression. <br>

![A picture explaining the value of each roman numeral](/assets/img/answers-05/roman-numerals-explanation.png){: .normal}
_An explanation of the decimal values for each of the Roman numerals_

I also posted a sample file [`roman_numerals.txt`](https://mitt.uib.no/files/3148777/download?download_frd=1)
with a lot of different numbers written in Roman numerals. <br>

The challenging part about this exercise is that each letter used in a Roman numeral (`M`) is optional, so there isn't
a single Roman numeral that always occurs. However, there is a very specific ordering of the numerals which is allowed.
For example, letters that are of higher value (like `M` or `C`) need to occur before letters of lower value. The exeption
is of course the `I`-deduction-rule, where `IX` = 9 and `IV` = 4. We therefore need to use a series of
optional groups in order to capture the different variations. <br>

There are a few different versions which will work, but here's one of them:
```regexp
^M*(C[MD]|D?C{0,3})(X[CL]|L?X{0,3})(I[XV]|V?I{0,3})$
```

Explanation of the characters in the expression:
  - The initial `^`: The word/number to be match must start with the following
  - `M*`: zero or more occurences of a single "M", which has a value of 1000
  - `(C[MD]|D?C{0,3})`: The first optional group to capture the range 100 - infinity, which either:
    - begins with a "C", followed by a single optional "M" (`CM` = 900) or "D" (`CD` = 400), or:
    - begins with 1 or 0 "D"s, followed by 0-3 occurences of "C". (`DCCC` = 800)
  - `(X[CL]|L?X{0,3})`: The second optional group, for the range 10 - 99, which either:
    - begins with a single "X", followed by an optional "C" or "L"- begins with 0 or 1 "L", followed by 0-3 occurrences of "X"
  - `(I[XV]|V?I{0,3})`: The last optional group to capture the range 1 - 9, which either:
    - begins with a single "I", followed by a single "X" (`IX` = 9) or a single "V" (`IV` = 4), or:
    - begins with 0 or 1 occurrences of "V", followed by 0-3 "I"s.
  - The final `$`: The word/number to be matched must end here. For example there can be a whitespace or newline.

As you can see, it's not really possible to create a regular expression for roman numerals which doesn't match the
empty string. Well, at least not that I know of. Make sure to tell me if you figure it out... ;)
<br>



