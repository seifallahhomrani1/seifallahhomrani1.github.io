---
layout : post
title : "Python - Flow Control"
author : Seif-Allah
Description : "Booleans, Conditions, Blocks of Code, Flow Control Statements and Modules."
category : Python 
header-img : "img/FlowControl.png"
tags : [programming language, development, python, introduction]
--- 


### Prerequisite : 
Last post.


### Boolean Values : 
The Boolean data type has only two values : True and False.
They also start with a capital T or F, with the rest of the word in lowercase. 

### Comparison Operators 
Comparison operators compare two values and evaluate down to a sing boolean value. 

<pre>

| Operator    | Meaning                  |
|-------------|--------------------------|
| ==          | Equal to                 |
| !=          | Not equal to             |
| <           | Less than                |
| >           | Greater than             |
| <=          | Less than or equal to    |
| >=          | Greater than or equal to |


</pre>


<script src="https://gist.github.com/pu71n/25a79b6f9784b7a8b5068d502d703e36.js"></script>

* Note that an integer or floating-point value will always be unequal to a string value. Python considers strings as text. 
* The = operator is for assignment only, not to confuse with the == (equal to ) operator. 


### Boolean Operators 

The threee boolean operators (and, or, and not) are used to compare Boolean values. Like comparison operators, they evaluate these expressions down to a boolean value. 


**The "and" operator**
<br>
<pre>
| Expression             | Evaluates to |
|------------------------|--------------|
| True and True          | True         |
| True and False         | False        |
| False and True         | False        |
| False and False        | False        |
</pre>
<br>
**The "or" operator**
<pre>
| Expression             | Evaluates to |
|------------------------|--------------|
| True or True           | True         |
| True or False          | True         |
| False or True          | True         |
| False or False         | False        |
</pre>

### The not Operator 
Unlike "and" and "or", the not operator operates on only one boolean value (or expression). The not operator simply evaluates to the opposite boolean value. 

<script src="https://gist.github.com/pu71n/dd5365f135cf9078ac6a15b34c5d3269.js"></script>

The Boolean operators have an order of operations just like the math operators do. After any math and comparison operators evaluate, Python evaluates the not operators first, then the and operators, and then the or operators.


### Elements of Flow Control 
Flow control statements often start with a "condition", and all are followed by a block of code called the "clause". 
* Conditions :
The boolean expressions that we've talked about can be considered all as conditions, which are the same thing as expressions; condition is just a more specific name in the context of flow control statements. Conditions always evaluate down to a boolean value, True or False. 
* Blocks of Code : 
Lines of Python code can be grouped together in blocks. You can tell when a block begins and ends from the indentation of the lines of code. The are three rules for blocks. 
1. Blocks begin when the indentation increases. 
2. Block's can contain other blocks. 
3. Blocks end when the indentation decreases to zero or to a containing block's indentation. 

Let's take an example : 
<script src="https://gist.github.com/pu71n/7e96e6a9d1436518cfa97d4e557084dd.js"></script>

By looking at the indentations, you'll be able to identify the blocks of code. 	

### Flow Control Statements 
Now let's take a look at the most important piece of flow control: \*\*the statements themselves\*\*. 

1. **if statements** : take a look at the previous example and you'll understand the syntax of an if statement : 
* The **if** keyword .
* A condition (that is, an expression that evaluates to Trus or False) 
* A colon
* Starting on the next line, an indented block of code (called the if clause) 

2. **else statements** :  
The else clause is executed only when the if statement's condition is false. 
The else statement always consists of the following :
* The **else** keyword. 
* A colon
* Starting on the next line, an indented block of code (called the else clause)

3. **elif statements** : 
The elif statement is an "else if" statement that always follows an if or another elif statement. It provides another condition that is checked only if any of the previous conditions were False. Its synax is : 
* The **elif** keyword. 
* A condition. 
* A colon
* Starting on the next line, an indented block of code (called theelif clause)

4. **while Loop Statement** :
The code in a while clause will be executed as long as the while statement's condition is True. In code, a while statement always consists of the following : 
* The while keyword
* A condition
* A colon
* Starting on the next line, an indented block of code (called the while clause)

5. **break Statements** :
There's a shortcut to getting the program execution to break out of a while loop's clause early. If the execution reaches a break statement, it immediatly exits the while loop's clause. In code, a break statement simply contains the **break** keyword. 

6. **continue Statements** : 
Like break statements, continue statements are used inside loops. When the program execution reaches a continue statement, the program execution immediately jumps back to the start of the loop and reevaluates the loop's condition.

7. **for loops and the rang() function**:
In code a for statement with a range() function looks something like **for i in range(5):** and always includes the following : 
* The **for** keyword 
* A variable name 
* The in keyword
* A call to the range() method with up to three integers passed to it (Starting, Stopping, and Stepping Arguments)
The first argument will be where the for loop's variable starts, and the second argument will be up to, but not including, the number to stop at, and the third is the step; the amount that the variable is increased by after each iteration.
* A colon
* Starting on the next line, an indented block of code (called the for clause)
Like the most programming languages for loops, the variable starts with a 0 and ends with 4 (n-1).
Note that you can use **break** and **continue** statements inside for loops as well. The continue statement will continue to the next value of the for loop's counter, as if the program execution had reached the end of the loop and returned to the start. In fact, you can use continue and break statements only inside while and for loops. If you try to use these statements elsewhere, Python will give you an error. 

**Notes** :
* Python does not have a **Do While** loop but we can make one.
* There are some values in other data types that conditions will consider equivalent to True and False such as : 0, 0.0 and '' (The empty string). 
* A negative for loop example should be like :
>\>\>\> for i in range(5,-1,-1): print (i)
...
5
4
3
2
1
0

### Importing Modules 
All Python programs can call a basic set of functions called built-in functions, including the print(), input(), and len() functions you've see before. Python also comes with a set of modules called the standard library. Each module is a Python program that contains a related group of functions that can be embedded in your programs. For example the **math** module has mathematics related functions, the random **random** module has rundom number-related functions, and so on.
Before you can use the functions in a module, you must import the module with an import statement. In code, an import statement consists of the following : 
* The **import** keyword
* The name of the module
* Optionally, more module names, as long as they are seperated by commas
Once you import a module, you can use all the cool functions of that module. 



**Note** : When we call imported functions, we need to use the name of the imported module as a prefix (e.g random.randint()). There's an alternative form of the import statement which is **from**, composed by from keyword followed by the module name. Whith this form of import statement, calls to functions will not need the prefix. However, using the full name makes for more readable code, so it is better to use the normal form of the import statement. 


### Ending a Program Early with sys.exit() 
The last flow control concept is how to terminate the program. This always happens if the program execution reaches the bottom of the instructions. However, you can cause the program to terminate, or exit, by calling the sys.exit() function. Since this function is in the **sys** module, you have to import it before your program can use it. 

<br>
That's all for today ! See you tomorrow <3


