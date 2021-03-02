---
title: Solutions to the exercises from Lab Session 02
date: 2021-02-24 17:30:00 +0100
categories: [Answers to Lab Exercises]
tags: [regex, regular_expressions, shell, wsl, bash, wc, nano, zsh, scripting, grep, egrep, tr, read, echo, awk, gawk]
pin: true
---

Try to solve the lab exercises yourself before you view the solutions! <br>
You can find the exercises to Lab Session 02 [here](https://ling123labs.com/posts/Lab-Session-02/). <br>
> ***[Lecture notes](https://lingkurs.h.uib.no/webroot/index.php?page=scripting/searchregexp&lang=en&course=ling123)***

*Disagree with my answers, or have something to add? <br>
Leave a [comment](#post-extend-wrapper)!*

---


### Exercise 1.1: Introduction to regexps<br>

*a) Here's a different example. Take a look at the
[regex cheat sheet](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Cheatsheet)
to figure out what each of the characters in the expression mean.*

![egrep demo 2](/assets/img/regex%20demo%201.png){: .normal}
_Searching for words which begin with 'trav'_

 - `^`: "Begins with"
 - `trav`: Four characters we want to search for
 - `\w`: A word character
 - `*`: 0 or more occurrences of the previous character (the `\w`)

<br>
<br>


*b) This regular expression searches for all four-digit numbers in the file.
There are multiple different expressions which accomplish the same thing.
See if you can find them!*

![egrep demo 3](/assets/img/regex%20demo%203.png){: .normal}
_Searching for four-digit numbers_

There a many ways to search for digits, and some variants are only implemented in certain programs/languages/shells.
Here are a few examples:
  - As shown above, `[[:digit:]]` or `[:digit:]` (depends on the implementation)
  - With `[0-9]`, which selects any digit in the range 0-9
  - `\d` might also work in some regex implementations <br>
  - `(0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9)` will also work.

<br>
<br>


### Exercise 1.3: Tokenization with egrep <br>
*Look at the examples in the lecture notes for `Tokenization with egrep`. <br>
a) As an exercise, change the regexp to make a list of words with two letters or fewer.*<br>

The following shell script will extract all words with two characters or less (1 or 2 characters)
from a given input file. This includes numbers.

```shell
egrep -ow '^\w{1,2}\b' filename
```

If you don't want to retrieve numbers, you can use `egrep` with this expression instead:
```shell
egrep -ow '^[[:alpha:]]{1,2}\b'
```
<br>
<br>

*b) What do you gain and what do you lose by case folding?* <br>
Case folding can be used to identify unique words regardless of whether the words are at the beginning
of a new sentence or not. Since lower and upper case characters are encoded differently, we will have to use
case folding before creating a frequency list or tokenization of a file.

However, case folding can sometimes make it harder to distinguish between
[common nouns](https://www.merriam-webster.com/dictionary/noun#note-1) and
[proper nouns](https://www.merriam-webster.com/dictionary/proper%20noun), such as the name of
a person, a place, or a thing. Since a lot of names are based on existing words/concepts, many words mean
different things depending on whether they are capitalized or not. For example,
*Bash* is a shell environment, while *(to) bash* is a verb. <br>
<br>
<br>

*c) What is a word? Should we exclude numbers? What about compounds with hyphens?*<br>
In general, I guess we can say that while words can be used to express numbers,
the words are not the numbers themselves, and the numbers are certainly not words.
So while "one" is a word, "1" is a numeral (digit). <br>
<br>
Whether we should exclude numbers depends on what our current aim is. It is common to exclude
numericals (*1*), while keeping words that express numbers (*one*). <br>
<br>
The same goes for compounds with hyphens. Technically, they are not a single word.
However, they are often used as single word, and sometimes a compund with a hyphen has a very specific meaning
that cannot be captured without the hyphen. We should therefore think twice i.e. before removing or keeping hyphens when we
are creating a frequency list or tokenizing a file.

<br>
<br>

---

### Exercise 2.2: Extracting file contents with regex and shell scripting
*a) Create a second script which takes a regular expression from the user and filters the records.* <br>
There are many ways to do this, and that's kind of the point.
Play around with different scripts by prompting the user for an input, and then doing something
simple with it. Here's my suggestion:

![An example of filtering contents of a file with a script](/assets/img/answers-02/filter.sh.png){: .normal}
_Filtering the contents of a file with a simple script using grep_

<br>
<br>


### Exercise 2.3: Character translation <br>
*a) Try the last example in the lecture notes section about character translation,
but read input from a file instead of the terminal by means of `< chess.txt`.* <br>

```shell
tr -cs 'a-zA-Z '\n' < chess.txt
```

<br>
<br>

*b) Write a script that replaces all vowels with asterisks*. <br>

```shell
#!/usr/bin/env bash
# replace_vowels.sh

tr -s 'aeiouyæøå' '*' <$1

```

Output from running the script on the command line:
```bash
$ source replace_vowels.sh chess.txt
– Er d*t *ll*r*d* r*m*s? sp*rt* pr*gr*ml*d*r Ol* R*lfsr*d * NRKs st*d*.

D*t v*r sp*lt 40 s*k*nd*r *v p*rt*t m*ll*m W*ng H* *g J*n N*p*mnj*sjtsj*j. D*
t*k d* hv*r*ndr* * h*nd*n, *tt*r * h* t*tt t*lv tr*kk p* br*tt*t.

etc...
```

<br>
<br>


### Exercise 2.4: Tokenization with tr <br>
*a) Why is there an empty line at the beginning of the tokens file? If it bothers you, try to remove it by adding
`egrep -v '^$'` to the pipeline.*<br>
There is an empty line at the beginning of the tokens file because `chess.txt` begins with a hyphen.
Because of the expression `tr -cs '[[:alnum:]]' '\n'`, all characters that aren't an alphanumerical character
have been replaced with a newline. The hyphen is not an alphanumerical character, and has therefore been replaced.

We can grep only the lines in the output from the `tr` command which contain text by adding
`| egrep -v '^$'` like this:

```shell
tr -cs '[[:alnum:]]' '\n' < chess.txt | egrep -v '^$' | less
```

<br>
<br>


*c) How could we distinguish meaningful capitalization
(as in proper nouns) from accidental capitalization, i.e. at the beginning of a sentence?*<br>
This is possible to do by only lowercasing words that occur at the beginning of a sentence.
However, this still might not work for all edge cases, as some proper nouns also occur at the
beginning of a sentence. Ideally, we would need a dictionary to look up whether a given word occurring
at the beginning of a sentence is a proper or a common noun. We could then decide whether the word should
be lowercased or not. <br>

<br>
<br>

*d) When tokenizing, you can also select other strings than whole words. Find all consonant clusters by using the regular
expression `[bcdfghjklmnpqrstvwxzBCDFGHJKLMNPQRSTVWXZ]` instead.* <br>

```bash
# Replace any character that is not a consonant with a newline
$ tr -cs '[bcdfghjklmnpqrstvwxzBCDFGHJKLMNPQRSTVWXZ]' '\n' < chess.txt | less
r
d
t
ll
r
d
r
m
s
sp
etc...
```

<br>

