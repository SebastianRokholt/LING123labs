---
title: Solutions to Exercises from Lab Session 13
date: 2021-05-23 10:22:00 +0200
categories: [Solutions to Lab Exercises]
tags: [exam, revision, python, awk, shell, scripting, levenshtein, frequency, n-grams, trigrams, html, compound, JFLAP, transition, table]
pin: true
---

The solutions to the exam prep exercises are presented below. The exercises are across different
subjects to cover what I believe is the most important stuff from this semester. As always, I recommend that you attempt
the exercises yourself before looking at the solutions. The exercises to Lab Session 13 are available in their entirety
[here](https://ling123labs.com/posts/Lab-Session-13/). <br>

Let me know if you have any questions to the exercises or anything else from the curriculum. <br>
Good luck on the exam!

*Disagree with my solutions, or have something to add? Leave a [comment](#post-extend-wrapper)!* <br>
<br>

---

<br>

### Exercise 1: Computing wages <br>
*Look at the file `wages.txt`, which contains data about the name, hourly wage and hours worked this month for 11 employees.
Write an AWK or Python script which prints out the monthly salary of each employee,
but only if the monthly salary is greater than 10,000.* <br>

```text
Arne 182 0
Marie 190 0
Vishal 213 150
Grete 192 16
Finn 192 37
Kari 220 145
Arnfinn 220 150
Torstein 250 132
Trude 196 150
Geir 160 13
Hoda 196 42
```
<br>

AWK solution:

```awk
#!/usr/bin/env awk
# Filename: high_wages.awk
{($2 * $3 > 10000)
      {print $1, $2 * $3}
}
```

```shell
$ awk -f high_wages.awk wages.txt
Vishal 31950
Kari 31900
Arnfinn 33000
Torstein 33000
Trude 29400
```


### Exercise 2: Levenshtein <br>
*a) What's the Levenshtein distance between the words "monstrous" and "miscellaneous"? Use a cost of 2 for substitutions and
a cost of 1 for insertions and deletions.* <br>

```python
# Filename: levenshtein-1.py

from functools import lru_cache

# The Levenshtein function from the lecture notes
@lru_cache(maxsize=4095)
def levenshtein_dist(string, target):
    if not string:
        return len(target)
    if not target:
        return len(string)

    if string[0] == target[0]:
        return levenshtein_dist(string[1:], target[1:])

    lins = levenshtein_dist(string, target[1:]) + 1
    lsub = levenshtein_dist(string[1:], target[1:]) + 2  # change this from 1 to 2 to calculate with higher sub cost
    ldel = levenshtein_dist(string[1:], target) + 1

    return min(lins, lsub, ldel)


print(levenshtein_dist("monstrous", "miscellaneous"))
```

```shell
$ python levenshtein-1.py
12
```


*b) Write a Python program which calculates the Levenshtein distance between two input strings, but only with insertion
and deletion operations. Is there a different result when testing with the two words from exercise `a`?* <br>

```python
# Filename: levenshtein-2.py
from functools import lru_cache


# Modified Levenshtein function with no substitution operations
@lru_cache(maxsize=4095)
def levenshtein_dist(string, target):
    if not string:
        return len(target)
    if not target:
        return len(string)

    if string[0] == target[0]:
        return levenshtein_dist(string[1:], target[1:])

    lins = levenshtein_dist(string, target[1:]) + 1
    ldel = levenshtein_dist(string[1:], target) + 1

    return min(lins, ldel)


print(levenshtein_dist("monstrous", "miscellaneous"))
```

```shell
$ python levenshtein-2.py
12
```

There is no difference. The reason is that a substitution is always equivalent to a deletion operation followed by an
insertion.
<br>


### Exercise 3: AWK <br>
*In general, what does this script do? <br>
Assume that the file `text.txt` is a file with multiple lines of text content.* <br>

```shell
awk '{sum += length($0)} END {print sum / NR}' text.txt
```

The program calculates the input file's average line length. `sum` is the name of a variable which increases iteratively
with the length of the entire line (remember: in AWK `$0` is the entire input line).
At the end of the program, it prints out the final sum (the sum of all line lenghts) divided by `NR`, which is an
AWK built-in variable that keeps track of the index for the current input row/line that is being processed.
This means that at the end of the program, `NR` is equal to the number of lines in the input file.
The sum of all line lengths divided by the number of rows/lines gives the average line length of the input file.

<br>


### Exercise 4: Frequency lists <br>
*a) Create a word frequency list of the short story
[The Last Question](https://www.multivax.com/last_question.html) by Isaac Asimov.* <br>

```shell
#!/usr/bin/env bash
# wordfreq.sh

egrep -ow '\w+' $1 |
 gawk '{print tolower($0)}' |
 sort | uniq -c | sort -r >> $1-freq-list.txt

echo "Frequency list created. Saved as '$1-freq-list.txt' "
```

```shell
$ source wordfreq.sh the-last-question.txt
Frequency list created. Saved as '../the-last-question.txt-freq-list.txt'
```

<br>

*b) How many words with twelve letters or more occur more than once?* <br>

```shell
#!/usr/bin/env bash
# twelve-letter-words-cnt.sh
# Prints the number of words in the input file which are 12 letters or longer
# and occur more than once

egrep -o '\b[[:alpha:]]{12,}\b' "$1" | tr -cs '[:alnum:]' '\n' | sort |
 uniq -ci | sort -nr | awk '$1 > 1 {print $0}' | wc -l
```

```shell
$ source twelve-letter-words-cnt.sh the-last-question.txt
5
```
There are 5 words with twelve letters or longer that occur more than once.
<br>


### Exercise 5: Python <br>
*a) Define a function `vocabulary` that takes the name of a file as its argument and returns the sorted set of tokens.* <br>

```python
from nltk import word_tokenize
from re import sub


# returns the sorted set of tokens in a file
def vocabulary(path_to_file):
    with open(path_to_file, encoding="UTF-8") as text:

        # read the content of the file and lowercase it
        content = text.read().lower()

        # tokenize the content with nltk
        tokens = word_tokenize(content)

        # Clean the tokens, remove unwanted characters, and get all unique tokens by converting from list to set
        tokens = set([sub(r"[.,'§!()-_;`:]", '', token) for token in tokens])

        # If token is not empty, add to list comprehension, then sort the list
        tokens = sorted([token for token in tokens if token != ''])

    return tokens


print(vocabulary('grunnloven-v1814.txt'))
```

You can get the file I used to test the program
[here](https://www.stortinget.no/no/Stortinget-og-demokratiet/Lover-og-instrukser/Grunnloven-fra-1814/).


*b) Define a function `vocabulary_out` that takes the names of an input file and an output file as arguments,
and writes the sorted set of tokens from the input file to the output file. It should use `vocabulary`.* <br>

```python
# Tokenize an input file and write to a specified output file by using the vocabulary function
# NB! The code for 5 a)  must be executed first for this code segment to work.


def vocabulary_out(path_to_input_file, path_for_output_file):

    # Get sorted list of tokens from input file
    tokens = vocabulary(path_to_input_file)

    # Create a new file and write the tokens to it
    with open(path_for_output_file, mode='w', encoding="UTF-8") as output:
        for token in tokens:
            output.write(f'{token}\n')


vocabulary_out('grunnloven-v1814.txt', 'grunnloven-v1814-vocab.txt')
```

The first 20 lines of the file `grunnloven-v1814-vocab.txt` should look something like this:
```text
a
aabenbar
aabne
aabner
aabnes
aall
aand
aar
aarlig
aarligen
aars
aarsag
aasædesretten
addresse
adgang
adlyder
adskilles
af
afdelinger
affattede
```

<br>


### Exercise 6: Character frequencies <br>
*What is the least frequently used letter in the file
[cuerpo.txt](https://lingkurs.h.uib.no/webroot/static/cuerpo.txt)?* <br>

We can modify the shell script
[`charfreq.sh`](https://lingkurs.h.uib.no/webroot/index.php?page=scripting/charfreq&lang=en&course=ling123)
from the lectures on shell scripting to get the least frequently used
letter. Since `charfreq.sh` returns a frequency list of all *characters* sorted descendingly by frequency, we need
to remove the `-r` flag from the `sort` command at the end of the script so that it returns a list which is sorted
ascendingly instead. Since we only want to count alphanumeric characters, a.k.a the letters, we also need to run the
file through a `sed` command in the beginning. Removing all non-alphanumeric characters can be achieved with
`sed 's/[^a-zA-Z0-9]//g'`. Finally, we can
use the `head` command with the flag `-1` when running the script to only print the first line,
which contains our answer.

```shell
#!/usr/bin/env bash
# Filename: letterfreq.sh

# Remove non-alphanumeric characters from the file
sed 's/[^a-zA-Z0-9]//g' |

# Get the character frequencies and sort ascendingly
awk '
  BEGIN { FS="" }

  { for (field=1; field <= NF; field++) {
  freq[tolower($field)]++ }
  }

  END { for (char in freq)
            { print freq[char], char }
}' | sort -n
```

```shell
$ source letterfreq.sh < cuerpo.txt | head -1
3 w
```

The answer is the letter "w", which has been used only 3 times.

<br>


### Exercise 7: Extracting words from HTML <br>
*Create a list of all unique [compound words](https://guidetogrammar.org/grammar/compounds.htm)
(hyphenated or closed form) containing the word "korona" or "corona" (case-insensitive) by
searching in [Norwegian newspapers on the web](http://avis.uib.no/). Some examples are "koronafast",
"pre-korona-times" and "koronaknekken". Which five compound words are most commonly used?
Collect data from both 2020 and 2021.* <br>

Perform a case-insensitive search in the korpus `Aviskorpus, bokmål, 01.01.21 --->`
with a regular expression of your choosing. We want to match words that contain "corona" or "korona" in the beginning,
end and at the middle of the compound word (with and without hyphens).
However, we don't want our expression to match the word "korona" or "corona" alone without text on either side.
It is perhaps easiest to achieve this by using the regex "OR" operator `|`, though there are other ways to do it as well.
Another way would be to perform multiple searches (one with `[ck]orona` in the center, one preceding and one following)
and concatenate the results. Anyway, I solved it by searching for `([ck]orona..*|..*[ck]orona|..*[ck]orona..*)`.
Feel free to expand the number of matches by increasing the option
`Antall sider per linje` to 10,000+, though this might take a while to run. Save the page with results to a file by
right-clicking and selecting `Save Page As...` and specifying the type (HTML only), filename and location. Repeat the process
with the `År` option set to 2020 (keep searching in the same korpus as previously) and concatenate the two files
in the shell with the command `cat filename1.html filename2.html > corona_compounds_2020-2021.html`.

We can then get all corona-related compund words we searched for by using `egrep '<b>' corona_compounds_2020-2021.html
| sed -E 's/.*<b>(.*)</b>.*/\1/'` to retrieve the words between the bold tags (`<b>`, `</b>`) in the file.
The results from grep can be piped to `sort` and `uniq` to produce only the unique words. Finally, the results can be
saved to a new file with `> filename.txt`. <br>

The full (shell) solution:

```shell
$ cat cqp-avis-korona-10000_2021.htm cqp-avis-korona-10000_2020.htm > corona_compounds_2020-2021.html
$ egrep '<b>' corona_compounds_2020-2021.html | sed -E 's!.*<b>(.*)</b>.*!\1!' | sort | uniq > corona_compounds_2020-2021.txt
$ head -20 corona_compounds_2020-2021.txt
ArbeidslivKoronaLO
BarcelonaKoronavirus
betacoronavirus
BorgenKoronavirus
BorgenVaksineKoronavirus
corona-
coronaanbefalinger
coronaansvarlig
corona-åpne
coronaår
coronaåret
corona-året
Coronaåret
CORONA-ÅRET
coronaårsak
coronaavdeling
corona-avdeling
corona-avdelinger
corona-avlysningene
coronaavlyst
```

I haven't tried the Python implementation of this, but the steps should be mostly
the same. Let me know if you run into problems!

<br>


### Exercise 8: Finite State Automata <br>
*a) Use JFLAP to create an FSA for the regular expression `a+h?rgh+`.* <br>

![JFLAP Finite State Automata](/assets/img/answers-13/aaaaargh-JFLAP.png){: .normal}

*b) Create a transition table for your FSA.* <br>

![Aaargh transition table](/assets/img/answers-13/transition-table-aaargh.png){: .normal}

<br>


### Exercise 9: Trigrams <br>
*Write a Python, shell or AWK script which takes a text file as input and:* <br>
*1) Creates a new file with all trigrams (and only the trigrams)* <br>
*2) Prints out the longest trigram (as determined by the number of letters in each trigram)* <br>

```shell
#!/usr/bin/env bash
# Filename: get-trigrams.sh
# Creates a new file with all trigrams and prints out the longest trigram

# First, tokenize the file
tr '[:upper:]' '[:lower:]' < "$1" | tr -cs '[a-záéóíúñ]' '\n' | egrep -v '^$' > "$1-tokens"

# Then create a new file with the next and next-next tokens
tail -n+2 "$1-tokens" > "$1-next"
tail -n+3 "$1-tokens" > "$1-next-next"

# Create a new file with the trigrams
paste "$1-tokens" "$1-next" "$1-next-next" > "$1-trigrams"

# Delete the last three lines, they aren't trigrams
sed -i '$d' "$1-trigrams"
sed -i '$d' "$1-trigrams"
sed -i '$d' "$1-trigrams"


# Print out the longest trigram(s) with awk
awk '
BEGIN {FS = "\t"}    # Set field seperator to tab

# Calculate the length of each line, store to var "len"
{len = length($0)}

# If current len is greater than the max len, delete the array of results and set new max
len > max {delete result; max = len}

# If current len is equal to the longest len so far, add it to the array of results
len == max {result[NR] = $0}

# Finish by printing out the array of results.
END {for (i in result) printf("%s chars: %s \n", length(result[i]), result[i])}
' "$1-trigrams"
```

```shell
$ source get-trigrams.sh the-last-question.txt
collecting      interstellar    hydrogen
$ head the-last-question.txt-trigrams
the     last    question
last    question        by
question        by      isaac
by      isaac   asimov
isaac   asimov  the
asimov  the     last
the     last    question
last    question        was
question        was     asked
was     asked   for
```

<br>

---


And that's it! If you have solved both these and the previous lab exercises,
I think you should be well-prepared for the exam.
Let me know if you have any more questions. Good luck!
