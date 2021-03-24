---
title: Solutions to the exercises from Lab Session 01
date: 2021-02-08 22:00:00 +0100
categories: [Solutions to Lab Exercises]
tags: [corpora, corpus, language-data, norgramtall, corpuscle, antconc, shell, setup, wsl, ubuntu, bash, wc, vim, nano, emacs, zsh]
pin: false
---

Try to solve the lab exercises by yourself before you view the solutions! <br>
You can find the exercises to Lab Session 01 [here](https://ling123labs.com/posts/Lab-Session-01/). <br>
> ***[Lecture notes](https://lingkurs.h.uib.no/webroot/index.php?page=nlpintro&lang=en&course=ling123)***

*Disagree with my answers, or have something to add? <br>
Leave a [comment](#post-extend-wrapper)!*

<br>

---


### Exercise 1.1: Computers in the study of language <br>
*a) Which areas of language study might benefit from computational data and tools?* <br>

As most areas of language study benefit greatly from computational data and tools, there's not really an exhaustive list
which can answer this question definitely.
Linguistics has become extremely popular in the past decade due to its ability to produce impressive
applications for speech recognition, language translation, sentiment analysis and much, much more. Most of these
applications are heavily reliant on large amounts of training data, which are retrieved and processed using computational
tools. Language data is often scraped from the web or otherwise supplied by information systems. Recent advances in speech
recognition technology have even made studying spoken language much easier by removing transcription work almost entirely.
This has in turn vastly increased the size and variety of natural language datasets we get to work with. <br>

Other, more traditional domains within linguistics use many processing tools to analyse modern written
language. Historical linguistics make use of computational tools to compare modern language to older texts.
Morphology and phonetics benefit greatly from methods such as tokenization, and they can
analyse the rules of language in the context of regular languages and finite state automata. Semantics and pragmatics
can use language processing tools to filter written (or spoken) texts by their context or keywords, and can even use
these tools to compare the variations in context for the same words. <br>

I'm sure you can find many other domains within language study which might benefit. The point is, modern language
researchers need to be fluent in language processing tools, and there is great value in knowing how to utilize
the vast amounts of language data that exist in digital form.
<br> <br>

*b) Think of other applications for corpora besides machine translation.* <br>
As mentioned above, corpora can be used as training data for a wide array of different
natural language processing applications. They can also be used for statistical analysis in any field of
language research. Lexicographers (the people who write dictionaries) have known for a long time that the best way to
avoid missing things is to have a big corpus and a powerful computer. Other examples of applications are
using concordance examples from corpora to teach a language or testing a hypothesis of how language is used.
Corpora can also reveal instances of very rare or exceptional use cases that we wouldn't get by looking at single
texts or through introspection.
<br><br>


### Exercise 1.2: Corpora <br>
*a) Think of linguistically relevant expressions to search for.* <br>
Many people use search engines to investigate correct grammatical use of words or phrases. For example,
one might search *recurring* or *reoccurring* to decide which one is appropriate. <br>

As language researchers, we can therefore use
search engines to discover which words or phrases are difficult for people to use correctly. One way of doing this is
by looking at the [*Google Trends*](https://trends.google.com/trends/?geo=US) tool. <br>

One interesting example of how Google Trends can be used is to look at difficult terms used by politicians after a
debate, or after an important news event such as a natural disaster or a climate change summit.
Which words do people need to google? How can we explain the results?
We can also compare different terms, such as different spellings of the same word, or compare different countries' most
popular terms.

![example usages of Google Trends](/assets/img/google-trends.png){: .normal}
_Exploring the query "how to spell" with Google Trends_

<br><br>

*b) Look at the <a href="https://norgramtall.w.uib.no">NorGramTall</a> blog,
which uses a corpus to find out preferences in Norwegian.* <br>
...
<br><br>


*c) What are some limitations of using the web as a corpus?* <br>
 - The Web is dynamic, so researchers have little control over precision and recall of search results.
   Results may be very difficult, if not possible, to replicate.
 - Web data may be incorrect, or may not represent the general population.
 - Reliable metadata may be lacking.
 - it is sometimes challenging to track down and verify sources.
 - Web data is usually reliant on third parties.
 - Using language data from the web require technical skills, such as web scraping or programming in query languages.
<br><br><br>


### Exercise 1.3: Searching in corpora <br>
*a) Look for all words starting with "child" by means of the regular expression `child.*` in an English
corpus, for instance, in the corpus "Child Rights" in <a href="https://clarino.uib.no/korpuskel/page">Corpuscle</a>.
Make a word list. Find <a href="https://www.englishclub.com/vocabulary/collocations.htm">collocations</a>.
Find their distribution relative to country.* <br>

> Tip: You need to log in with *Feide* in order to use Corpuscle.
> At [Corpuscle](https://clarino.uib.no/korpuskel/page), click "CLARIN SPF" at the top, search for "Feide", and log in.
> You can then navigate to the Corpus list and select the "Child's Rights" corpus. Select "Metadata" from the menu on the left-hand side and click 'Accept'.
> You are now able to query the corpus by selecting "Query" from the menu on the left.
> Navigate creating queries, searching for collocations and creating distributions through the menu.


**Word list:**
![Creating a world list in Corpuscle](/assets/img/corpuscle-1.png){: .normal}
_Creating a world list in Clarino Corpuscle_

> To search with the regular expression `child.*` in Corpuscle, enter the query `child*` without the punctuation mark
> in basic search or the query `"child.*` in advanced search.

**Collocations:**
![Collocations in Corpuscle](/assets/img/corpuscle-2.png){: .normal}
_Looking for collocations in Clarino Corpuscle_

> Click the "Show collocations"-button to view the results.
> Sort the results by frequency by selecting "Frequency" from the drop down list between `sorted by` and `Download`.
> Tick the box with "show freq." to view the collocation frequencies.

**Distribution of collocations relative to country:**
![The distribution of collocations relative to country](/assets/img/corpuscle-3.png){: .normal}
_The distribution of collocations relative to country_

> Click "Run Query" to run the query again.
> Show the distribution of word relative to country by selecting "word" and "country" from the drop down lists.
> Leave the "group by" and "and" fields empty.
> Tick the box named "counts only" to only view the count for each word in the result.
> You can also explore collocations by selecting a positive or negative offset for the word, by selecting a number from
> the dropdown lists next to the triangle (delta) on the "of"-row.

<br><br>


*b) Try out <a href="https://www.laurenceanthony.net/software/antconc/">Antconc</a>, a tool with which you can
analyze your own corpus.* <br>
...
> When using Antconc, I suggest starting with a .txt-file such as
> [lofoten.txt](https://lingkurs.h.uib.no/webroot/static/lofoten.txt)

<br><br>


### Exercise 1.4: The trouble with language <br>
*a) What happens if a ligature (ﬄ) is considered different from its component letters?* <br>
The combination of the letters "f", "f", and "l" usually need to be represented with a unique Unicode character.
Otherwise, it might be hard to read. The problem is therefore that two words that both contain "ffl"
might be represented differently. This especially has implications for search, but can also create some
headaches for language processing in general (i.e. string comparisons).
<br><br>

*b) Which different kinds of knowledge are necessary to understand language? How much "common sense" and knowledge
about the world is necessary?* <br>

> Disclaimer: I will try my best to avoid entering philosophical territory here, but keep in mind that these questions have been -
> and still are - quite heavily debated. So it goes without saying that a lot of what follows is opinion.
> You should do some critical thinking on your own, and figure out for yourself what you think is the correct answer.

Most linguists believe that knowledge about grammar is necessary to speak a language, at least in any efficient sense.
I will leave it up to you to decide on whether you think that this knowledge is
[innate or not](https://plato.stanford.edu/entries/innateness-language/).

Language is a medium through which humans pass on most of our knowledge.
Language shapes the way we think about the world. Rhetoric sways opinions.<br>
Any modern cognitive scientist would (probably) argue that learning language is an important part of learning about
the world, and one could also take the view that human language and knowledge are intricately related in such a sense that they are difficult to
separate. Each word is bound to a concept, a "thing" we all have some common understanding of. One could therefore state that
common understanding is key to communication in general, because understanding a word on an individual level doesn't get you very far.
Arguably, understanding of a word arises when we are able to relate the word to the same concept as everyone else
does when they hear that word.
Additionally, we can consider that true masters of a language also realize that different groups of the speaking
population understand the same words differently, unrelated to context. An example is the word *"ping"*.
So therefore, one can say that both linguistic and social knowledge is necessary to understand and use language.  <br>

Many words and phrases mean different things based on the context they are used in, and interpreting the different meanings of a
word is a (somewhat) unsolved challenge in natural language processing. Even more so for languages such as Norwegian,
which has much fewer words than English and to a greater extent relies on using the same words in different context to
convey different meanings. So one might argue that knowledge about human culture and social life, humour, and different technical domains is needed to
understand how the same word can be used in different contexts. After all, we have all experienced how a
difference in situational awareness can cause communication difficulties (such as misinterpretation). <br>

On the other hand, [recent advances in AI](https://blogs.nvidia.com/blog/2018/08/27/nlp-deep-learning/) have shown promising
results through the use of very complex neural networks which seem to interpret words with many meanings.
And chatbots "understand" more and more about what humans want from them by serving up increasingly accurate answers to
requests given in the form of natural language. So can we not say that a chatbot "understands" what we mean when we ask it
"When does McDonald's close" or "Where can I buy a PS5" and it gives us the correct answer we were looking for? <br>

Some will argue that computers and computer programs don't "know" anything. After all,
software is just a bunch of ones and zeros, computers don't "understand" anything on a "deep" level.
Yet we now have chatbots which seem to do quite well in domain-specific language interpretation tasks. We have software
applications that can do sentiment analysis to figure out if a person is angry or sad based on a tweet they wrote.
We have knowledge graphs which can link related concepts and explain how different things all fit together. A car in
computer memory is no longer "just" a string of 1's and 0's, a car is related to a truck, and a car has four wheels,
and you need a licence to drive one.
So is this still a case where the software understands what we are telling them? Or should we stick to the
[Chinese Room](https://en.wikipedia.org/wiki/Chinese_room) argument and state that they are only matching an input to
the correct output? In the future, the answers to these questions might redefine how we look at the requirements for
"understanding" natural language.
<br>

What do you think?
There are many great books and articles on the subject. Ask Koenraad for a few suggestions if you are curious.
I have a few, but linguistics is a divisive field and I am not a linguist, so I will refrain from doing so
here... :)

<br>
<br>

*c) Some characters may be hard to distinguish, for instance different dashes, which could be a hyphen, minus sign,
etc. `(‐ - ⁃ －)` and different characters similar to apostrophe `(' ʼ ′ ʹ)`. Try to find a way to examine if they are
different, or if they are the same character in various fonts.* <br>

You can use [Unicode Character Search](https://www.fileformat.info/info/unicode/char/search.htm?q=-&preview=entity)
to figure this out. Insert each character and see which code it belongs to, then compare the codes to see which ones
are actually different. <br>
<br> <br>

---

### Exercise 2.1: Getting started with shell scripting <br>
*a) Check your locale with the locale command. Change the locale to a different language and type date.* <br>
![Setting the locale in Ubuntu bash](/assets/img/change-locale.png){: .normal}
> Restart the terminal for the changes to take effect.

![Setting the locale in Ubuntu bash](/assets/img/locale-after-restart.png){: .normal}

<br><br>


#### Exercise 2.2: Additional exercises for basic shell usage and word counting <br>

*a) Acquaint yourself with the command-line, try navigating directories, viewing files, etc. Try out some various
<a href="https://lingkurs.h.uib.no/webroot/index.php?page=unix&lang=en&course=ling123">commands</a>.* <br>
...
<br><br>


*b) Try editing a file using a terminal text editor such as <a href="https://vim.fandom.com/wiki/Vim_Tips_Wiki">Vim</a>,
<a href="https://www.nano-editor.org/">Nano</a>, or <a href="https://www.gnu.org/software/emacs/">Emacs</a>.
What are the benefits of using one of these instead of an IDE like Atom or Pycharm? What are the drawbacks?* <br>
No definitive answer here either, but the general idea is that we have to make a trade-off between increased
functionality/a nice interface and simplicity/efficiency. <br>

Editing files in the terminal is quick and easy for simple stuff, but there is a significant lack of functionality
compared to an IDE such as `VScode`. However, it is still very useful to learn how to code in a terminal editor, because
you often save time and effort by quickly writing some code in the terminal instead of having to open an external program.
So unless you're writing longer scripts, I recommend simply getting used to a terminal editor such as `nano`.  <br>
<br><br>


#### Exercise 2.3: Counting lines and words<br>
*a) The `wc` command can take more than one file as arguments. Test something like the following which also uses a text
file <a href="https://lingkurs.h.uib.no/webroot/static/lofoten.txt">lofoten.txt</a>:* <br>
```shell
wc chess.txt lofoten.txt
```

![Word counting with two files](/assets/img/word-count-two-files.png){: .normal}

<br><br>



#### Exercise 2.4: Word counting on the Web<br>
*a) Find an article on the web. Copy the content to a new file in a terminal text editor (`vim`, `nano`, `emacs`). <br>
If `Ctrl+c` and `Ctrl+v` (`Cmd` instead of `ctrl` for Mac) doesn't work, try `Ctrl+c` and `right click` instead.* <br>

1. Find an article on the web. For example, you can use [The Gutenberg Project]() to download free books in plain text
   (UTF-8).
   ![The Gutenberg Project](/assets/img/gutenberg-project.png){: .normal}
2. Copy the text directly into a text editor, such as `nano`. It might take a few moments if it is a large amount of text.
3. Save the file. In nano, press `ctrl + x` / `cmd + x`, then press `y` if prompted to save buffer. Then enter the name
of the new file and press `Enter`.

<br><br>



*c) Use a pattern of your choosing, and find the number of occurrences with the `wc` command.* <br>

![Counting words in a file](/assets/img/word-count-on-the-web.png){: .normal}

<br>
