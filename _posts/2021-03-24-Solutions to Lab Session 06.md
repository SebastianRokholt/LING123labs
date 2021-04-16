---
title: Solutions to Exercises from Lab Session 06
date: 2021-03-24 20:16:00 +0100
categories: [Solutions to Lab Exercises]
tags: [AWK, awk, gawk, bash, html, zsh, scripting, shell, Ubuntu, paste, comm, wc, sort, similarity, vocabulary]
pin: false
---

Remember to at least attempt the lab exercises by yourself before you take a peek at my proposed solutions below. <br>
The exercises to Lab Session 05 are available [here](https://ling123labs.com/posts/Lab-Session-06/). <br>
> ***[Link to relevant lecture notes](https://lingkurs.h.uib.no/webroot/index.php?page=scripting/htmlcorpus&lang=en&course=ling123)***

*Disagree with my solutions, or have something to add? <br>
Leave a [comment](#post-extend-wrapper)!* <br>


---


### Exercise 1: Extracting text from HTML <br>
*Consider refining the script from the lecture notes further by
*conflating [inflections](https://www.thoughtco.com/inflection-grammar-term-1691168)*,
e.g. *"virus"* and *"viruset"*.
Identify challenges for various cases. What extra information would you ideally need?* <br>

This process is called "stemming". In order to identify all different inflections, one would need a dictionary of sorts
to look up the base for each word. There are
[many different function in the *Natural Language Toolkit*](https://www.nltk.org/howto/stem.html) (NLTK)
for Python which accomplish stemming in different ways. <br>

<br>

### Exercise 2: Sorting by key <br>
*Look at the example from the
[lecture notes](https://lingkurs.h.uib.no/webroot/index.php?page=scripting/sortbykey&lang=en&course=ling123).
Some lines from Bergens Tidende have the date in the wrong format.
For example, *7.* should be *07* etc. Improve the script solve this problem.* <br>

You can add another `sed` expression in the beginning to solve this problem. Use regex groups to extract what
you want to keep and use regex insertions (`\1`, `\2`, etc.) to insert them back into the string for further processing.

```shell
#!/usr/bin/env bash
# corona-date-improved.sh

egrep '<b>' cqp-avis-korona-1000.html |
 # Fixing dates in the wrong format (2. instead of 02)
 sed -E 's!^(......)(.)\.(.*<b>......-?.*</b>.*)!\10\2\3!' |

 # Create columns of newspaper, date and keyword
 sed -E 's!^(..)(......).*<b>......-?(.*)</b>.*!\1 \2 \3!' |
 awk '{print $1, $2, tolower($3)}' |

 # Sort by date
 sort -k2 |

 # Sort by keyword, removing duplicates
 sort -u -k3 |

 # Sort by date again
 sort -k2
```

<br>


### Exercise 3: Comparing vocabularies <br>
*a) For two files of about equal length, use `comm` to produce the specific vocabulary of each of the files,
in other words, the vocabulary that is unique for each file. Count the lines for each vocabulary file.
What would be possible causes for similar or different sizes of the specific vocabularies?* <br>


Counting the number of unique words in the first file (i.e. "chatterley-types"), then the second (i.e "sherlock-types").
Both files must be tokenized and sorted alphabetically in advance.

```shell
$ comm -23 chatterley-types sherlock-types | wc -l
1741
$ comm -13 chatterley-types sherlock-types | wc -l
1745
```

The similarly large sizes in the unique vocabularies for the two texts might indicate that the texts are from different
domains/genres or that they, such as is the case with these two texts, are written in two different time periods.
The almost equal amount of unique words can also indicate that the two texts use a similarly complex language.


*b) What could the information in text-specific vocabularies be used for? Or, conversely, what could the vocabulary that
two files have in common tell us?* <br>

The unique vocabularies could tell us what kind of domain each of the texts are in. They could also indicate how similar
or dissimilar the texts are. <br>

*c) Note that comparisons with `comm` ignores the frequency of each word.
How could we take frequencies into account when comparing two different vocabularies?*

Say we have two different texts, like this:
  text 1: "A day may come when the courage of men fails… but it is not this day.”
  text 2: "Every man of courage is a man of his word."

One could compare the two texts by taking frequencies into account by creating a vector of each text,
and then compare the vectors mathematically. This can be accomplished in 5 steps:

  1. Tokenize and lemmatize both texts. Remove stopwords.
  2. Pick only the unique words from both texts.
  3. Count the number of occurences of unique words in each of the texts.
> Analysis of text 1:
> day, 2
> come, 1
>courage, 1
>man, 1
>every, 0
>word, 0

>Analysis of text 2:
>day, 0
>come, 0
>courage, 1
>man, 2
>every, 1
>word, 1

  4. Create a vector for the frequency of the unique words for each text
> vector for text 1:
> [2, 1, 1, 1, 0, 0]

> vector for text 2:
> [0, 0, 1, 2, 1, 1]

  5. Calculate the similarity between the vectors, using some similarity metric

> metric 1: euclidean distance
> metric 2: hamming distance
> metric 3: cosine similarity
> + a bunch of others

<br>
<br>
