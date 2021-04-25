---
title: Solutions to Exercises from Lab Session 07
date: 2021-04-24 19:16:00 +0200
categories: [Solutions to Lab Exercises]
tags: [python, lists, dictionaries, tuples, slice, range, palindromes, nltk, functions, methods, regex, regexp, tokenization, dictionary, list, tuple]
pin: false
---

Disclaimer: You should always attempt to solve the lab exercises by yourself before looking at the proposed solutions
below.
The exercises to Lab Session 07 are available [here](https://ling123labs.com/posts/Lab-Session-07/). <br>
> ***[Link to relevant lecture notes](https://lingkurs.h.uib.no/webroot/index.php?page=python&lang=en&course=ling123)***

*Disagree with my solutions, or have something to add? <br>
Leave a [comment](#post-extend-wrapper)!* <br>

---


### Exercise 2: Some Python data types<br>
*Look at the examples from the
[lecture notes](https://lingkurs.h.uib.no/webroot/index.php?page=python/python-data&lang=en&course=ling123). <br>
a) Write an expression that uses `fruits` and returns 'apricot'.* <br>

We can run Python in "Interactive Mode" in the terminal by executing the command
`python` or `python3` with no arguments. We then run a sequence of commands, like this:
like this:
```python
>>> fruits = ('apple', 'apricot', 'blackberry', 'blueberry', 'apple')
>>> fruits[1]
'apricot'
```

Keep in mind that when we use interactive mode, the executed code and the output
is deleted from memory upon closing it. If we want to write some code which will stick around,
we can use a code editor to write a Python script/program, which we can then run
in the terminal.

```python
# fruits.py
fruits = ('apple', 'apricot', 'blackberry', 'blueberry', 'apple')
print(fruits[1])
```

we can then run `fruits.py` like this:
```shell
$ python fruits.py
apricot
```
Alternatively, if we have multiple versions of Python installed and would like
to use Python 3, we can run:

```shell
$ python3 fruits.py
apricot
```

When writing a Python script in the Pycharm IDE (Integrated Development Environment, i.e a code editor ++),
we can simply run the script by clicking on "Run" in the toolbar at the top, then selecting "run fruits.py"
or simply pressing `ctrl + shift + F10` (or, if you've already run the file at least once, you can just press
`shift + F10`). The Spyder and VSCode code editors also have similar shortcuts, though the
buttons you need to press may change. You'll have to check your IDE's documentation. <br>
<br>


*b) Add another key and value to the dict `movies`.* <br>
```python
# movies.py
movies = {'7 Samurai': 'Akira Kurosawa', '2001 A Space Odyssey': 'Stanley Kubrick'}
movies['Snatch'] = 'Guy Ritchie'
print(movies)
```

```shell
$ python movies.py
{'7 Samurai': 'Akira Kurosawa', '2001 A Space Odyssey': 'Stanley Kubrick', 'Snatch': 'Guy Ritchie'}
```
<br>


*c) Write an expression that uses `movies` and returns '`brick`'.* <br>
```python
# movies.py
movies = {'7 Samurai': 'Akira Kurosawa', '2001 A Space Odyssey': 'Stanley Kubrick'}
movies['Snatch'] = 'Guy Ritchie'

print(movies['2001 A Space Odyssey'][10:])
```

```shell
$ python movies.py
brick
```

<br>


### Exercise 3: Functions and methods
*Define and test your own function.* <br>

```python
# my_function.py


# Create a new function
def chatty_bot():
        # Print some text on the terminal and take an input from the user
        name = str(input('Hi there! What is your name?\n > '))
        # Print a reply
        print(f'Well hello there, {name}!')

        # Take a second input
        story = str(input("So what's your story? You can tell me anything! I promise I won't tell Google.\n > "))

        # Print a reply based on some conditions. If the length of the story is greater than 0:
        while len(story) == 0:
                # Keep trying to take another input until the input given has some content
                story = str(input("Try again. What's your story? \n > "))

        # If the length of the input is greater than 30 characters
        if len(story) > 30:
                # Print something
                print("Uhm, I didn't need your life story, but cool story bro. Bye!")

        # If the length of the input is less or equal to 30, but not 0:
        elif (len(story) > 0) and (len(story) <= 30):
                # Print something else
                print("Huh, okay! Well, I need to go, see you later!")


# Call our function
chatty_bot()
```

Running the program with our function:

```shell
$ python my_function.py
Hi there! What is your name?
 > Sebastian

Well hello there, Sebastian!
So what's up? You can tell me anything! I promise I won't tell Google.
 >

Excuse me, I didn't catch that. What's going on?
 > Counting coins on the counter of the 7/11
  From a quarter past six 'til a quarter to seven
  The manager, Bevin, starts to abuse me
  "Hey man, I just want some muesli"

Huh, okay! Well, I need to go, see you later!
```


<br>


### Exercise 4: Regular expressions and case folding in Python
*a) Write an expression that removes vowels `[aeiouy]` from a string.* <br>

```python
# remove_vowels.py
import re

string = "Help! I'm stuck in the paste"

# remove vowels from a string
substituted = re.sub('[aeiouy]', '', string)
print(substituted)
```

```shell
$ python remove_vowels.py
Hlp! I'm stck n th pst
```
<br>


*b) Write an expression that removes vowels only at the end of the string.*<br>

```python
# remove_vowels_end.py
import re

string = "Help! I'm stuck in the paste"

# remove vowels from the end of a string
end_substituted = re.sub(r'[aeiouy]', '', string[-1:])
print(string[:-1] + end_substituted)
```

```shell
$ python remove_vowels_end.py
Help! I'm stuck in the past
```

<br>


*c) Apply the method `upper()` to a string. Also apply the method `split()` to split the string at whitespace.*<br>

```python
# upper_split.py

string = "Help! I'm stuck in the paste"

# remove vowels from the end of a string
split_upper = string.upper().split(' ')
print(split_upper)
```

```shell
$ python upper_split.py
['HELP!', "I'M", 'STUCK', 'IN', 'THE', 'PASTE']
```

<br>


### Exercise 5: List comprehensions
```python
>>> sentence = 'Anna moved to Oslo'
>>> vowels = 'AEIOUYaeiouy'
>>> upper_vowels = [v.upper() for v in sentence if v in vowels]
```

*Use the `len` function to count the vowels in `upper_vowels`*. <br>

```python
>>> len(upper_vowels)
7
```

<br>


### Exercise 6: Ranges and Slices
*a) Take a slice of a list containing integers in the range from -5 to 5. The slice should start
at index position 7 and end at index position 2. Double each element in the slice.* <br>

```python
# range_slice.py

numbers = [*range(-5, 5)]
# OR
numbers = [num for num in range(-5, 5)]
# OR
numbers = []
for num in range(-5, 5):
    numbers.append(num)


# Solution 1: With list comprehension
multiplied_slice = [num * 2 for num in numbers[7:2:-1]]
print(multiplied_slice)


# Solution 2: With for loop
sliced = numbers[7:2:-1]
multiplied_slice = []
for num in sliced:
    multiplied_slice.append(num * 2)

print(multiplied_slice)
```

```shell
$ python range_slice.py
[4, 2, 0, -2, -4]
[4, 2, 0, -2, -4]
```

<br>



### Exercise 7: Palindromes with Python
*a) Look at the [lecture notes](https://lingkurs.h.uib.no/webroot/index.php?page=python/palindromes-py&lang=en&course=ling123).
Try the program that prints the palindromes from a test file containing some lines that are palindromes
and some lines that aren't.* <br>

First, we need to create a new text file `testpalindromes.txt` to test the Python program:

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

We can then create a new Python script to test the `is_palindrome()` function from the lecture.
```python
# palindromes.py
import re


# Checks if a given input string is a palindrome. Returns True/False
def is_palindrome(string):
    return string == string[::-1]


# Open the file with the 'with' command, and specify what we want to call the file as
with open('testpalindromes.txt') as text:
    # Until we say otherwise, loop forever
    while True:
        # Read every line in the file and strip the newline character ("\n")
        line = text.readline().strip()

        # If there are no more new lines in the file, we are at the end.
        if not line:
            # Break out of the while loop
            break

        # If we are not at the end of the file, check if the word (line) is a palindrome
        elif is_palindrome(line):
            # If yes, then print the word (line)
            print(line)
```

We can then run our Python script to see the output.
```shell
$ python palindromes.py
0202-2020
radar
level
regninger
step on no pets
madam
```
As can be seen above, the program only prints out the words in the text file which are palindromes. <br>

<br>


*b) Define a function `palindromes` that takes the name of a file as an argument and prints the lines in that file
which are palindromes.*<br>

Here, we just need to take the code from the previous exercise and add it to a new function called `palindromes()`.
In order to improve readability and reduce the amount of code, I decided to merge the `is_palindromes()` function
with `palindromes()` by adding the code `elif line == line[::-1]:` to check if a word is a palindrome. <br>

```python
# palindromes_function.py
def palindromes(filename):
    with open(filename) as text:
        # Until we say otherwise, loop forever
        while True:
            line = text.readline().strip()  # strip newline

            # If there are no more new lines in the file, we are at the end.
            if not line:
                # Break out of the while loop
                break

            # If we are not at the end of the file, check if the word (line) is a palindrome
            elif line == line[::-1]:
                # If yes, then print the word (line)
                print(line)


palindromes('testpalindromes.txt')
```

Run the script and check that the output is correct.

```shell
$ python palindromes_function.py
0202-2020
radar
level
regninger
step on no pets
madam
```

<br>



*c) The above only selects strict palindromes. Make a version which disregards non-alphanumeric characters.
Do this by filtering out all non-alphanumerical characters before the comparison.
Also disregard case differences with the `casefold` method.*<br>

```python
# palindromes_robust.py


# Update the function which checks if a string is a palindrome
def is_palindrome(string):
    # substitute non-word characters with empty string, then convert to lower/casefold
    string = re.sub(r'\W', '', string).casefold()

    # Check if the normalized string is a palindrome: If yes, return True
    return string == string[::-1]


# Run the palindromes function on the test file again
def palindromes(filename):
    with open(filename) as text:
        # Until we say otherwise, loop forever
        while True:
            line = text.readline().strip()  # strip newline

            # If there are no more new lines in the file, we are at the end.
            if not line:
                # Break out of the while loop
                break

            # If we are not at the end of the file, check if the word (line) is a palindrome
            elif is_palindrome(line):
                # If yes, then print the word (line)
                print(line)


palindromes('testpalindromes.txt')
```


```shell
$ python palindromes_robust.py
02.02.2020
0202-2020
radar
level
regninger
step on no pets
Anne var i Ravenna.
Agnes i senga
Wow Bob, WOW!
Bad dab!
madam
```

<br>



### Exercise 8: IP address formatting
*Write a Python program that removes leading zeros after the punctuation marks in an IP *address. <br>
This is an IP address: `216.08.094.196`.* <br>

```python
# reformat_IP.py
import re

# Reformatting IP addresses with regex substitution from the re module
IP = "216.08.094.196"
string = re.sub('\.[0]*', '.', IP)
print(string)
```

```shell
$ python reformat_IP.py
216.8.94.196
```

<br>


### Exercise 9: Extracting numbers
*Write a Python program to extract and print any numbers present in a given input string. <br>
If the string is: `"jeg fikk 100 kroner ekstra, så da satt jeg igjen med 1500.- totalt."`,
then the output of the program should be*:
```
100
1500
```

You can extract digits from text using the `split()` function from the `re` module, like this: <br>

```python
# get_digits.py
import re

# Extract digits and add them to a variable (a list)
text = "Ten 10, Twenty 20, Thirty 30"
result = re.split("\D+", text)

# Print results
for element in result:
    print(element)
```

```shell
$ python get_digits.py

10
20
30
```


<br>

### Exercise 10: Extracting quote values from a string
*Write a Python program which extracts values between the quotation marks of a string.* <br>

```python
# extract_quotes.py

# You can write a multi-line string by using three single quotes, add line breaks with the Enter key
text = '''
Avi:    Tony?
Bullet Tooth Tony:  What?
Avi:    Look in the dog.
Bullet Tooth Tony:  What do you mean "look in the dog?"
Avi:    I mean open him up.
Bullet Tooth Tony:  It's not as if it's a tin of baked beans!
                    What do you mean "open him up"?
'''

# Print words occurring between double quotes in the text
print(re.findall(r'"(.*)"', text))
```

```shell
$ python extract_quotes.py
['look in the dog?', 'open him up']
```

<br>



### Exercise 11: Extracting names from a text
*Write a Python function that takes a string as input,
and returns all unique capitalized words in the string which are not
[stopwords](https://en.wikipedia.org/wiki/Stop_word).
You may use `stopwords` from the `nltk` package to achieve this. <br>
So if the input is "When I lived in England I had a cat named Tom, and a cow named Darcy"
we want the output to be a list or tuple containing `England, Tom, Darcy`.*

Named Entity Recognition is its own subfield of Natural Language Processing (a.k.a Computational Linguistics),
because extracting names from a text is
a notoriously difficult task. I don't expect you to make use of any advanced techniques or tools here, so your solutions
are fine as long as you extract the words that start with an upper case letter. <br>

For example, you can solve this one by extracting all uppercase words with `re.findall(r"\b[A-Z][a-z]*", string)`,
importing the English `stopwords` library from NLTK and then removing all stopwords from the final result: <br>

```python
# extract_names.py
import re

text_1 = '''

Avi:    Yes, London, you know: fish, chips, cup o' tea, bad food,
        worse weather, Mary fuckin' Poppins, London!

'''

text_2 = '''
When I lived in England I had a cat named Tom, and a cow named Darcy.
'''


def extract_names(string):
    from nltk.corpus import stopwords

    stop_words = stopwords.words('english')
    # print(sorted(stop_words))
    capitalized = set(re.findall(r"\b[A-Z][a-z]*", string))

    named_entities = []
    for word in capitalized:
        if word.lower() not in stop_words:
            named_entities.append(word)

    return named_entities


print(extract_names(text_2))
print(extract_names(text_1))
```

```shell
$ python extract_names.py
['Tom', 'England', 'Darcy']
['Mary', 'Yes', 'London', 'Poppins', 'Avi']
```


<br>
