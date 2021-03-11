---
title: Solutions to Exercises from Lab Session 04
date: 2021-03-11 12:01:00 +0100
categories: [Answers to Lab Exercises]
tags: [shell, scripting, bash, zsh, awk, gawk, ]
pin: true
math: true
---

Here are my solutions to the exercises from lab session 04. <br>
You should try to solve the lab exercises yourself before you peek at the answers! <br>
The exercises to Lab Session 04 are available [here](https://ling123labs.com/posts/Lab-Session-04/). <br>
> ***[Link to relevant lecture notes](https://lingkurs.h.uib.no/webroot/index.php?page=scripting/countcolumns&lang=en&course=ling123)***

*Disagree with my solutions, or have something to add? <br>
Leave a [comment](#post-extend-wrapper)!* <br>

---


### Solution to Exercise 1: Counting columns with Awk <br>
*Look at the example from the
[lecture notes](https://lingkurs.h.uib.no/webroot/index.php?page=scripting/countcolumns&lang=en&course=ling123).
The French examples **d'information** and **l'agriculture** are contractions of two words.
Translate the apostrophe to a space before counting.*
<br>

We can use `sed` to substitute apostrophes by running `sed "s/'/ /g"` and sending the output to the rest of the
code as seen in the lecture notes. Like this:

```shell
$ sed "s/'/ /g" fraud-mwes.txt | head
cas de fraude
direction générale
type de contrôle
fiche d information
identification de la fiche-mère
transit communautaire
prélèvement agricole
communication trimestrielle
enquête administrative
principal obligé
```

Running the complete line of code gives us:
```shell
$ sed "s/'/ /g" fraud-mwes.txt | awk '{print NF}' | head | sort | uniq -c | sort -nr
    101 2
     54 3
     23 4
      4 5
      2 6
      1 7
```
<br>

### Solution to Exercise 2: Cutting columns and numbering lines <br>
*a) Extend the script, for instance with `sed`, so that you put a period after each number.
Experiment with different formats.* <br>

We can insert a period after each number with the `sed` command, by dividing the line into two groups
and inserting them at the end with a period and *tab* in between. You might have to use whitespace
`\s` instead of *tab* `\t` depending on how your file is formatted on your system.

```shell
$ awk '{ print $2 }' most-freq-en.txt | sort | nl | sed -E 's/(\s*[[:digit:]]+)\t(\w+)/\1.\t\2/g' | head
     1.  A
     2.  About
     3.  After
     4.  All
     5.  Also
     6.  An
     7.  And
     8.  Any
     9.  As
    10.  At
```
<br>

*b) Reverse the columns in the last example, so that the numbers come after the words.
This can be done by adding `| awk '{ print $2 " " $1 }'` after the `nl` command.* <br>

As given in the exercise description above, we first insert `| awk '{ print $2 " " $1 }'`
after the `nl` command. This awk code will print the words first, then a space, then the numbers at the end.
However, we now need to modify our `sed` expression to account for the reordering of the columns.
We can just reorder the elements, and then insert the groups at the end with a *tab* character (`\t`) at the beginning
and between the groups so that the output looks neat with even spacing between the columns.

```shell
$ awk '{ print $2 }' most-freq-en.txt | sort | nl | awk '{print $2 " " $1}'|
 sed -E 's/(\w+) ([[:digit:]]+)/\t\1\t\2./g' | head
        A       1.
        About   2.
        After   3.
        All     4.
        Also    5.
        An      6.
        And     7.
        Any     8.
        As      9.
        At      10.
```

<br>

### Solution to Exercise 3: Reverse dictionary <br>
*Check the alphabetical order for a few different languages.*<br>

[This Wikipedia article](https://en.wikipedia.org/wiki/Alphabetical_order#Language-specific_conventions)
provides a detailed description of the alphabetic ordering for different languages!
In particular, read the section on Language-Specific conventions.
