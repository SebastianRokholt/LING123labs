---
title: Solutions to Exercises from Lab Session 03
date: 2021-03-15 18:05:00 +0100
categories: [Answers to Lab Exercises]
tags: [shell, scripting, bash, zsh, awk, gawk, word-frequencies, substitution, ciphers, n-grams, RStudio, R]
pin: true
math: true
---

Try to solve the lab exercises by yourself before you view the solutions! <br>
You can find the exercises to Lab Session 03 [here](https://ling123labs.com/posts/Lab-Session-03/). <br>
> ***[Lecture notes](https://lingkurs.h.uib.no/webroot/index.php?page=nlpintro&lang=en&course=ling123)***

*Disagree with my answers, or have something to add? <br>
Leave a [comment](#post-extend-wrapper)!* <br>

---


## Part 1) Word frequencies and N-grams

### Exercise 1.1: Script files and word frequencies<br>

*a) Recreate [this example](https://lingkurs.h.uib.no/webroot/index.php?page=scripting/scriptfiles2&lang=en&course=ling123)
(`tokenize.sh` + count word frequencies) and send the output to a file. Compare this result with a frequency list
obtained otherwise, for instance, with [Antconc](https://www.laurenceanthony.net/software/antconc/).* <br>

![Frequency list in the shell](/assets/img/answers-03/chess-freq.png){: .normal}
_Creating a frequency list for chess.txt in the shell_

![Frequency list with Antconc](/assets/img/answers-03/antconc-freqlist.png){: .normal}
_Frequency list for chess.txt with Antconc_

When comparing the frequency list to Antconc, we see that the output is mostly the same.
The only difference, as far as I can see, is that Antconc sorts the occurences first by frequency, then alphabetically,
while `bash` sorts the occurrences by frequency, then in *reverse* alphabetical order. <br>

However, there might be other differences depending on the tokenization process in bash, such as that it might be
necessary for you to specify how apostrophes should be interpreted. Antconc also has a lot more functionality,
and enables you to easily sort by different parameters. It also provides a ranking of each word in the frequency list.
<br>
<br>


*b) Create a script file `freq.sh` to make a frequency list of a tokenized file given as argument.
Try to combine it with the script for tokenization.* <br>

![A script which creates a frequency list of a file](/assets/img/answers-03/freq.sh.png){: .normal}
_A script which creates a frequency list of a file_

<br>
<br>

*c) Make a frequency list of a larger text, then select lines matching a regexp, e.g. words with three letters or less.
How do frequencies of shorter words compare to frequencies of longer words?* <br>

![Word frequency by word length](/assets/img/answers-03/freq-by-word-length.png){: .normal}
_Frequency by word length_

We can select words that are three letters or less with the regular expression
`\b[[:alpha:]]{1,3}\b`. If we sort the output with `sort -k1 -nr`, we can see the words with the highest frequency.
When we execute the code for words of different lengths, we see that the relationship between word length and occurrence
resembles an exponential function. Words of three words or less are much more prevalent (at least the popular ones)
than words of four words, and words of four letters are much more prevalent than words with six letters, and so on. <br>
<br>
<br>


### Exercise 1.2: Plotting word frequencies in R <br>
*a) Recreate the example from the [lecture notes](https://lingkurs.h.uib.no/webroot/index.php?page=scripting/plotfreq&lang=en&course=ling123)
in RStudio.* <br>

![Word frequency by index in RStudio](/assets/img/answers-03/word-freq-by-index-RStudio.png){: .normal}
_An RStudio plot of frequency by index (rank) for words in for Sherlock Holmes_

<br>
<br>

*b) Make observations about the distribution of the frequencies.* <br>
As we can see in the plot, the words that have a low index (occur at the top)
in the frequency list have a very high frequency. The single most frequent word has
an index of 1 and a frequency of over 700 occurences, while the second most frequent word
occurs about 550 times. There is a very long tail of words that are only used once or twice. <br>

<br>
<br>

*c) The picture can be made clearer by using a logarithmic y-axis.
Try `plot(Frequency,log="y")` instead. Then try making both axes logarithmic with `log="xy"`.* <br>

![Word frequency by index with a log x and y axis](/assets/img/answers-03/word-freq-by-index-logarithmic.png){: .normal}
_Word frequency by index with a logarithmic x and y axis_

This plot indicates more clearly that there is an exponential relationship between
a word's index in the frequency list and the number of occurrences.
This generally means that the most frequent word (index 2) is approximately twice as frequent
as the second most frequent word, and three times more frequent than the third most frequent word,
and so on and so forth.

<br>
<br>

*d) Compare the result to the similar graph below, which shows word frequencies in different languages:* <br>
![Zipf's Law](/assets/img/Zipfs-Law.png){: .normal}
_A plot of the rank versus frequency for the first 10 million words in 30 Wikipedias (dumps from October 2015)
on a log-log scale. <br>Obtained from [Wikipedia.org](https://en.wikipedia.org/w/index.php?title=Zipf%27s_law&oldid=1001240628)
on 30.01.2021_

The graph shows that the observations we made from plotting the Sherlock frequency list
holds true for most languages. [Zip's Law](https://en.wikipedia.org/wiki/Zipf%27s_law)
roughly states that the rank (index) of a word in a frequency list is inversely proportional to its frequency.

<br>
<br>


### Exercise 1.3: N-grams
*a) Execute [these commands](https://lingkurs.h.uib.no/webroot/index.php?page=scripting/ngrams&lang=en&course=ling123)
on a larger tokenized file. Create a frequency list of the resulting bigrams.* <br>
> There's a very large text file you can use [here](https://mitt.uib.no/courses/27100/files?preview=3088069).
> You will of course need to tokenize it first :) <br>

![Creating a frequency list for bigrams in the Bible](/assets/img/answers-03/creating-bible-bigrams.png){: .normal}
_Creating a frequency list for bigrams in The Bible_

<br>
<br>

*b) Create a list of trigrams for the same file. Compare the number of trigrams with the number of bigrams.* <br>

![Creating a frequency list for bigrams in the Bible](/assets/img/answers-03/creating-bible-trigrams.png){: .normal}
_Creating a list for trigrams in The Bible and comparing the number of bigrams to trigrams_

Okay, so the number of bigrams is equal to the number of trigrams? That doesn't make sense.
What's going on here...?

![Comparing the number of bigrams to trigrams](/assets/img/answers-03/comparing-num-bigrams-to-trigrams.png){: .normal}
_Comparing the number of bigrams to trigrams_

Ah, now we see the mistake. For bigrams, there is a single line which isn't actually a bigram.
For trigrams, there are two lines which don't contain trigrams. This is because the `paste` command kept on pasting
empty space when one of the files we gave as input had reached the end. <br>

In conclusion, for The Bible the total number of bigrams is 994,514 and the total number of trigrams it's 994,513.
In general, if $$X = \text{The number of words in a given sentence K}$$, <br>
then the number of *N*-grams for sentence $$K$$ would be calculated as:

$$Ngrams_K = X - (N - 1)$$


<br>
<br>

---
<br>

## Part 2) Ciphers and string substitutions <br>

### Exercise 2.1: Ciphers
*a) Create a script `decrypt.sh` which turns the ciphertext produced by Koenraad's `encrypt.sh` back to normal. <br>
The following workflow should give the original file: `source encrypt.sh < chess.txt | source decrypt.sh`*

```shell
#!/usr/bin/env bash
# decrypt.sh

normal='qwertyuiopasdfghjklzxcvbnmQWERTYUIOPASDFGHJKLZXCVBNM'
decrypted='a-zA-Z'

tr $normal $decrypted

```

<br>
<br>

*b) Extend the strings to include the characters `æøåÆØÅ` (if your implementation of `tr` supports multi-byte characters),
and also digits, space and punctuation. Consider a less systematic ordering of the characters in the second string.* <br>

```shell
#!/usr/bin/env bash
# encrypt-improved.sh

normal='abcdefghijklmnopqrstuvwxyzæøåABCDEFGHIJKLMNOPQRSTUVWXYZÆØÅ'
decrypted='qkQeæfAJUHGORwTYødjPÆSWiXåxZLyItnØFbpgcÅoMvKsDazVCulrEhBmN'

tr $normal $decrypted

```

<br>
<br>


### Exercise 2.2: String substitutions
*Reproduce the lecture [example code](https://lingkurs.h.uib.no/webroot/index.php?page=scripting/sed&lang=en&course=ling123)
with `(EUR|NOK|USD)` instead of `kr\.?.` Now you have two groups. Use `\2` instead of `kroner` in the replacement.* <br>



<br>
<br>

### Exercise 2.3: String substitutions (2)

*a) Some accented vowels are missing from the
[example](https://lingkurs.h.uib.no/webroot/index.php?page=scripting/sed2&lang=en&course=ling123) in the lecture notes.
Add them.* <br>


<br>
<br>

*b) Replace empty onsets or codas with 0.* <br>


<br>
<br>

*c) After reading the chapter on
[cutting columns](https://lingkurs.h.uib.no/webroot/index.php?page=scripting/columns&lang=en&course=ling123),
cut the vowel column from this comma-separated output and produce a graph of the frequencies of the vowels.* <br>



<br>
<br>

---


## Part 3: Additional exercises
These exercises are slightly more challenging. See if you are able to solve them all!
Let the seminar leader know if you are stuck, and you might get a hint... <br>


### Exercise 3.1: Word frequencies (extra)
a) Tokenize [The Bible](https://mitt.uib.no/courses/27100/files?preview=3088069) and save it to a new file
`bible-tokens.txt`. <br>
b) Retrieve all words from `bible-tokens.txt` which contain the letter *A* (case insensitive)
non-initially and non-finally. Get the frequencies for these words and sort them by highest occurence, like this:

```
16117 word number one
11695 word number two
5877 word number three
...
```
c) Use `egrep` to only retrieve words from `bible-tokens.txt` that are five letters long or more,
then count and sort them like above. <br>
d) Out of all words in the text, how many percent of them are five letters or longer and used more than once? <br>
> Tip: &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;
>  $$\dfrac{\text{amount of long words which occur more than once}}{\text{total number of words in the text}}$$

d) Write a shell script that can take a regular expression (argument `$1`) and a file name (argument `$2`) from the user,
and output a frequency list like in the exercises above. <br>



### Exercise 3.2: N-grams
a) Find all trigrams in `chess.txt` containing the word *det*. Afterwards, find only trigrams containing *det* as the
first word, middle word, then last word. <br>
b) Why can it be useful to differentiate where the word occurs in the N-grams? <br>



### Exercise 3.3: String substitutions
Using `sed`, create a shell script which reformats dates written as *DD.MM.YYYY* to *YYYY-MM-DD*. <br>
