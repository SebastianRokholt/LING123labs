---
title: Solutions to Exercises from Lab Session 12
date: 2021-04-29 20:40:00 +0200
categories: [Solutions to Lab Exercises]
tags: [xslt, xsltproc, xml, xsl, html, regex, regular, expression, cheat, sheet, JFLAP]
pin: true
---
<br>

Disclaimer: You should always attempt to solve the lab exercises by yourself before looking at the proposed solutions
below.
The exercises to Lab Session 12 are available in their entirety [here](https://ling123labs.com/posts/Lab-Session-12/). <br>
> ***[Link to relevant lecture notes](https://lingkurs.h.uib.no/webroot/index.php?page=xml/sonnet&lang=en&course=ling123)***

*Disagree with my solutions, or have something to add? <br>
Leave a [comment](#post-extend-wrapper)!* <br>
<br>

---

<br>

### Exercise 1: Creating a wishlist for books with XSLT <br>
*Consider the following XML document describing a catalog of books:*

```xml
<?xml version="1.0"?>
<catalog>
  <book id="bk101">
    <author>Gambardella, Matthew</author>
    <title>XML Developer's Guide</title>
    <genre>Computer</genre>
    <price>44.95</price>
    <publish_date>2000-10-01</publish_date>
    <description>An in-depth look at creating applications
      with XML.</description>
  </book>
  <book id="bk102">
    <author>King, Stephen</author>
    <title>The Dark Tower: The Gunslinger</title>
    <genre>Fantasy</genre>
    <price>5.95</price>
    <publish_date>1982-12-10</publish_date>
    <description>In the first book of this brilliant series, Stephen King introduces readers to one of his
      most enigmatic heroes, Roland of Gilead, The Last Gunslinger. He is a haunting figure,
      a loner on a spellbinding journey into good and evil. In his desolate world, which
      frighteningly mirrors our own, Roland pursues The Man in Black, encounters an alluring woman named Alice,
      and begins a friendship with the Kid from Earth called Jake. Both grippingly realistic and eerily dreamlike,
      The Gunslinger leaves readers eagerly awaiting the next chapter.</description>
  </book>
  <book id="bk103">
    <author>Haldeman, Joe</author>
    <title>The Forever War</title>
    <genre>Science Fiction</genre>
    <price>6.95</price>
    <publish_date>1974-12-03</publish_date>
    <description>The Earth's leaders have drawn a line in the interstellar sand—despite the fact that the fierce
      alien enemy that they would oppose is inscrutable, unconquerable, and very far away. A reluctant conscript
      drafted into an elite Military unit, Private William Mandella has been propelled through space and time to
      fight in the distant thousand-year conflict; to perform his duties without rancor and even rise up through
      military ranks. Pvt. Mandella is willing to do whatever it takes to survive the ordeal and return home.
      But "home" may be even more terrifying than battle, because, thanks to the time dilation caused by space
      travel, Mandella is aging months while the Earth he left behind is aging centuries.</description>
  </book>
  <book id="bk104">
    <author>Rothfuss, Patrick</author>
    <title>The Name of the Wind</title>
    <genre>Fantasy</genre>
    <price>7.95</price>
    <publish_date>2001-03-10</publish_date>
    <description>Told in Kvothe's own voice, this is the tale of the magically gifted young man who
      grows to be the most notorious wizard his world has ever seen. A high-action story written with a poet's hand,
      The Name of the Wind is a masterpiece that will transport readers into the body and mind
      of a wizard.</description>
  </book>
  <book id="bk105">
    <author>Kurzweil, Ray</author>
    <title>How to Create a Mind</title>
    <genre>Science</genre>
    <price>14.99</price>
    <publish_date>2012-11-13</publish_date>
    <description> In How to Create a Mind, Kurzweil presents a provocative exploration of the most important project
      in human-machine civilization—reverse engineering the brain to understand precisely how it works and using
      that knowledge to create even more intelligent machines.</description>
  </book>
</catalog>
```

*Transform the XML document to HTML by editing the XSL stylesheet provided.
The answer should look like a table with two columns containing the attributes "Title" and "Author" for each book
in the book catalog. Test your XSL stylesheet on the XML document and render the transformed output to a web page.*

XSLT solution:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">
  <xsl:template match="/">
    <html>
      <body>
        <h2>My Book Wishlist</h2>
        <table border="1">
          <tr bgcolor="#9acd32">
            <th>Title</th>
            <th>Author</th>
          </tr>
          <xsl:for-each select="catalog/book">
            <tr>
              <td><xsl:value-of select="title"/></td>
              <td><xsl:value-of select="author"/></td>
            </tr>
          </xsl:for-each>
        </table>
      </body>
    </html>
  </xsl:template>
</xsl:stylesheet>
```

HTML output after running `xsltproc stylesheet.xsl books.xml > book_wishlist.html`:

```html
<html>
   <body>
      <h2>My Book Wishlist</h2>
      <table border="1">
         <tr bgcolor="#9acd32">
            <th>Title</th>
            <th>Author</th>
         </tr>
         <tr>
            <td>XML Developer's Guide</td>
            <td>Gambardella, Matthew</td>
         </tr>
         <tr>
            <td>The Dark Tower: The Gunslinger</td>
            <td>King, Stephen</td>
         </tr>
         <tr>
            <td>The Forever War</td>
            <td>Haldeman, Joe</td>
         </tr>
         <tr>
            <td>The Name of the Wind</td>
            <td>Rothfuss, Patrick</td>
         </tr>
         <tr>
            <td>How to Create a Mind: The Secret of Human Thought Revealed</td>
            <td>Kurzweil, Ray</td>
         </tr>
      </table>
   </body>
</html>
```

Final result when opening the HTML file with the Firefox browser:
![HTML Output](/assets/img/answers-12/book%20wishlist%20HTML.png){: height="180"}

<br>


### Exercise 2:  Regular expressions for Norwegian dates <br>
*Write a regular expession for a Norwegian short date, e.g. 10.03.2004 or 10.03.04.
The century and the leading zero for the day and month should be optional.* <br>

```regexp
\b[0-9]{1,2}\.[0-9]{1,2}\.([0-9]{2}){1,2}\b
```

The regular expression matches dates that are enclosed by a word boundary (\b), meaning that there has to be a space or
punctuation on either side of the date for it to be matched. The expression then requires there to be one or two digits
followed by a (literal) punctuation mark, then another one or two digits and punctuation mark, then either one or two
occurrences of two digits in succession (i.e. 04 and 2004 are both matched, but 204 is not).

<br>


### Exercise 3: Matching strings with word combinations <br>
*Write a regular expression which matches all strings that contain both `natural` and `language`, both consecutively or
with many other words in between. Match the whole string, not just the part where these two words occur.* <br>

```regexp
.*(natural)+.*(language)+.*
```

<br>

### Exercise 4: JFLAP and regular expressions
Look at the [lecture notes on JFLAP](https://lingkurs.h.uib.no/webroot/index.php?page=formallang/fsa1&lang=en&course=ling123).

*a) Write the transition table for the non-deterministic variant.* <br>

![A transition table](/assets/img/answers-12/transition-table.png){: .normal}

*b) Construct these automata in Jflap and test them with input.* <br>

I recommend that you try this yourself. If you run into problems, let me know. Here's a step-by-step guide to
how you can solve this exercise in JFLAP: <br>
 - Download and install [JFLAP](http://www.jflap.org/). Select `Get JFLAP` from the menu and fill out the form. Download the JFLAP version suitable for you OS and install it. NB! Make sure you have Java installed before installing JFLAP.
 - Open JFLAP and select `Finite Automaton`.
 - Select the State Creator icon (small circle on the toolbar) and create the states by left-clicking.
 - Select the Transition Creator icon (small arrow on the toolbar) and drag between states to create transitions. JFLAP will prompt you for a text input to name the transition.
 - Select the Attribute Editor icon (mouse pointer on the toolbar). Right click state `q0` and check the `Initial` box.
 - Right click state the final state(s) and check the `Final` box.
 - Click the `Input` tab from the toolbar at the top and select `Fast Run` or `Step by State` to test your FSA.
<br>

*c) The examples all describe the same language, which is finite.
Make a new automaton for the infinite language described by the regexp `kr\. [12]0*\.00`. Test. Also write a
transition table for this automaton.*<br>

JFLAP: <br>
![kroner.jflap](/assets/img/answers-12/kroner%20FSA.png){: height="180"}

<br>
Transition table: <br>
![Transition table for kroner](/assets/img/answers-12/kroner-transition-table.png){: .normal}


<br>

*d) Modify the last regexp so that only an even number of 0's before the decimal point are allowed,
e.g. `kr. 100.00`, `kr. 20000.00`, etc., but not `kr. 20.00`. Make an automaton for this language and test.* <br>
<br>

This can be achieved by grouping two 0's together, followed by a \* to indicate 0 or more occurrences of the two 0's.
```regexp
kr\. [12](00)*\.00
```
