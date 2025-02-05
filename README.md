# CS Regex Reference Guide for JavaScript

Check out this Gist if you're interested in learning more about Regex, aka
Regular Expressions.
> It's not a complete guide, but I did cover the basics to help you get started.
> I've also included my references and contact information at the bottom if you
> want to learn more.

---

## Summary

This is a general reference guide on how to understand and use some basic regular
expressions. I've broken down specific functions below with simple examples to
help you learn the concepts quickly.

---

**Publish Notes**
The content on this Gist was created on **[this GitHub Repo](https://github.com/ErikPlachta/cs-regex-reference-guide/)**, published to this **[GitHub Website](https://erikplachta.github.io/cs-regex-reference-guide/)**, and published on [this GitHub Gist](https://gist.github.com/ErikPlachta/250107ea2e00086af9c1d29082c502b1/).

---

---

## Repo Stats

[![GitHub license](https://img.shields.io/github/license/ErikPlachta/cs-regex-reference-guide)](https://github.com/ErikPlachta/cs-regex-reference-guide) [![GitHub Number of Languages](https://img.shields.io/github/languages/count/ErikPlachta/cs-regex-reference-guide)](https://github.com/ErikPlachta/cs-regex-reference-guide)
[![GitHub top Language](https://img.shields.io/github/languages/top/ErikPlachta/cs-regex-reference-guide)](https://github.com/ErikPlachta/cs-regex-reference-guide)
[![GitHub issues](https://img.shields.io/github/issues/ErikPlachta/cs-regex-reference-guide)](https://github.com/ErikPlachta/cs-regex-reference-guide/issues)
![GitHub last commit](https://img.shields.io/github/last-commit/erikplachta/cs-regex-reference-guide)

---

---

## Table of Contents

- [CS Regex Reference Guide for JavaScript](#cs-regex-reference-guide-for-javascript)
  - [Summary](#summary)
  - [Repo Stats](#repo-stats)
  - [Table of Contents](#table-of-contents)
  - [1. What is Regex?](#1-what-is-regex)
    - [What are some other ways to explain Regular Expressions?](#what-are-some-other-ways-to-explain-regular-expressions)
  - [2. Starting with Examples](#2-starting-with-examples)
    - [Understanding Regex -> Regular Expressions Are ~~not~~ Easy to Understand](#undersatnding-regex---regular-expressions-are-not-easy-to-understand)
    - [**Example - Phone Number**](#example---phone-number)
    - [**Example - Email Address**](#example---email-address)
  - [The Syntax](#the-syntax)
    - [**1. Regex Components**](#1-regex-components)
      - [**1.1 Literal Characters**](#11-literal-characters)
      - [**1.2 Meta Characters**](#12-meta-characters)
    - [**2. Anchors / Positions**](#2-anchors--positions)
    - [**3. Quantifiers -  Greedy and Lazy Match**](#3-quantifiers----greedy-and-lazy-match)
    - [**4. OR Operators**](#4-or-operators)
      - [**4.1 Character Classes / Bracket Expressions**](#41-character-classes--bracket-expressions)
      - [**4.2 Alteration Classes / Grouping and Capturing**](#42-alteration-classes--grouping-and-capturing)
    - [**5. Boundaries**](#5-boundaries)
    - [**6. Flags**](#6-flags)
    - [**7. Back-references**](#7-back-references)
    - [**8. Look-ahead and Look-behind**](#8-look-ahead-and-look-behind)
  - [Author](#author)
  - [Contact Me](#contact-me)
  - [Resources and References](#resources-and-references)

---

---

## 1. What is Regex?

... aka **regular expression**, is a universal syntax language used to simplify
advanced searching/filtering of content based on a user-specified search pattern.
> You define what you are searching at the level of precision you need.

**What makes a regex search/filter different from others is that it searches for
patterns in ASCII or Unicode characters.**
> You're not just looking for a specific character value, you're looking for all
> instances of a pattern within the content. For example, all phone-numbers,
> email-addresses, websites, or really any type of content that follows a
> universal pattern.

---

---

### What are some other ways to explain Regular Expressions?

Great questions!

The [MDN team said,](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)
> "*Regular expressions are patterns used to match character combinations in strings.*"

[Wikipedia says,](https://en.wikipedia.org/wiki/Regular_expression)
> "*A regular expression is a sequence of characters that specifies a search pattern in text. Usually such patterns are used by string-searching algorithms for "find" or "find and replace" operations on strings, or for input validation.*"

---

---

## 2. Starting with Examples

### Understanding Regex -> Regular Expressions Are ~~not~~ Easy to Understand

To understand a regex pattern, *the search / filter you're creating*, you'll need
to learn some syntax. But first, let's start with some examples.

### **Example - Phone Number**

Without the area-code, phone numbers are generally 10-digits separated by a space
or a hyphen.

---

**We can look for 10 digits like this `/\d\d\d-\d\d\d-\d\d\d\d/`**
Here's, we're using the [meta character](#2-meta-characters) argument `\d` for 
each unique digit that we're searching for separated by a hyphen.

BUT what IF the single phone-number is formatted differently through-out the data?
For Example, our regex expression ran on the below data would only return 1
results, `123-456-7890`, even though ALL of them are the same phone number.
> `(123)-456-7890`, `123-456-7890`, `123.456.7890`, and `123 456 7890`

---

**So how could we improve our regex expression?**

Well, considering the same phone numbers 4 times again, we want to account for
`(`,`)`, ` `, and `-`.
> `(123)-456-7890`, `123-456-7890`, `123.456.7890`, and `123 456 7890`

**1.** Add a FEW OR operators to account for spaces vs hyphens
    > To do this we'll use **`[]`**, the [character class syntax](#character-classes),
    > where each [literal character](#1-literal-characters) inside the square-
    > brackets is considered a unique argument.

**2.** Add the ability to match `(` and `)` if they exist.
    > For optional parameters, we'll use the **`?`** [quantifier](#quantifiers),
    > which allows us to search for instances that a value does and does not exist.

**What does this fully fleshed out syntactically accurate regex argument look like?**

`/(\(?)+(\d{3})+[-.) ]+(\s?)(\d{3})+[-. ]+(\s?)+(\d{4})/`

Let's break it down 👇🏼

| Syntax      | Description |
|-------------|-------------|
| **`/`** | Starting the regex expression |
| **`\(?`** | Left-parenthesis `(` if exists|
| **`+`** | Followed by... |
| **`\d{3}`** | A collection of 3 digits |
| **`+`** | Followed by... |
| **`[)-. ]`** | a right-parenthesis `)`, OR hyphen `-`, OR period `.`, OR a space` `|
| **`+`** | Followed by... |
| **`\s?`** | A white-space if it exists |
| **`[- ]`** | Hyphen OR a space|
| **`+`** | Followed by... |
| **`\d{3}`** | A collection of 3 digits |
| **`+`** | Followed by... |
| **`[-. ]`** | A hyphen, dash, or space |
| **`+`** | Followed by... |
| **`\s?`** | A white-space if it exists |
| **`+`** | Followed by... |
| **`\d{4}`** | A collection of 4 digits |
| **`/`** | Ending the regex expression |

---

### **Example - Email Address**

Now that we've covered the basics, let's look at a regex search pattern built to
search for email addresses.
> You'll notice I've covered less details here.

**Do you see a pattern in this regex search pattern?**

`/([a-z0-9_\.-]+)@([\da-z\.-]+)\.([a-z\.]{2,6})/`

Let's break it apart into smaller chunks based on the high-level patterns we see!

| Symbol | Description |
|--------|-------------|
| **`/`** | Starting the regex expression |
|**`(`** | Encapsulating a sub-expression |
|**`[a-z0-9_\.-]`** | Any alpha-numeric characters along with `_`, `.`, and `-`. Results must |
|**`+)`** | Ending sub-expression, and requiring it be followed by the next argument. |
|**`@`**    | MUST include the `@` character. |
|**`([\da-z\.-]+)`** | Any digit or alpha-number character, followed by a `.` or `-`. |
|**`([a-z\.]{2,6})`** | Look for alpha-characters of any combination between 2-6 characters in length |
| **`/`** | Ending the regex expression |

---

---

## The Syntax

In this section, I've included the syntax used in the above examples. The goal
is to have it server as a point-of-reference for the above along with helping you
develop the confidence and knowledge to create your own regex patterns.
> Not all sections contain examples by design.


---

> *Note: If you're looking for a resource to make it easy to lean regex while
> following along with this guide, check out this  website https://regexr.com/.*
>> *It allows you to create regex search patterns and get live-feedback.*

---

### **1. Regex Components**

#### **1.1 Literal Characters**

Any/all ASCII or Unicode characters you're wanting to search for or filter out. 
> This will include single characters.

| Syntax | Description                |
|--------|----------------------------|
| **`a-z`** | Any lower-case letters  |
| **`A-Z`** | Any upper-case letters  |
| **`0-9`** | Any digits |
| **`\.`**  | A period character |
| **`Unicode Characters`**  | There's a lot. here's an index -> *[Microsoft - Insert ASCII or Unicode Latin-based symbols and characters](https://support.microsoft.com/en-gb/office/insert-ascii-or-unicode-latin-based-symbols-and-characters-d13f58d3-7bcb-44a7-a4d5-972ee12e50e0)* |

#### **1.2 Meta Characters**

Regex operator that represent specific data-types within the ASCI or Unicode
character sets.

| Syntax | Description  | Note | Example |
|--------|--------------|------|---------|
| **`\`** | Converts qualifying ASCI or Unicode character into meta-characters. | *WARNING: If you don't use this it will be considered a literal-character!* | |
| **`/^`**| Any new line | | |
| **`.`** | Any ASCI or Unicode Character.| *WARNING: Within a [character class](#character-classes), a `.` does not need to be escaped to be read as a [literal character](#1-literal-characters)* | |
| **`\d`**| Any Digit 0-9 | | |
| **`\w`**| Anything that is a word-character | *`A-Z`*, *`a-z`*, **`0-9`** |  **`\w\w`** -> Returns any sequent of two word-characters. |
| **`\W`**| Anything that is NOT a word-character |   |   |
| **`\s`**| Any white-space characters. | `Space`, `Tab`, and sometimes `new-line`| **`\s\s`** -> Returns any sequent of two white-space characters. |
| **`\S`**| Anything that is NOT white-space. | `Space`, `Tab`, and sometimes `new-line` | |
| **`[a-z]`**| All character a-z. | When in a class, the `-` plays as an operator to reutrn a-z character argument values. See the [character classes](#character-classes) section for more details | | `[a-c]` -> return all characters between `a` and `c` |

---

---

### **2. Anchors / Positions**

... are used to match the location of a literal character within your defined
search parameters.

| Syntax  | Description  |   Example   |
|---------|--------------|-------------|
| **`^`** | Used to look for a string value start | `^test` looks for all strings that start with the [literal characters](#1-literal-characters) `t`, `e`, `s`, and `t`. |
| **`$`** | Used to look for [literal characters](#1-literal-characters) that end with a specific value. | `/test$/` looks for all strings that end with the [literal characters](#1-literal-characters) `t`, `e`, `s`, and `t`. |

---

---

### **3. Quantifiers -  Greedy and Lazy Match**

... are a meta character that modify the pervious meta characters in a regular expression.
> Based on your regex search parameters, how many of times do you want it to
> match in a row?

| Syntax | Description  |   Example   |
|--------|--------------|-------------|
| **`*`**|  0 or more   | **`/\d*/`** -> returns all digits, period. |
| **`?`**|  0 or 1      | **`/test?t/`** -> all combinations of `test` and `testt` where the second T is optional. |
| **`+`**|  1 or more   | **`/\d+/`** -> returns all digits, of length 1 or more. | |
| **`{min,max}`**| Range of number of times former argument must exist to qualify as a result. | **`\w{1,5}`** -> All word-character combinations with 1-5 characters followed by white-space.|
| **`{n}`**| Number of times the former argument must exist to qualify as a result. | **`\w{5}\s`** -> All word-character combinations with 5 character followed by white-space.   |

---

---

### **4. OR Operators**

How to use OR arguments within a regex statement.

#### **4.1 Character Classes / Bracket Expressions**

... is one of the two **OR operators**, where arguments are placed inside of square-brackets **`[ ]`**.

| Syntax | Description | Notes | Example |
|--------|-------------|-------|---------|
| **`[^argument]`** | **NOT OR Operator**, returns anything except for the argument in the Class | A carrot, `^`, becomes a [meta character](#2-meta-characters) when used at the preface of a class. Anywhere else and it becomes a [literal character](#1-literal-characters) | `[^0-5]` -> anything not 0-5. `[^a-c]` -> Anything that is not between the letters a-c.
| **`[.]`** |  All literal character instances of a period, `.` | Within a class, does **not** need to be escaped to be read as a literal character. |`[-.]` -> looks for the literal characters `-` OR `.` |
| **`a[bc]de`** | All cases of `abde` AND/OR `acde`. |   |  |
| **`[letter-letter]`**   | Any literal characters a-z based on character case. | A hyphen, `-` becomes a [meta character](#2-meta-characters) when used between two literal characters of the same family within a class. | `[a-c]` -> return all characters between `a` and `c`. `/\b[A-Za-z]{4}\b/` -> to match any 4-letter word with letter [literal characters](#1-literal-characters) in it. `/\b[A-Z][a-z]*\b/` -> to match any 0-or more letter word with letter [literal characters](#1-literal-characters) in it starting with a capital letter. `\b[\w]{4}\b` -> All 4 letter words that contain any value used within words. |
| **`[number-number]`** | A range between two numbers | A hyphen, `-` becomes a [meta character](#2-meta-characters) when used between two literal characters of the same family within a class. | `[0-5]{3}` -> All combinations of 3-digits where each unique digit is between 0 - 5 |

---

#### **4.2 Alteration Classes / Grouping and Capturing**

... is the second **OR Operator**, and is used with as an or operator to look
for grouped literal characters within parenthesis and separated by a vertical
bar `( arg1 | arg2 )`.

| Syntax | Description | Notes | Example |
|--------|-------------|-------|---------|
| **`(arg1\|arg2)`** | Return all instances where `arg1` or `arg2` exist. | This is how you search for very specific groups of literal characters. | `/[\w.]+@\w+\.+(com\|net\|edu)/` -> Returns all email address that end with .net, .com, or.edu |

---

---

### **5. Boundaries**

| Syntax  | Description  |   Example   |
|---------|--------------|-------------|
| **`\b`** |  A word boundry | **All 4 letter words** -> **`/\b\w{4}/`** -> Looking at each word, look ALL word-character values of length 4. `/\btest\b/` -> Returns a whole word search |

---

---

### **6. Flags**

... are used to classify specific search-case scenarios to you regex expression.
They can be combined or used individually as needed, and are added to the end
of your regex expression. `/regex-pattern/flag`

| Syntax  | Description  |   Example   |
|---------|--------------|-------------|
| **`g`** | Globally searching. |`/[a-z]/g` -> returns all letters within all content. |
| **`i`** | Case-insensitive searching | `/[a-z]/gi` -> returns ALL literal character despite case from A-Z and a-z |
| **`m`** | Multi-line searching. | `/^\d/gm` -> returns ALL initial digits within all lines 
| **`s`** | `Dotall mode` returns results with any [literal character](#11-literal-characters) between them. | |
| **`u`** | Enable Unicode support | |
| **`y`** | Sticky mode allows you to search exact position within content | |

---

---

### **7. Back-references**

… are used to synchronize pattern-group result parameters within a regex expression based on the pattern group you specify.

| Syntax  | Description  |   Example   |
|---------|--------------|-------------|
| **`\n`** | References `n` pattern group for what parameters to look for. | **`(['"])(.*?)\1`** ran on the content \``Testing: "my regex expression..."`\` -> returns `"my regex expression..."`.

---

---

### **8. Look-ahead and Look-behind**

| Syntax  | Description  |
|---------|--------------|
| **`(?=arg)`**	  | Lookahead returns what's immediately after `arg` |
| **`(?<=arg)`**	| Lookbehind returns what's immediately before `arg` |

---

---

<!--
![GitHub commit activity](https://img.shields.io/github/commit-activity/w/erikplachta/cs-regex-reference-guide)
![GitHub commit activity](https://img.shields.io/github/commit-activity/m/erikplachta/cs-regex-reference-guide)
![GitHub commit activity](https://img.shields.io/github/commit-activity/y/erikplachta/cs-regex-reference-guide)
-->
## Author

**[Erik Plachta](https://github.com/ErikPlachta)**

**Thanks for taking the time to read this!**
> If you want to check out more of my work, head on over to my [GitHub Page](https://www.github.com/erikplachta).

## Contact Me

**Do you want to get in touch?**
> Feel free to connect with me on my [Twitter @ErikPlachta](https://www.twitter.com/erikplachta/) or [LinkedIn @ErikPlachta](https://www.linkedin.com/in/erikplachta/)

---

## Resources and References

A collection of resources I used to learn about Regex.

- [Wikipedia - Regular Expressions](https://en.wikipedia.org/wiki/Regular_expression)
- [MDN - Regular Expressions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)
- [Microsoft - Regular Expression Language - Quick Reference](https://docs.microsoft.com/en-us/dotnet/standard/base-types/regular-expression-language-quick-reference)
- [MDN - Regular expression syntax cheat sheet](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions/Cheatsheet)
- [Regular-Exprsesions.info](https://www.regular-expressions.info/)
- [RexEgg.com](https://www.rexegg.com/)
- [YouTube - The Coding Train - Introduction to Regular Expressions - Programming with Text](https://www.youtube.com/watch?v=7DG3kCDx53c)
- [Regex tutorial — A quick cheat sheet by examples](https://medium.com/factory-mind/regex-tutorial-a-simple-cheatsheet-by-examples-649dc1c3f285)
- [zone.ni.com - Regular Expressions Components](https://zone.ni.com/reference/en-XX/help/371714F-01/nirghelp/regular_expressions_components/)
- [javascript.info - Backreferences in pattern](https://javascript.info/regexp-backreferences)
- [rexegg.com - Mastering Lookahead and Lookbehind](https://www.rexegg.com/regex-lookarounds.html)
