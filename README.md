# Regular Expressions

## Introduction

<details>
<summary>What are regular expressions?</summary>  
<br />  

Regular expressions are a tool for searching and matching parts of a text by describing the patterns that should be used to identify those parts. 

A regular expression is a set of symbols that describes a text pattern. It is used for matching, searching and replacing text. It is not a programming language.

Use cases
- test if email address or phone number is in a valid format
- search a document for text (Notepad++)
- restrict file extensions for search (VS Code)
</details>

<details>
<summary>Regular Expression Engines</summary>  
<br />  

- C/C++
- Java
- JavaScript
- .NET
- Perl
- PHP
- Python
- Ruby
- Unix
- Apache
- MySQL

Most features of regular expressions work the same across all engines, but there might be some differences.

Online playground/sandbox for JavaScript 
- https://regexr.com/
- https://regex101.com/
- https://www.regexpal.com/
</details>

<details>
<summary>Notation conventions and modes</summary>  
<br />  

Notation | Expression
--|--
Standard regex notation|```/expression/```
Global | ```/expression/g```
Case insensitive | ```/expression/i```
Multiline | ```/expression/m```
</details>
<br />

## Characters

<details>
<summary>Literal Characters</summary>  
<br />

> ```/car/```	matches "car" and "carnival" 	

Case sensitive by default

> e.g. ```/cat/```  

Standard (non-global) matching - The earliest leftmost match is always preferred.  
> The cow, camel and ```cat``` communicated.

Global matching finds all matches.  
> The cow, camel and ```cat``` communi```cat```ed.
</details>

<details>
<summary>Meta characters</summary>  
<br />  

Characters with special meaning

``` \ . * + - { } [ ] ^ $ | ? ( ) : ! = ```
</details>

<details>
<summary>Wildcard .</summary>  
<br />  

Period ```.``` matches anything (single character) except new line  
> ```/h.t/``` will match "hat", "hot" and "hit" (but not "heat")

- Broadest match possible
- Most common metacharacter
- Most common mistake

> ```/.a.a.a/``` will match "banana" and "papaya"  
```diff
- Caution
```  
> ```/9.00/``` will match "9.00", aswell as "9500" and "9-00"  

A good regular expression should match the text you want to target and only that text, nothing more.
</details>

<details>
<summary>Escaping metacharacters \</summary>  
<br />  

Backslash ```\``` escapes the next character to allow use of metacharacters as literal characters  

> ```/9\.00/```	will now match "9.00", but not "9500" or "9-00" 

Escape a backslash using double ```\\```
Quotes marks are not metacharacters
</details>

<details>
<summary>Other special characters</summary>
<br />  

- Spaces
- Tabs (\t)
- Line returns (\r, \n, \r\n)
</details>
<br />

## Character sets [ ]

<details>
<summary>What is a character set?</summary>  
<br />  

```[``` Begin a character set  
```]``` End a character set 

A character set will match any one of several characters, but only one character.  
Order of characters in character set does not matter

> ```/[aeiou]/``` matches any one vowel  
> ```/gr[ea]y/``` will match "grey" and "gray"   
> ```/gr[ea]t/``` will not match "great"
</details>

<details>
<summary>Character Range -</summary>  
<br />  

Includes all characters between hyphens.  
```-``` is a metacharacter only inside a character set ```[ ]```

- ```/[0-9]/```
- ```/[a-z]/```
- ```/[A-Z]/```
- ```/[A-Za-z]/```

> ```/[a-dx-z]/``` will match a, b, c, d and x, y, z
 
 ```diff
 - Caution
 ```  
> ```[50-99]``` is not all numbers from 50 to 99. A character set including 5, 0-9 and 9 (same as ```[0-9]```) 
</details>

<details>
<summary>Negative Metacharacter ^</summary>  
<br />  

Add ```^``` as the first character inside a character set to negate the character set

> ```/[^aeiou]/``` matches any one consonent  
> ```/[^a-zA-Z]/``` matches any one non alphabetic character  
> ```/see[^mn]/``` matches "seek" and "sees", but not "seem" or "seen"  
 ```diff
 - Caution
 ```  
> ```/see[^mn]/``` does not match "see", but matches "see." and "see "
</details>

<details>
<summary>Metacharacters inside Character Sets</summary>  
<br />  

Most metacharacters inside character sets are already escaped
with these exceptions ```] - ^ \```  
> ```/h[a.]t/``` matches "hat" and "h.t", but not "hot"  
</details>

<details>
<summary>Shorthand character sets</summary>
<br />  

There's also a negative version of each, which we get by just capitalizing the letter

Shorthand | Meaning | Equivalent
--|--|--
```\d``` | Digit | equivalent to ```[0-9]```
```\w``` | Word Character | equivalent to ```[a-zA-Z0-9_]```
```\s``` | Whitespace | equivalent to ```[\t\r\n]```  
```\D``` | Not digit | equivalent to ```[^0-9]```
```\W``` | Not word character | equivalent to ```[^a-zA-Z0-9_]```
```\S``` | Not whitespace | equivalent to ```[^\t\r\n]```  

<br />
The first three are most common, the last three are not in all regular expression engines.  

```diff
- Caution underscore is a word character, hyphen is not 
```  
> ```/\d\d\d\d/``` matches "1984", but not "text"  
> ```/\w\w\w/``` matches "ABC", "123" and "1_A"  
> ```/\w\s\w\w/``` matches "I am", but not "Am I"  
> ```/[^\d]/``` is the same as ```/\D/``` and ```/[^0-9]/```

```diff
- Caution /[^\d\s]/ is not the same as [\D\S] 
```  
> ```/[^\d\s]/``` means NOT digit and NOT space character  
> ```[\D\S]``` means EITHER NOT digit or NOT space character (try this out)
</details>
<br />

## Repetition

<details>
<summary>Repetition metacharacters</summary>  
<br />  

Metacharacter | Meaning
--|--
```*``` | Preceding item, zero or more times 
```+``` | Preceding item, one or more times
```?``` | Preceding item, zero or one time  
<br />
> ```/.+/``` matches any string of characters except line returnds  
> ```/\d+/``` matches any number of digits  
> ```/\s[a-z]+ed\s/``` matches lower case words ending in "ed" (space before and after)

> ```/apples*/``` matches "apple", "apples" and "applessss" (zero or more s)   
> ```/apples+/```	matches "apples" and "applessss", but not "apple"  (atleast one s is required)  
> ```/apples?/``` matches "apple" and "apples", but not "applessss" (atmost one s is required)  
> ```/colou?r/```	matches "color" and "colour"	
</details>

<details>
<summary>Quantified Repetition {min, max}</summary>  
<br />  

- ```{min, max}``` are positive numbers, max should be greater than min
- min must always be included and can be zero

>  ```/\d{4,8}/``` matches numbers with four to eight digits  
> ```/\d{4}/``` matches numbers with EXACTLY four digits (min is max)  
> ```/\d{4,}/``` matches numbers with four or more digits (max is infinite)

> ```/\d{0,}/``` same as ```/\d*/```  
> ```/\d{1,}/``` same as ```/\d+/```  
> ```/\d{3}-\d{3}-\d{4}/``` matches motst US phone numbers
</details>

<details>
<summary>Greedy Expressions</summary>  
<br />  

- Standard reprtition quantifiers are greedy  
- Expression tries to match the longest possible string before giving control to the next expression part

```diff
- Caution 
```

> ```/\d+\w+\d+/``` on "01_FY_07_report_99.xls" hoping to match only ```01_FY_07``` (digits words digits), it matches ```01_FY_07_report_99``` instead

> ```/".+", ".+"/``` on "Pimpale", "Rishikesh", "RedMane Technology LLC." hoping to match only ```"Pimpale", "Rishikesh"``` (lastname comma first name), it matches the whole string instead

> ```/.*[0-9]+/``` on "Page 266"  
```Page 26``` is matched by the wild card portion and ```6``` is matched by the numbder character portion.
</details>

<details>
<summary>Lazy Expressions ?</summary>  
<br />  

- ```?``` Make the preceding quantifier lazy  
- Expression tries to match as littile as possible before giving control to the next expression part  
- It is not necessarily faster or slower than Greedy expressions, it just returnds different results  
- The context of ```?``` is different inside a character set ```[]```  

> ```/\d+\w+?\d+/``` on "01_FY_07_report_99.xls" now matches ```01_FY_07```  

> ```/".+", ".+"/``` on "Pimpale", "Rishikesh", "RedMane Technology LLC." now matches ```"Pimpale", "Rishikesh"``` instead of the whole string

> ```/.*?[0-9]+/``` on "Page 266"  
now ```Page_``` is matched by the wild card portion and ```266``` is matched by the numbder character portion.

> ```/.*?[0-9]+?/``` on "Page 266"  
now ```Page_``` is matched by the wild card portion and only ```2``` is matched by the numbder character portion.

</details>
<br />

## Grouping and Alternation

<details>
<summary>Grouping metacharacters ( )</summary>  
<br />  

```(``` Start grouped expression  
```)``` End grouped expression  

- ```( )``` Group portions of the expression  
- Apply repetition operators
- Create a group of alternation expressions (next topic)
- Capture group for use in matching and replacing (Tools that allow find and replace)

> ```/(abc)+/``` matches "abc" and "abcabcabc"  
> ```/(in)?depdendent/``` matches "indepdendent" and "depdendent"  
> ```/run(s)?/``` is the same as ```/runs?/```, but more readable

capture group for use in matching and replacing  
> ```/(\d{3})-(\d{3}-\d{4})/``` matches 312-709-4162  
> ```$1``` = 312 	```$2``` = 709-4162
</details>

<details>
<summary>Alteration metacharacter |</summary>  
<br />  

- ```|``` OR operator matches previous or next expression  
- Ordered, leftmost expression gets precedence

> ```/apple|orange/```	matches "apple" and "orange"  
> ```/w(ei|ie)rd/``` matches "weird" and "wierd"  
> ```/(AA|BB|CC){4}/``` matches "AABBAACC" and "CCCCBBBB"  
</details>

<details>
<summary>Efficency when using alternation</summary>  
<br />  

- Put simplest (most efficient) expression first  

> ```/peanut|peanutbutter/``` on "peanutbutter" matches peanut (leftmost expression)  
> ```/peanut(butter)?/``` matches the whole string ```peanutbutter``` (greedy by default) 
> ```/peanut(butter)??/``` matches only ```peanut``` (lazy) 

</details>
<br />

## Anchors

<details>
<summary>Start and End Anchors</summary>  
<br />  

Start/End anchors reference a position, not an actual character. They are zero-width  

Metacharacter | Meaning
--|--
```^``` | Start of a string/line   
```$``` | End of a string/line  
```\A``` | Start of a string, never end of line (not supported in JS) 
```\Z``` | End of a string, never end of a line (not supported in JS) 
<br />  

> ```/^apple/``` or ```/\Aapple/``` must be first word  
> ```/apple$/``` or ```/\ZApple/``` must be last word  
> ```/^\w+@\w+\.(com|edu|org|net)$/``` Simple Email validation
</details>

<details>
<summary>Line breaks and multiline mode</summary>
<br />

Language | Syntax
--|--
Perl | ```/^regex$/m```
Ruby | ```/^regex$/m```
PHP | ```/^regex$/m```
JavaScript | ```/^regex$/m```
Java | ```Pattern.compile("^regex$", Pattern.MULTILINE)```
.NET | ```Regex.Match("string","^regex$",RegexOptions.Multiline)```
Python | ```re.search("^regex$", "string", re.MULTILINE)```
<br />  
</details>

<details>
<summary>Word boundries</summary>  
<br />  

Metacharacter | Meaning
--|--
```\b``` | Word boundry (start or end)  
```\B``` | Not a word boundry
<br />

- Reference a position, not an actual character  
- Before the first word character in the string  
- After the last word character in the string  
- Between word character and non-word character  

> ```/^I\b/gm``` matches all occurances of "I" at the start of a paragraph  
> ```\B\w+\b/``` finds two matches in "This is a test" (```hi``` and ```es```)
</details>
