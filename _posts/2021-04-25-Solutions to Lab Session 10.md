---
title: Solutions to Exercises from Lab Session 10
date: 2021-04-25 16:40:00 +0200
categories: [Solutions to Lab Exercises]
tags: [python, Levenshtein, nltk]
pin: true
---
<br>


Disclaimer: You should always attempt to solve the lab exercises by yourself before looking at the proposed solutions
below.
The exercises to Lab Session 10 are available in their entirety [here](https://ling123labs.com/posts/Lab-Session-10/). <br>
> ***[Link to relevant lecture notes](https://lingkurs.h.uib.no/webroot/index.php?page=python/levenshtein&lang=en&course=ling123)***

*Disagree with my solutions, or have something to add? <br>
Leave a [comment](#post-extend-wrapper)!* <br>
<br>

---

<br>

### Exercise 1.1: Minimum edit distance (Levenshtein) <br>
*The Levenshtein function uses the same cost of 1 for all edits. One could argue that a substitution is a combination of a
deletion and an insertion, so it should be more costly.
Change the program so that a substitution has a cost of 2, and test for different strings.*<br>

To solve this exercise you simply need to edit the cost for substitutions, `lsub`, from 1 to 2 like this:

```python
# Script name: levenshtein.py
from functools import lru_cache


@lru_cache(maxsize=4095)
def levenshtein_dist(string, target):
    if not string:
        return len(target)
    if not target:
        return len(string)

    if string[0] == target[0]:
        return levenshtein_dist(string[1:], target[1:])

    lins = levenshtein_dist(string, target[1:]) + 1
    lsub = levenshtein_dist(string[1:], target[1:]) + 2  # change this from 1 to 2
    ldel = levenshtein_dist(string[1:], target) + 1

    return min(lins, lsub, ldel)


print(levenshtein_dist("graffe", "giraffe"))
print(levenshtein_dist("allowed", "aloud"))
print(levenshtein_dist("fined", "fine"))
```

```shell
$ python levenshtein.py
1
4
1
```


<br>


### Exercise 1.2: Recursive functions <br>
*a) Write a Python function that recursively reverses an input string.* <br>

The trick to solving this exercise is to realize the foundations of recursion. If you don't have a good grasp of
recursion yet, I advise you to check out my [lab post](https://ling123labs.com/posts/Lab-Session-10/)
on recursion before proceeding. <br>

When we want to reverse a string with recursion, we need to figure out the base case and the recursive case.
Now, the base case would be an empty string, right? Because then we could just return the string unchanged, because the
reverse of an empty string is still just an empty string.<br>

Then there's the recursive case. If we move through the string from left to right, one character at the time, we
can simply extract the character from the string and add it to the end of whatever the result would be. So for the
first letter in "Hello world", we would just remove it from the string and rerun the whole function with just
"ello world", then add the "H" to the end of the result to be returned at the end. The same goes for every other letter
in the string, and the same goes for all other possible strings. The algorithm is the same. <br>

We can implement our solution like this: <br>

```python
# Script name: reverse_string.py
def reverse_string(string):
    # Base case
    if string == "":
        return string

    # Recursive case
    return reverse_string(string[1:]) + string[0]


# Test
print('Original string: "Hello world"')
print(f'Reversed string: {reverse_string("hello world")}')
```

```shell
$ python reverse_string.py
Original string: "Hello world"
Reversed string: dlrow olleh
```

As a side note, we can use what we just learned to validate palindromes, like this: <br>

```python
# Side note: Using recursive string reversion to validate for palindromes
def reverse(s):
    if s == "":
        return s
    else:
        return reverse(s[1:]) + s[0]


def is_palindrome(string):
    if string == reverse(string):
        return True
    else:
        return False


print(is_palindrome("racecar"))
print(is_palindrome("word"))
print(is_palindrome("saippuakivikauppias"))
```

```shell
$ python palindromes.py
True
False
True
```

<br>


*b) Create a function that recursively counts the number of vowels in a string.* <br>


```python
# Script name: count_vowels.py


def count_vowels(string):
    length = len(string)

    # Base case
    if length == 1:
        if string.lower() in ['a', 'e', 'i', 'o', 'u']:
            return 1
        return 0

    # Recursive case
    vowels_a = count_vowels(string[0:length // 2])
    vowels_b = count_vowels(string[length // 2:length])

    return vowels_a + vowels_b


# test
print(count_vowels("Abacus"))
print(count_vowels("I am the one who knocks"))
print(count_vowels("The quick brown fox jumps over the lazy dog"))
```

```shell
$ python count_vowels.py
3
7
11
```


<br>

## Part 2: Introduction to XSLT


### Exercise 2.1: Filtering XML with XSLT
Look at the examples in the [lecture notes](https://lingkurs.h.uib.no/webroot/index.php?page=xml/xsl-filtering&lang=en&course=ling123). <br>
*a) Try formatting words and tags in other ways.*<br>

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet xmlns:xsl="http://www.w3.org/1999/XSL/Transform" version="1.0">
 <xsl:output method="text" />

 <xsl:template match="w">
  <xsl:apply-templates/>
  <xsl:text> (</xsl:text>
  <xsl:value-of select="@pos"/>
  <xsl:text>) </xsl:text>
 </xsl:template>

 <xsl:template match="text()">
  <xsl:value-of select="normalize-space(.)"/>
 </xsl:template>

</xsl:stylesheet>
```

```shell
$ xsltproc convert-tags.xsl lofoten.xml
Lofoten (NP0) has (VHZ) an (AT0) abundant (AJ0) selection (NN1) of (PRF) birds (NN2) . (SENT) We (PNP) meet (VVB) birds (NN2) from (PRP) the (AT0) forest (NN1) , (PUN) moors (NN2) , (PUN) highlands (NN2) , (PUN) sea (NN1) and (CJC) ocean (NN1) , (PUN) and (CJC) many (DT0) species (NN0) which (DTQ) migrate (VVB) past (AV0) Lofoten (VVB) every (AT0) spring (NN1) and (CJC) autumn (NN1) . (SENT) The (AT0) white-tailed (AJ0) eagle (NN1) flourishes (NN2) in (PRP) Lofoten (NP0) , (PUN) and (CJC) the (AT0) area (NN1) has (VHZ) one (CRD) of (PRF) the (AT0) worlds (NN2) largest (AJS) stocks (NN2) . (SENT) Most (DT0) sea (NN1) bird (NN1) species (NN0) are (VBB) found (VVN) in (PRP) this (DT0) region (NN1) : (PUN) razorbill (VVB) , (PUN) guillemot (VVB) , (PUN) cormorant (NN1) , (PUN) kittiwake (NN1) and (CJC) the (AT0) characteristic (AJ0) puffin (NN1) , (PUN) just (AV0) to (TO0) mention (VVI) a (AT0) few (DT0) . (SENT) Especially (AV0) the (AT0) farthest (AJS) islands (NN2) of (PRF) Vry (NP0) and (CJC) Rst (NP0) are (VBB) renowned (AJ0) for (PRP) their (DPS) bird (NN1) colonies (NN2) and (CJC) bird (NN1) rocks (NN2) . (SENT) Hundreds (CRD) of (PRF) thousands (CRD) of (PRF) puffins (NN2) and (CJC) other (AJ0) sea (NN1) birds (NN2) can (VM0) be (VBI) heard (VVN) and (CJC) seen (VVN) here (AV0) , (PUN) joined (VVN) in (PRP) a (AT0) colourful (AJ0) orchestra (NN1) . (SENT)
```


*b) Try to reconstruct the plain text.* <br>

In this case, it's challenging to reconstruct the plaintext perfectly. This is because we have to figure out
how to delete the preceding whitespace for the various punctuation marks in the reconstructed text. Additonally,
`xsltproc` struggles with the Norwegian characters [æøåÆØÅ]. So if you got the
following output, that's good enough.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet xmlns:xsl="http://www.w3.org/1999/XSL/Transform" version="1.0">
 <xsl:output method="text" />

 <xsl:template match="w">
  <xsl:apply-templates/>
  <xsl:text> </xsl:text>
 </xsl:template>

 <xsl:template match="text()">
  <xsl:value-of select="normalize-space(.)"/>
 </xsl:template>

</xsl:stylesheet>
```

```shell
$ xsltproc reconstruct-plaintext.xsl lofoten.xml
Lofoten has an abundant selection of birds . We meet birds from the forest , moors , highlands , sea and ocean , and many species which migrate past Lofoten every spring and autumn . The white-tailed eagle flourishes in Lofoten , and the area has one of the worlds largest stocks . Most sea bird species are found in this region : razorbill , guillemot , cormorant , kittiwake and the characteristic puffin , just to mention a few . Especially the farthest islands of Vry and Rst are renowned for their bird colonies and bird rocks . Hundreds of thousands of puffins and other sea birds can be heard and seen here , joined in a colourful orchestra .
```

However, in order to get a result closer to the original text, we can additionally remove any whitespace
preceding punctuation with a `sed` substitution, like this:
```shell
$ xsltproc reconstruct-plaintext.xsl lofoten.xml | sed -E "s/\s([\.,:])/\1/g"
Lofoten has an abundant selection of birds. We meet birds from the forest, moors, highlands, sea and ocean, and many species which migrate past Lofoten every spring and autumn. The white-tailed eagle flourishes in Lofoten, and the area has one of the worlds largest stocks. Most sea bird species are found in this region: razorbill, guillemot, cormorant, kittiwake and the characteristic puffin, just to mention a few. Especially the farthest islands of Vry and Rst are renowned for their bird colonies and bird rocks. Hundreds of thousands of puffins and other sea birds can be heard and seen here, joined in a colourful orchestra.
```
<br>
