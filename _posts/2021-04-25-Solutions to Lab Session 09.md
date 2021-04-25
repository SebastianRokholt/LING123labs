---
title: Solutions to Exercises from Lab Session 09
date: 2021-04-25 16:28:00 +0200
categories: [Solutions to Lab Exercises]
tags: [python, dictionary, dictionaries, regex, zip, zipper, n-grams, n-gram, frequency, counter, frequencies, Levenshtein, casefold, nltk]
pin: true
---
<br>

![Python Logo](/assets/img/lab-post-07/python-logo.png){: height="100"}


Disclaimer: You should always attempt to solve the lab exercises by yourself before looking at the proposed solutions
below.
The exercises to Lab Session 09 are available in their entirety [here](https://ling123labs.com/posts/Lab-Session-09/). <br>
> ***[Link to relevant lecture notes](https://lingkurs.h.uib.no/webroot/index.php?page=python/charfreqpy&lang=en&course=ling123)***

*Disagree with my solutions, or have something to add? <br>
Leave a [comment](#post-extend-wrapper)!* <br>
<br>

---

<br>

### Exercise 1: Character frequencies with Python <br>
*Compare the
[Python implementation](https://lingkurs.h.uib.no/webroot/index.php?page=python/charfreqpy&lang=en&course=ling123)
of calculating character frequencies to the Awk implementation. What is simpler and what is more complicated?
For reference, here is an example of a shell scripting implementation of counting character frequencies with AWK:* <br>
```shell
#!/usr/bin/env bash
# Filename: charfreq.sh
awk '
  BEGIN { FS="" }

  { for (field=1; field <= NF; field++) {
  freq[tolower($field)]++ }
  }

  END { for (char in freq)
        { print freq[char], char } }' | sort -nr
```
<br>

Here's a version of the Python implementation, with my comments explaining each step:
```python
# Script name: charfreq.py

# A Python implementation of calculating character frequencies can look like this
def charfreq(string):
    # Create an empty dictionary
    freq = {}

    # for every character in the lowercased string
    for char in string.lower():
        # if that character is already in the dictionary
        if char in freq:
            # Increase count for that character by one
            freq[char] += 1
        # If we have not seen the character before
        else:
            # create a new key for that character, and set the value (count) to 1
            freq[char] = 1

    # Sort the dictionary by the value (item[1]) in the key-value pairs in the dict
    freq_sorted = dict(sorted(freq.items(), key=lambda item: item[1],
                              reverse=True))

    # Return the sorted dict
    return freq_sorted


# Call the function with a test string as input
test_string = "All work and no play makes Jack a dull boy."
print(f"Test string: {test_string}")
print(charfreq(test_string))
```

Let's run the two different implementations on the same text and compare the results.

```shell
$ python charfreq.py
Test string: All work and no play makes Jack a dull boy.
{' ': 9, 'a': 6, 'l': 5, 'o': 3, 'k': 3, 'n': 2, 'd': 2, 'y': 2, 'w': 1, 'r': 1, 'p': 1, 'm': 1, 'e': 1, 's': 1, 'j': 1, 'c': 1, 'u': 1, 'b': 1, '.': 1}
$ echo "All work and no play makes Jack a dull boy." > jack.txt
$ source charfreq.sh < jack.txt
9
6 a
5 l
3 o
3 k
2 y
2 n
2 d
1 w
1 u
1 s
1 r
1 p
1 m
1 j
1 e
1 c
1 b
1 .
```

We can clearly see that the AWK version is simpler, at least in terms of the amount of code required.
This is mainly because AWK loops through the file line by line by default, so no code is needed to achieve that effect.
However, there are
some minor differences between the two programs. The shell script takes a file as input and prints the
character frequencies with each *frequency - character* pair on its own line, while the Python script simply prints out
a dictionary object which is displayed horizontally with the character first. If we wanted the Python script to
be able to accept a file as argument, or to be used similarly as the AWK script in the terminal, we would need
even more code, increasing the complexity of the script. <br>

<br>


### Exercise 2: Using the Counter module <br>
*a) Write a function that takes a filename as an argument and prints the character frequencies as
showed in the [lecture notes](https://lingkurs.h.uib.no/webroot/index.php?page=python/charcounter&lang=en&course=ling123)
on using the Counter module.* <br>

```python
# Script name: counter-1.py
from collections import Counter


# Function to print character frequencies of an input file
def count_chars(filename):
    # Open the file, set the encoding to UTF-8 so all characters are read correctly
    with open(filename, encoding="UTF-8") as text:
        # Read the file, count the lowercased characters
        cnt = Counter(text.read().lower())
        # For every character-count pair in the dictionary, sorted by most common
        for pair in cnt.most_common():
            # If the character isn't a newline ("\n" would screw up the formatting of the output, we don't want that)
            if pair[0] != '\n':
                # Print the character, a space, and the count
                print(pair[0], ' ', pair[1])


# Call the function
count_chars('chess.txt')
```

```shell
$ python counter-1.py
    79
e   53
t   36
r   32
n   26
a   25
l   22
i   22
s   22
d   18
g   18
o   16
j   14
k   11
p   10
m   9
.   7
u   6
v   6
,   6
h   5
å   5
f   4
–   3
?   1
4   1
0   1
w   1
b   1
```


*b) Reverse the left-to-right order in which the character and frequency are printed.* <br>

Here, we simply need to change `print(pair[0], ' ', pair[1])` to `print(pair[1], ' ', pair[0])`:
```python
# Script name: counter-2.py
from collections import Counter


# Function to print character frequencies of an input file.
# Order: frequency - character)
def count_chars(filename):
    # Open the file, set the encoding to UTF-8 so all characters are read correctly
    with open(filename, encoding="UTF-8") as text:
        # Read the file, count the lowercased characters
        cnt = Counter(text.read().lower())
        # For every character-count pair in the dictionary, sorted by most common
        for pair in cnt.most_common():
            # If the character isn't a newline ("\n" would screw up the formatting of the output, we don't want that)
            if pair[0] != '\n':
                # Print the character, a space, and the count
                print(pair[1], ' ', pair[0])
# Call the function
count_chars('chess.txt')
```

```shell
$ python counter-2.py
79
53   e
36   t
32   r
26   n
25   a
22   l
22   i
22   s
18   d
18   g
16   o
14   j
11   k
10   p
9   m
7   .
6   u
6   v
6   ,
5   h
5   å
4   f
3   –
1   ?
1   4
1   0
1   w
1   b
```

<br>

*c) Print the character and its frequency only if the character is alphanumeric. Use the
[`isalnum()` method](https://www.w3schools.com/python/ref_string_isalnum.asp) to check if a word is alphanumeric.* <br>

We add an if statement (`if pair[0].isalnum() is True:`), saying that if the character is alphanumeric,
we print the character and its frequency. It's important to keep in mind that the `.isalnum()` method returns `True`
or `False`, not the string being tested. We can therefore use it directly in the conditional statement. <br>

```python
# Script name: counter-3.py
from collections import Counter


# Function for calculating alphanumeric character frequencies
def count_chars(filename):
    # Open the file, set the encoding to UTF-8 so all characters are read correctly
    with open(filename, encoding="UTF-8") as text:
        # Read the file, count the lowercased characters
        cnt = Counter(text.read().lower())
        # For every character-count pair in the dictionary, sorted by most common
        for pair in cnt.most_common():
            # If the character is an alphanumeric character
            if pair[0].isalnum() is True:
                # Print the character, a space, and the count
                print(pair[1], ' ', pair[0])


# Call the function
count_chars('chess.txt')
```

```shell
$ python counter-3.py
53   e
36   t
32   r
26   n
25   a
22   l
22   i
22   s
18   d
18   g
16   o
14   j
11   k
10   p
9   m
6   u
6   v
5   h
5   å
4   f
1   4
1   0
1   w
1   b
```

<br>


### Exercise 3: Plotting frequencies <br>
*a) Recreate the plot from the
[lecture notes](https://lingkurs.h.uib.no/webroot/index.php?page=python/freqplot&lang=en&course=ling123).
Save the plot in a file.* <br>

```python
# Script name: plotting-freq.py
import matplotlib.pyplot as plt
from collections import Counter


# Open the file and calculate the frequencies
with open('chess.txt') as text:
    cnt = Counter(text.read().lower())
    frequencies = [pair[1] for pair in cnt.most_common()]

print(f"The frequencies look like this: \n{frequencies}")

# Create a line plot of the frequencies (Index vs Frequency)
plt.plot(frequencies)

# Set the plot title and axis labels
plt.title('Character frequencies in chess.txt')
plt.ylabel('Frequency')
plt.xlabel('Index')

# Save the plot to a file
plt.savefig("chess_freq_plot.png")  # Must be called before plt.show()

# Display the plot
# plt.show()  <--- For displaying the plot in PyCharm, Jupyter Notebook or other IDE's that support displaying graphs
```

```shell
$ python plotting-freq.py
The frequencies look like this:
[79, 53, 36, 32, 26, 25, 22, 22, 22, 18, 18, 16, 14, 11, 11, 10, 9, 7, 6, 6, 6, 5, 5, 5, 4, 3, 3, 3, 1, 1, 1, 1, 1]
```

Opening the file `chess_freq_plot.png` with an image viewer like Windows Photos, we get this: <br>

![chess-freq-plot.png](/assets/img/answers-09/chess-freq-plot.png){: .normal}
_A plot of the character frequencies in chess.txt_

<br>

*b) Create a new function that takes a filename as argument and plots the character frequencies.* <br>

I tested the function on a file called `grunnloven-v1814.txt`, which is a text file containing the original version
of Norway's 1814 constitution. You can find the text
[here](https://stortinget.no/no/Stortinget-og-demokratiet/Lover-og-instrukser/Grunnloven-fra-1814/).

```python
# Script name: plotting-freq-func.py
import matplotlib.pyplot as plt
from collections import Counter

def plotfreq(filename):
    with open(filename) as text:
        count = Counter(text.read().lower())
        freqs = [pair[1] for pair in count.most_common()]

    plt.plot(freqs)
    plt.title(f'Character frequencies in {filename}')
    plt.ylabel('Frequency')
    plt.xlabel('Index')
    plt.savefig(f"{filename}-plotfreq.png")
    plt.show()


# Call the function to test
plotfreq('grunnloven-v1814.txt')
```

Run the script like normal:
```shell
$ python plotting-freq-func.py
```

And open the generated plot file: <br>

![grunnloven-v1814-plotfreq.png](/assets/img/answers-09/grunnloven-v1814-plotfreq.png){: .normal}
_Character frequencies in the Norwegian Constitution of 1814_
<br>


### Exercise 4: Sorting dictionaries
*Create a function that sorts a dictionary by value (as opposed to the keys). In addition to the input dictionary,
the function should have a parameter `reverse` which is set to `False` by default. If the `reverse` parameter is `False`,
the function should sort the dictionary in descending order. Otherwise, the function should sort the dictionary
in ascending order.* <br>

```python
# Script name: sorting-dicts.py
my_dict = {"a": 2, "b": 4, "c": 3, "d": 1, "e": 0}

print('Original dictionary : ', my_dict)
print(my_dict.items())


def sort_dict(dict_obj, reverse=False):
    if reverse is False:
        sorted_dict = sorted(dict_obj.items(), key=lambda item: item[1])
        return sorted_dict

    else:
        sorted_dict = sorted(dict_obj.items(), key=lambda item: item[1], reverse=True)
        return sorted_dict


my_sorted_dict_1 = sort_dict(my_dict)
print('Dictionary in ascending order by value : ', my_sorted_dict_1)

my_sorted_dict_2 = sort_dict(my_dict, reverse=True)
print('Dictionary in descending order by value : ', my_sorted_dict_2)
```

```shell
$ python sorting-dicts.py
Original dictionary :  {'a': 2, 'b': 4, 'c': 3, 'd': 1, 'e': 0}
dict_items([('a', 2), ('b', 4), ('c', 3), ('d', 1), ('e', 0)])
Dictionary in ascending order by value :  [('e', 0), ('d', 1), ('a', 2), ('c', 3), ('b', 4)]
Dictionary in descending order by value :  [('b', 4), ('c', 3), ('a', 2), ('d', 1), ('e', 0)]
```

<br>


### Exercise 5: Word frequencies in Python <br>
*a) Use the `Counter` module to implement word frequencies differently, as demonstrated in the
[chapter on character frequencies](https://lingkurs.h.uib.no/webroot/index.php?page=python/charfreqpy&lang=en&course=ling123).
Sort by frequency or alphabetically.* <br>

```python
# Filename: wordfreqs-1.py
from collections import Counter
from nltk.tokenize import word_tokenize

# Open the file with UTF-8 encoding
with open("chess.txt", encoding="UTF-8") as text:
    # Read every line in the file, add it to a single long string of text
    text_string = text.read()
    # Tokenize
    tokens = word_tokenize(text_string.casefold())
    # Count the nr of words
    word_count = Counter(tokens)

    # Print the word count, sorted by frequency
    print(word_count)

    # Print the word count, sorted alphabetically
    print(sorted(word_count.items()))

```

```shell
$ python wordfreqs-1.py
Counter({'.': 7, 'det': 6, ',': 6, '–': 3, 'er': 3, 'i': 3, 'til': 3, 'jeg': 3, 'var': 2, 'og': 2, 'nepomnjasjtsjij': 2, 'å': 2, 'allerede': 1, 'remis': 1, '?': 1, 'spurte': 1, 'programleder': 1, 'ole': 1, 'rolfsrud': 1, 'nrks': 1, 'studio': 1, 'spilt': 1, '40': 1, 'sekunder': 1, 'av': 1, 'partiet': 1, 'mellom': 1, 'wang': 1, 'hao': 1, 'jan': 1, 'da': 1, 'tok': 1, 'de': 1, 'hverandre': 1, 'hånden': 1, 'etter': 1, 'ha': 1, 'tatt': 1, 'tolv': 1, 'trekk': 1, 'på': 1, 'brettet': 1, 'ja': 1, 'raskt': 1, 'sier': 1, 'nrk': 1, 'gikk': 1, 'inn': 1, 'en': 1, 'kjent': 1, 'stilling': 1, 'ser': 1, 'ingen': 1, 'grunn': 1, 'fortsette': 1, 'veldig': 1, 'glad': 1, 'for': 1, 'alle': 1, 'pausene': 1, 'får': 1, 'ganske': 1, 'slitsomt': 1, 'spilles': 1, 'mange': 1, 'partier': 1, 'om': 1, 'dagen': 1, 'legger': 1, 'han': 1})
[(',', 6), ('.', 7), ('40', 1), ('?', 1), ('alle', 1), ('allerede', 1), ('av', 1), ('brettet', 1), ('da', 1), ('dagen', 1), ('de', 1), ('det', 6), ('en', 1), ('er', 3), ('etter', 1), ('for', 1), ('fortsette', 1), ('får', 1), ('ganske', 1), ('gikk', 1), ('glad', 1), ('grunn', 1), ('ha', 1), ('han', 1), ('hao', 1), ('hverandre', 1), ('hånden', 1), ('i', 3), ('ingen', 1), ('inn', 1), ('ja', 1), ('jan', 1), ('jeg', 3), ('kjent', 1), ('legger', 1), ('mange', 1), ('mellom', 1), ('nepomnjasjtsjij', 2), ('nrk', 1), ('nrks', 1), ('og', 2), ('ole', 1), ('om', 1), ('partier', 1), ('partiet', 1), ('pausene', 1), ('programleder', 1), ('på', 1), ('raskt', 1), ('remis', 1), ('rolfsrud', 1), ('sekunder', 1), ('ser', 1), ('sier', 1), ('slitsomt', 1), ('spilles', 1), ('spilt', 1), ('spurte', 1), ('stilling', 1), ('studio', 1), ('tatt', 1), ('til', 3), ('tok', 1), ('tolv', 1), ('trekk', 1), ('var', 2), ('veldig', 1), ('wang', 1), ('å', 2), ('–', 3)]
```
<br>

*b) Given a counter with word frequencies, print out the vocabulary, i.e. all word types, alphabetically sorted,
one word per line.* <br>

```python
# Script name: wordfreqs-2.py
from collections import Counter
from nltk.tokenize import word_tokenize

# Given a counter with word frequencies
with open("chess.txt", encoding="UTF-8") as text:
    word_count = Counter(word_tokenize(text.read().casefold()))

# Print out the alphabetically sorted vocabulary
for word in sorted(word_count.items()):
    print(word[0])
```

```shell
$ python wordfreqs-2.py
,
.
40
?
alle
allerede
av
brettet
da
dagen
de
det
en
er
etter
for
fortsette
får
ganske
gikk
glad
grunn
ha
han
hao
hverandre
hånden
i
ingen
inn
ja
jan
jeg
kjent
legger
mange
mellom
nepomnjasjtsjij
nrk
nrks
og
ole
om
partier
partiet
pausene
programleder
på
raskt
remis
rolfsrud
sekunder
ser
sier
slitsomt
spilles
spilt
spurte
stilling
studio
tatt
til
tok
tolv
trekk
var
veldig
wang
å
–
```
<br>


*c) Compute the word frequencies for a slightly larger text and make a plot.* <br>

```python
# Filename: wordfreqs-3.py

# Import libraries ("packages")
import pandas as pd
import matplotlib.pyplot as plt
from nltk import regexp_tokenize


# Create a new function, which takes a file and returns a DataFrame object with word frequencies
def file_word_freq(filename):
    # Open the file, set encoding to UTF-8
    with open(filename, encoding="UTF-8") as text:
        # Create a single lowercased string of the file content
        text_str_lower = text.read().casefold()

        # Tokenize the file with nltk.regexp_tokenize
        tokens = regexp_tokenize(text_str_lower, '\w+')

        # Create a dictionary with the frequencies for each word
        word_freql = {}
        for token in tokens:
            if token in word_freql.keys():
                word_freql[token] += 1
            else:
                word_freql[token] = 1

        # Sort the dictionary by frequency
        sorted_word_freql = sorted(word_freql.items(), key=lambda item: item[1], reverse=True)

        # Pandas DataFrame takes a dictionary as argument,
        # where the keys are columns and the values are values in the column. Rearrange the dictionary
        freq_data = {'Frequency': [count[1] for count in sorted_word_freql],
                     'Word': [count[0] for count in sorted_word_freql]}

        # Create a DataFrame object
        wordfreq_df = pd.DataFrame(freq_data)

        # Return the DataFrame object
        return wordfreq_df


# Call the function, create frequency list for the original Norwegian Consitution of 1814
grunnloven_wordfreq_df = file_word_freq('grunnloven-v1814.txt')

# Print the first 15 rows of the DataFrame
print(grunnloven_wordfreq_df.head(15))

# Plot the DataFrame
grunnloven_wordfreq_df.plot()
plt.title("Word Frequencies for the Norwegian Consitution of 1814")
plt.xlabel("Word index")
plt.ylabel("Frequency")

# Save and show the plot
plt.savefig('Grunnloven-v1814-wordfreq')
plt.show()
```

```shell
$ python wordfreqs-3.py
       Frequency  Word
0         152      og
1         131       i
2         114       s
3         114       l
4         100      af
5          98     til
6          88     den
7          82      at
8          78      de
9          67   eller
10         64     som
11         59      er
12         54     det
13         50  kongen
14         49     paa
```

Let's take a look at the result:
![Word frequencies of the norwegian constitution of 1814](/assets/img/answers-09/Grunnloven-v1814-wordfreq.png){: .normal}
_Word frequencies for the Norwegian constitution of 1814_

<br>


### Exercise 6: Summing dictionaries <br>
*Write a Python program to sum all the values in a dictionary.* <br>

This one can be solved with the built-in `sum()` function and the `.values()` method for dictionaries, which extracts
all of the values stored in the dictionary. <br>

```python
# sumdict.py
my_dict = {"a": 2, "b": 4, "c": 3, "d": 1, "e": 0}
print(sum(my_dict.values()))
```

```shell
$ python sumdict.py
10
```

<br>


### Exercise 7: Dictionary-based functions <br>
*Create a function `dict_top_3` which takes a dictionary as input, and returns the 3 highest values.* <br>

```python
# Script name: dict_func.py

# Create a new dictionary object to test the function.
movies = {
    "I, Robot": 7.1, "The Lord of the Rings: Return of the King": 8.9,
    "Pulp Fiction": 8.8, "The Shawshank Redemption": 9.2, "The Godfather": 9.1,
    "The Dark Knight": 9.0, "Snatch": 8.3, "12 Angry Men": 8.9, "Schindler's List": 8.9,
    "Fight Club": 8.8
}


# Function to return the 3 highest values in a dictionary
def dict_top_3(dict_obj):
    sorted_dict = sorted(dict_obj.items(), key=lambda item: item[1], reverse=True)
    return sorted_dict[:3]


print(dict_top_3(movies))
```

```shell
$ python dict_func.py
[('The Shawshank Redemption', 9.2), ('The Godfather', 9.1), ('The Dark Knight', 9.0)]
```

<br>
