---
title : "Python - Pattern Matching"
layout : post
author : Seif-Allah
category : Python 
image : "/assets/images/python/python.png"

toc : true 
description : Using Regular expressions AKA Regex, making character classes, etc.
---

Regular expressions, called **regexes** for short, are descriptions for a pattern of text. 

### Creating Regex Objects 

All the **regex** functions in Python are in the **re** module. 
Passing a string value representing your regular expression to **re.compile()** returns a **Regex** pattern object (or simply, a Regex object)

### Matching Regex Objects 

A **regex** object's search() method searches the string it is apssed for any matches to the regex. The search() method will return **None** if the regex pattern is not found in the string. If the pattern is found, the **search()** method returns a **Match** object. **Match objects** have a **group()** method that will return the actual matched text from the searched string. 
Knowing that result of the search contains a Match object and not the null value None, we can call group() to return the match. 

### Review of Regular Expression Matching 

While there are several steps to using regular expressions in Python, each step is fairly simple. 

1. Import the regex module with **import re**
2. Create a **Regex object** with **re.compile** function. (Remeber to use raw string.) 
3. Pass the string you want to search into the Regex object's search method. This returns a **Match** object. 
4. Call the **Match** object's group() method to return a string of the actual matched text. 

### More Pattern Matching with Regular Expressions

1. **Grouping with Parentheses** : Adding parentheses will create groups in the regex : (\d\d\d)-(\d\d\d-\d\d\d\d), then you can use the group() match object method to grab the matching text from just one group. Passing 0 or nothing to the group() method will return the entire matched text. Using the **groups()** method will retreive all the groups at once as a tuple. you can use **multiple-assignment** trick to assign each value to a seperate variable.
To include parentheses inside the regular expression, we can use the \\( and \\) in the raw string passed to re.compile()

2. **Matching Multiple Groups With the Pipe** : The \| charcter is called a *pipe*. You can use it anywhere you want to match one of many expressions. For example, the regular expression r'Russia|Amarica' will match either 'Russia' or 'America'.
When both occur in the searched string, the first occurence of matching text will be returned as the **Match object**. You can also use the pipe to match one of several patterns as part of your regex (example : r'Bat(man|mobile|copter|bat)' ).
3. **Optional Matching with the Question Mark** : Sometimes there is a pattern that you want to match only oprtionally. That is, the regex should find a match whether or not that bit of text is there. The **?** charcter flags the group that precedes it as an optional part of the pattern. You can thing of the ? as saying :"if it's there print it, if it's not, don't worry".
4. **Matching Zero or More with the Star** : The \* (called the star or asterisk) means "match zero or more" - the group that precedes the star can occur any number of times in the text. It can be completely abscent or repeated over and over again. 
5. **Matching One or More with the Plus** : While \* means " match zero or more ", the + means "match one or more." 
6. **Matching Specific Repetitions with Curly Brackets** : If you have a group that you want to repeat a specific number of times, follow the group in your regex with a number in curly brackets, you can also specify a range by writing a minimum, a comma, and a maximum in between curly brackets, or even leave out the first or second number in the curly brackets to leave the minimum or maximum unbounded. 
7. **Greedy and Nongreedy Matching** : Python's regular expressions are *greedy* by default, which means that in ambiguous situtations they will match the longest string possible. The *non-greedy* version of the curly brackets, which matches the shortest string possible, has the closing curly bracket followed by a **question mark**. Note that the question mark can have two meanings in regular expressions: declaring a nongreedy match or flagging an optional group. These meanings are entirely unrelated.
8. **The findall() Method** : In addition to the *search()* method, Regex objects also have a **findall()** method. While *search()* will return a Match object of the first matched text in the searched string, the **findall()** method will return the strings of **every match in the searched string**.
find() will not return a Match object but a list of strings - as long as there are no groups in the regular expressions. Each string in the list is a piece of the searched text that matched the regular expression. If there are groups in the regular expression, the findall() will return a list of tuples. Each tuple represent a found match, and its items are the matched strings for each group in the regex. 

### Character Classes 
Here's a table which represents shorthands codes for common charachter classes.
<pre>
| Shorthand character class      | Represents                                                                                           |
|--------------------------------|------------------------------------------------------------------------------------------------------|
| \d                             | Any numeric digit from 0 to 9                                                                        |
| \D                             | Any character that is not a numeric digit from 0 to 9                                                |
| \w                             | Any letter, numeric digit, or the underscore character. Think of this as matching 'word' characters. |
| \W                             | Any character that is not a letter, numeric digit, or the underscore character.                      |
| \s                             | Any space, tab, or newline character. (Think of this as  matching 'space' characters.)               |
| \S                             | Any character that is not a space, tab, or newline.                                                  |


</pre>

Character classes are nice for shortening regular expressions.

### Making Your Own Character Classes
You can define your own character class using square brackets like for example r'[putin]'. You can also include ranges of letters or numbers by using a hyphen. For example : the character class [a-zA-Z0-9] will match all lowercase letters, uppercase letters, and numbers. Note that inside the square brackets, the normal regular expression symbols are not interpreted as such. This means you do not need to escape the .,\*,? or () characters with a preceding backslash. For example, the character class [0-5.] will match digits 0 to 5 and a period. You do not need to write it as [0-5\\.].
By placing a caret character (^) just after the character class's opening bracket, you can make a negative character class. A negative character class will match all the characters that are not in the character class. 

### The Caret and Dollar Sign Characters
You can also use the caret symbol (^) at the start of a regex to indicate that a match must occur at the beginning of a searched text. Likewise, you can put dollar sign ($) at the end of the regex to indicate the string must end with this regex pattern. And you can use ^ and $ together to indicate that the entire string must match the regex - That is, it's not enough for a match to be made on some subset of the string. 

### The Wildcard Character 
The . (or dot) character in a regular expression is called a **wildcard** and will match any character except for a newline. 

### Matching Everything with Dot-Star 
You can use the dot-star(.\*) to stand in for that "anything.". Remember that the dot character means "any single character except the newline" and the star character means "zero or more of the preceding character".
The dot-star uses **greedy** mode: It will always try to match as much text as possible. To match any and all text in a **nongreedy** fashion, use the dot, star, and question mark (.\*?).

### Matching Newlines with the Dot Character
The dot-star will match everything except a newline. By passing **re.DOTALL** as the second argument to **re.compile()**, you can make the dot character match all characters, including the newline character.

<!--25-04-2020 : 00:22-->


### Reveiw of Regex Symbols 
* The ? matches zero on one of the preceding group.
* The \* matches zero or more of the preceding group. 
* The + matches one or more of the preceding group. 
* The {n} matches exactly n of the preceding group.
* The {n,} matches n or more of the preceding group.
* The {,m} matches 0 to m of the preceding group.
* The {n,m} matches at least n and at most m of the preceding group.
* {n,m}? or \*? or +? performs a nongreedy match of the preceding group. 
* ^spam means the string must begin with spam. 
* spam$ means the string must end with spam. 
* The . matches any character, except newline characters.
* \d, \w, and \s match a digit, word, or space character, respectively. 
* \D, \W, and \S match anything except a digit, word, or space character, respectively. 
* [abc] matches any character between the brackets (such as a, b, or c). 
* [\^abc] matches any character that isn't the brackets.


### Case-Insensitive Matching
Normally, regular expressions match text with the exact casing you specify. But sometimes you care only about matching the letters without worrying whether they're uppercase or lowercase. To make your regex case-insensitive, you can pass **re.IGNORECASE** or **re.I** as a second argument to **re.compile()**.

