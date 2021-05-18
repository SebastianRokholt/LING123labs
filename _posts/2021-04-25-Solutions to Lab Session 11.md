---
title: Solutions to Exercises from Lab Session 11
date: 2021-04-29 15:40:00 +0200
categories: [Solutions to Lab Exercises]
tags: [xslt, xsltproc, xml, xsl, html, ocr, css, regex, regular, expression, email, address, re, python, shell, script]
pin: false
---
<br>

Disclaimer: You should always attempt to solve the lab exercises by yourself before looking at the proposed solutions
below.
The exercises to Lab Session 11 are available in their entirety [here](https://ling123labs.com/posts/Lab-Session-11/). <br>
> ***[Link to relevant lecture notes](https://lingkurs.h.uib.no/webroot/index.php?page=xml/sonnet&lang=en&course=ling123)***

*Disagree with my solutions, or have something to add? <br>
Leave a [comment](#post-extend-wrapper)!* <br>
<br>

---

### Solution to Exercise 1: Filtering XML with XSLT

*a) Edit the XSL file from the
[lecture notes](https://lingkurs.h.uib.no/webroot/index.php?page=xml/sonnet&lang=en&course=ling123) to change the
layout of the web page.* <br>

There are many ways you can change the layout of the page, and I recommend that you experiment with a few different
layouts in order to become more comfortable with xml and xls. Here's an example of a simple solution, where I've used
xsl to change the title to "William Shakespeare's Sonnet 141". I also removed the indent on the last quatrain.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet xmlns:xsl="http://www.w3.org/1999/XSL/Transform"
    xmlns:xs="http://www.w3.org/2001/XMLSchema" exclude-result-prefixes="xs" version="1.0">
    <xsl:template match="shakespeareanSonnet">
        <html>
            <head>
                <title>Sonnet <xsl:apply-templates select="number"/></title>
                <link>
                    <xsl:attribute name="rel">stylesheet</xsl:attribute>
                    <xsl:attribute name="href">sonnet.css</xsl:attribute>
                    <xsl:attribute name="type">text/css</xsl:attribute>
                </link>
            </head>
            <h1>
                <xsl:apply-templates select="author"/>'s Sonnet
                    <a>
                        <xsl:attribute name="href">
                            <xsl:value-of select="sourceRoot"/>
                            <xsl:value-of select="number"/>
                        </xsl:attribute>
                        <xsl:apply-templates select="number"/>
                    </a>
            </h1>
            <xsl:apply-templates select="quatrain"/>
            <xsl:apply-templates select="couplet"/>
        </html>
    </xsl:template>

    <xsl:template match="number">
        <xsl:apply-templates/>
    </xsl:template>

    <xsl:template match="quatrain">
        <p class="poemtext">
            <xsl:apply-templates select="line"/>
        </p>
    </xsl:template>

    <xsl:template match="couplet">
        <p class="indent poemtext">
            <xsl:apply-templates select="line"/>
        </p>
    </xsl:template>

    <xsl:template match="line">
        <span>
            <xsl:apply-templates/>
        </span>
        <br/>
    </xsl:template>
</xsl:stylesheet>
```

<br>

*b) Edit the CSS file to change some visual aspects such as colors, fonts, etc.* <br>

You're not really expected to be proficient in CSS for this course, so you don't have to put much effort into this part.
On that note, you can get a quick and easy introduction to CSS by reading this [W3Schools CSS tutorial]().
Here is an example of changing the font and text alignment of `sonnet.html`:

```css
html {
    font-family: 'Palatino Linotype', 'Book Antiqua', Palatino, serif;
}

h1 {
    text-align: center;
}

.indent {
    margin-left: 1em
}

.poemtext {
    text-align: center;
}

a {
    color: blue;
    text-decoration: none
}

a:hover {
    color: red
}
```

<br>

### Solution to Exercise 2: An XSLT application for OCR
*a) Change the XSL file to enable more classes.* <br>

Here you need to add more xsl class attributes under the `<xsl:template match="String">` part of the .xsl file.
You can simply copy-paste the same syntax as for the other classes, then change the text values.
You can then edit the threshold for the confidence value in the element attribute `test="@WC &lt; some_number"`.
Here's an example with five classes (`verylowwc`, `lowwc`, `mediumwc`, `highwc` and `veryhighwc`).

```xml
<?xml version="1.0" encoding="UTF-8"?>
<xsl:stylesheet version="1.0" xmlns:xsl="http://www.w3.org/1999/XSL/Transform">

    <xsl:template match="/">
        <html>
            <head>
                <title>
                    <xsl:text>OCR RESULT</xsl:text>
                </title>
                <link>
                    <xsl:attribute name="rel">stylesheet</xsl:attribute>
                    <xsl:attribute name="href">ocr-color-new.css</xsl:attribute>
                    <xsl:attribute name="type">text/css</xsl:attribute>
                </link>
            </head>
            <body>
                <p>
                    <xsl:apply-templates select="/alto/Layout/Page/PrintSpace/TextBlock" />
                </p>
            </body>
        </html>
    </xsl:template>

    <xsl:template match="TextLine"> <!--make a new line for each TextLine-->
            <xsl:apply-templates select="String" />
        <br/>
    </xsl:template>

    <xsl:template match="String"> <!--make a span for each String (word)-->
        <span>
            <xsl:attribute name="class">
                <xsl:choose> <!--choose class depending on word confidence value-->
                    <xsl:when test="@WC &lt; 0.60">
                        <xsl:text>verylowwc</xsl:text>
                    </xsl:when>

                    <xsl:when test="@WC &lt; 0.75">
                        <xsl:text>lowwc</xsl:text>
                    </xsl:when>

                    <xsl:when test="@WC &lt; 0.90">
                        <xsl:text>mediumwc</xsl:text>
                    </xsl:when>

                    <xsl:when test="@WC &lt; 0.95">
                        <xsl:text>highwc</xsl:text>
                    </xsl:when>

                    <xsl:otherwise>
                        <xsl:text>veryhighwc</xsl:text>
                    </xsl:otherwise>

                </xsl:choose>
            </xsl:attribute>
            <xsl:value-of select="@CONTENT" />
            <xsl:text> </xsl:text> <!--add space after each string-->
        </span>
    </xsl:template>
</xsl:stylesheet>
```
*b) Define the new classes in the CSS file. Try to set different visual characteristics.
Consider not only different colors, but also a different background-color.* <br>

```css
/* css for word confidence coloring */

.verylowwc {
    color: #4c0000;
    background-color: #ffe5e5;
}

.lowwc {
    color: #333300;
    background-color: #ffffe5;
}

.mediumwc {
    color: #002d00;
    background-color: #e5ede5;

}

.highwc {
    color: #191314;
    background-color: #fff2f4;
}

.veryhighwc {
    color: #00004c;
    background-color: #f4f4ff;
}
```

<br>


### Solution to Exercise 3: Regular expressions and email addresses
*Write a Python or shell script to extract all UiB email adresses from an input file. <br>
UiB email adresses must follow the format `local-part@domain`, where `local-part` follows the syntax as noted in
[this article](https://en.wikipedia.org/wiki/Email_address#Local-part) and `domain` is either `uib.no`
or `student.uib.no`.* <br>

```python
# Filename: regex-email.py

import re

test_text = """
<p class="field field-name-field-uib-lead field-type-text-long field-label-hidden field-wrapper">
      <a href="mailto:Guowen.Shang@uib.no">Guowen Shang</a>, Associate Professor of Chinese language,
      is currently studying sign languages – not languages
      with signs, but on signs. “The language signs are everywhere in our environment, but the language choices on
      the signs are determined by a range of social, economic, psychological and political factors”. This inquiry is
      in accord with his academic commitment to the language policy and planning issues, especially in China.</p>

Email addresses:
> myemail@maildrop.cc
> james-gordon93@testmail.ninja
> joe_mama@student.uib.no
> big_shot_prof@uib.no
> xcv921@uib.no
> intricatelongaddress@gmail.com
> not-a-valid-email:adress@uib.no
> another..not.valid@address@fakemail.com
> 900-a-valid-address(a_comment)@uib.no

Sit et maxime debitis nihil. Exercitationem omnis a velit in necessitatibus.
Feel free to [shoot me an email](sebastian.rokholt@student.uib.no) if you need help with anything.
Id odio sit at. Et laborum aut facere distinctio. Nemo et dolorum quisquam esse quidem.
"""

emails = re.findall('[a-zA-Z0-9_.+\-]+[(a-zA-Z0-9_.+\-)]*@(?:uib|student.uib).no', test_text)
for email in emails:
    print(email)
```

```shell
$ python regex-email.py
Guowen.Shang@uib.no
joe_mama@student.uib.no
big_shot_prof@uib.no
xcv921@uib.no
adress@uib.no
900-a-valid-address(a_comment)@uib.no
sebastian.rokholt@student.uib.no
```

<br>

---
