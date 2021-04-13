---
title: Solutions to Exercises from Lab Session 06
date: 2021-03-24 20:16:00 +0100
categories: [Solutions to Lab Exercises]
tags: [AWK, awk, gawk, bash, html, zsh, scripting, shell, Ubuntu, paste, comm, wc, sort, similarity, vocabulary]
pin: true
---

Remember to at least attempt the lab exercises by yourself before you take a peek at my proposed solutions below. <br>
The exercises to Lab Session 05 are available [here](https://ling123labs.com/posts/Lab-Session-06/). <br>
> ***[Link to relevant lecture notes](https://lingkurs.h.uib.no/webroot/index.php?page=scripting/htmlcorpus&lang=en&course=ling123)***

*Disagree with my solutions, or have something to add? <br>
Leave a [comment](#post-extend-wrapper)!* <br>


---


### Exercise 1: Extracting text from HTML <br>
a) Consider refining the script from the lecture notes further by
*conflating [inflections](https://www.thoughtco.com/inflection-grammar-term-1691168)* (similar to *lemmatization*),
e.g. *"virus"* and *"viruset"*.
Identify challenges for various cases. What extra information would you ideally need? <br>
> Tip: No coding required here. Look at the output from the `korona.sh` script, and try to think about all the different
> edge cases for removing inflections. How would you - hypothetically - attempt to do this?

b) Perform a new search in the [Norwegian Newspaper Corpus](http://avis.uib.no/).
Extract words and create a frequency list.
> Tip: When you visit a web page, you can right click and select "Save Page As..." to save the HTML file to your computer.
<br>
<br>


### Exercise 2: Sorting by key <br>
Look at the example from the
[lecture notes](https://lingkurs.h.uib.no/webroot/index.php?page=scripting/sortbykey&lang=en&course=ling123).
Some lines from Bergens Tidende have the date in the wrong format.
For example, *7.* should be *07* etc. Improve the script solve this problem.
<br>
<br>


### Exercise 3: Comparing vocabularies <br>
a) For two files of about equal length, use `comm` to produce the specific vocabulary of each of the files,
in other words, the vocabulary that is unique for each file. Count the lines for each vocabulary file.
What would be possible causes for similar or different sizes of the specific vocabularies? <br>

b) What could the information in text-specific vocabularies be used for? Or, conversely, what could the vocabulary that
two files have in common tell us? <br>

c) Note that comparisons with `comm` ignores the frequency of each word.
How could we take frequencies into account when comparing two different vocabularies?
<br>
<br>


### Exercise 4: Filtering and summing columns (with Awk) <br>
a) Modify the program to select words not containing virus. The operator `!~` means "does not match". <br>

b) Try a more complex regexp. <br>

c) Try a more complex condition. <br>

d) If you like *R*, do filtering and summing in *R*.
<br>
<br>
<br>


---

<br>

## Additional exercises

*If you are finished with all other lab exercises, you can get started on the exercises below.*

<br>


### Exercise 5: Getting started with Python <br>
a) Install the latest version of [Python](https://www.python.org/) on your machine, if you don't have it already. <br>

b) Read the [Python introduction at W3Schools](https://www.w3schools.com/python/python_intro.asp). <br>

c) Get familiar with an IDE / text editor, such as Spyder or PyCharm.
You should know how to:
- Create new files and folders
- Configure a Python interpreter
- Run the code and display the output in the built-in terminal

You can also look up different shortcut keys for:
- Autocomplete line
- Select/edit multiple lines/places at once
- Run the code in the file

<br>

