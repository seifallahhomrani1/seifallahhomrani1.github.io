---
layout : post
title : "Python - Basics"
author : Seif-Allah
Description : "Data types, comments and basic built-in functions"
category : Python
header-img : "img/basics.png"
tags : [programming language, development, python, introduction]

---
### Prerequisite 
Installing python and launching the interactive shell. 

### Math Operators from Highest to lowest precedence (Order of operations) 
<br>
<pre>
| Operator     | Operation           | Example     | Evaluates to |
|--------------|---------------------|-------------|--------------|
| **           | Exponent            | 2**3        | 8            |
| %            | Modulo              | 22%8        | 6            |
| //           | Integer Division    | 22//8       | 2            |
| /            | Divison             | 22/8        | 2.75         |
| *            | Multiplication      | 3*5         | 15           |
| -            | Substraction        | 5-2         | 3            |
| +            | Addition            | 2+2         | 4            |
</pre>
<br>
<script src="https://gist.github.com/pu71n/26de843a4890f21fb601671c040652cf.js"></script>

You can always test to see whether an instruction works by typing it into the interactive shell. Don't worry about breaking the computer : The worst thing that could happen is that Python responds with an error message.

## Data Types

A data type is a category for values, and every value belongs to exactly one data type. The most common data types in Python are Integers, Floating-point numbers and Strings. 


For the strings type, Python knows where it begin and ends with the single quote ('') surrounding the text, If you ever see the error message **SyntaxError : EOL while scanning string literal**, you probably forget the final single quote character at the end of the string. 

### String Concatenation and Replication
The meaning of an operator may change based on the data types of the values next to it. For example, + is the addition operator when it operates on two integers or floating-point values. However, when + is used on two string values, it joins the strings as the string concatenation operator.
<br>
<script src="https://gist.github.com/pu71n/bf092368bcb696a290d690b1b8647531.js"></script>
<br>
But you can't concatenate variables with different types. Python will display the error **Can't Convert 'int' object to str implicitly** \*\*Remember the Zen of Python; Explicit is better than Implicit\*\* . So your code will have to explicitly convert the integer to a string because python can't do this automatically. (We will discuss data types conversion later). 
The \* operator is used for multiplication when it operates on two integers or floating-point values. But when the * operator is used on one string value and one integer value, it becomes the string replication operator.

<br>
<script src="https://gist.github.com/pu71n/84960f7c6cc2c85ea425bca8a825d3fb.js"></script>
<br>

The * operator can be used with only two numeric values (for multiplication) or one string value and one integer value (for string replication). Otherwise, Python will just display an error message.

### Variables 
Rules of naming variables in python : 
1. It can be only one word.
2. It can use only letters, numbers, and the underscore '_' charcter.
3. It can't begin with a number. 
4. Variable names are case-sensitive.

Some programmers use camelcase for variable names (LookLikeThis) instead of underscores (Look_Like_This).

Before we move on to next element, I want to mention that python does not support constants (Non-changing value assignments) like some other languages. 

### Python files 
The interactive shell is cool, but now we need to write our code and save it into .py files. I will not get deeper into the text editors and OSs but I'm using Vim and Ubuntu distro so the next line commands are entered into a linux terminal, so basically this is the script of our first python program where we print some text and ask for a name and an age and then use them for a reply. 

<script src="https://gist.github.com/pu71n/638ed3bbf0deccd41f330a7df3b67576.js"></script>

Enough for today, see you tomorrow ! <3 
**Stay Home, Stay Safe!**
