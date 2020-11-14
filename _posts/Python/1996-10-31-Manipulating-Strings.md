---
layout : post
title : "Python - Manipulating Strings"
author : Seif-Allah
Description : "Introduction to Python  "
category : Python 
header-img : "img/function.png"
tags : [programming language, development, python, introduction]
---

Typing string values in Python code is fairly straightforward: They begin and end with a single quote or double quotes. The benifit of using the double quotes is that string can have a single quote character in it. 
### Escape Characters 
An **escape character** lets you use characters that are otherwise impossible to put into a string. An escape character consists of a backslash **(\)** foloowed by the character you want to add to the string. 
<pre>
| Escape character    | Prints as    |
|---------------------|--------------|
| \'                  | Single quote |
| \"                  | Double Quote |
| \t                  | tab          |
| \n                  | Newline      |
| \\                  | Backslash    |
</pre>

### Raw Strings 
You can place an **r** before the beginning quotation mark of a string to make it a raw string : a **raw string** completely ignores all escape characters and prints any backslash that appears in the string. 

### Multiline Strings with Triple quotes 
While you can use the \n escape character to put a newline into a tring, it is often easier to use multiline strings. A multiline string in Python begins and ends with either three single quotes or three double quotes. Any quotes, tabs, or newlines in between the triple quotes are considered part of the string. Python's indentation rules for blocks do not apply to lines inside a multiline string. 

### Multiline Comments 

While the hash character (#) marks the beginning of a comment for the rest of the line, a multiline string is often used for comments that span multiple lines. 

### Indexing and Slicing String

Strings use indexes and slices the same way lists do. You can think of the string "I'm putin" as a list and each character in the string as an item with a corresponding index. 
If you specify an index, you'll get the character at that position in the string. If you specify a range from one index to another, the starting index id included and the ending index is not.

### The in and not in Operators with Strings 
The **in** and **not in** operators can be used with strings just like with list values. An expression with two strngs joined using **in** or **not in** will evaluate to boolean True or False.

### Useful String Methods 
* **upper()** and **lower()** : return a new string where all the letters are converted to uppercase or lowercase. Note that these methods do not change the string itself but return new string values.
* **isupper()** and **islower()** : return a boolean if the string has at least one letter and all the letters are uppercase or lowercase, respectively.
* **isalpha()** : returns True if the string consists only of letters and is not blank.
* **isalnum()** : returns True if the string consists only of letters and numbers and is not blank ('').
* **isdecimal()** : returns True if the string consists only of numeric characters and is not blank. 
* **isspace()** : returns True if the string consists only of spaces, tabs and newlines and is not blank. 
* **istitle()** : returns True if the string consists only of words that begin with an uppercase letter followed by only lowercase letters.
* **startswith()** and **endswith()** : return True if the string value the are called on begins or ends (respectively) with the string passed to the method; otherwise, they return False. 
* **join()** and **split()** :  The join() method is useful when you have a list of strings that need to be joined together into a single string value. The join() method is called on a string, gets passed a list of strings, and retruns a string. The returned string is the concatenation of each string in the passed in list. The split() method does the opposite : It's called on a string value and returns a list of strings. 
* **rjust()** and **ljust()** : return a padded versioon of the string they are called on, with spaces inserted to justify the text. The first argument to bith methods is an integer length for the justified string.
* **center()** : return a centered text that has the correct spacing than justifying it to the left or right. 
* **strip()**, **rstrip()** and **lstrip()** : strip() method will return a new string without any whitespace characters at the beginning or end. The lstrip() and rstrip() methods will remove whitespace characters from the left and right ends, respectively. Optionally, a strip argument will specify which characters on the ends should be stripped. 
<br>
Enough for today <3 
