---
title : "Python - Functions"
layout : post
category : Python
description : "Python Functions, Local and Global Scope and Exception Handling"
author : Seif-Allah
image : "/assets/images/python/python.png"
toc : true

---
<br>
**A function is like a mini-program within a program**
<br>
**Function Syntax :**
<pre>
**def** functionName(SomeArguments)
	>Code
	>Code
	>Code

functionName(SomeArguments)  <- **Function Call**
</pre>
<br>
When the program execution reaches these calls, it will jump to the top line in the function and begin executing the code there. When it reaches the end of the function, the execution returns to the line that called the function and continues moving through the code as before.


### def statements with Parameters

A **parameter** is a variable that an argument is stored in when a function is called.
One special thing to note about parameters is that the value stored in a parameter is forgotten when the function returns. We will discuss this later.

### Return Values and return Statments
When you call the **len()** function and pass it an argument such as 'hello', the function call evaluates to the integer value 5, which is the length of the string you passed it. In general, the value that a function call evaluates to is called the **return value** of the function.
When creating a function using the **def** statement, you can specify the return value should be with a return statelent. A return statement consists of the following :
* The **return** keyword.
* The value or expression that the function should return.

When an expression is used with a **return** statement, the return value is what this expression evaluates to.

Let's take an example :
<script src="https://gist.github.com/pu71n/9fbe5f15931e56ac819760d371f9e8aa.js"></script>

### The **None** Value
In Python there is a value called **None**, which represents the abscence of a value. **None** is the only value of the **NoneType** data type. (Other programming languages might call this value null, nil or undefined.) Just like the boolean True and False values, None must be typed with a capital N.
This value-without-a-value can be helpful when you need to store somethin that won't be confused for a real value in a variable. One place where **None** is used is as the return value of print(). The print() function displays text on the screen, but it doesn't need to return anything like for example len(). But since all function calls need to evaluate to a return value, print =() returns None.
<br>
<script src="https://gist.github.com/pu71n/3ad89ef8f0fccdf25c9cd26e1e0ee98c.js"></script>
Behind the scenes, **Python adds return None to the end of any function definition with no return statement.**

### Keyword Arguments and print()

Keyword arguments are often used for optional parameters. For example, the print() function has the optional parameters **end** and **sep** to specify what should be printed at the end of its arguments and between its arguments, respectively.

<script src="https://gist.github.com/pu71n/82caba62982857e316e22d251d76cd33.js"></script>

Similarly, when you pass multiple string values to print(), the function will automatically seperate them with a string space but you could replace the default seperating string by passing the **sep** keyword argument.
<br>
<script src="https://gist.github.com/pu71n/afa307bda73b4b5c825b45a72f27a94a.js"></script>

### Local and Global Scope

* Parameters and variables that are assigned in a called are said to exist in that function's **local scope**.
* Variables that are assigned outside all functions are said to exist in the **global scope**. A variable that exists in a local scope is called a **local variable**, while a variable that exists in the global scope is called **global variable**. A variable must be one or the other; it cannot be both local and global.

A local scope is created whenever a function is called. When the function returns, the local scope is destroyed, and these variables are forgotten.
Scope matter for several reasons:
* Code in the global scope cannot use any local scope variables.
* However, a locale scope can access global variables.
* Code in a function's local scope cannot use variables in any other local scope.
* You can use the same name for different variables if they are in different scopes.

### The Global Statement
If you need to modify a global variable from within a function, use the **global** statement. It tells Python that this variable in this function refers to the global variable, so don't create a local variable with this name.
Here's an example :
<br>
<script src="https://gist.github.com/pu71n/b8d70462dd488971aed1c9c37fb27107.js"></script>

### Excecption Handling

Errors can be handled with **try** and **except** statements. The code could potentially have an error is put intry clause. The program execution moves to the start of a following **except** clause if an error happens.
Here's an example:
<br>
<script src="https://gist.github.com/pu71n/4751d420d6adc95c5677783dee5250f7.js"></script>

When code in a try clause causes an error, the program execution immeditly moves to the code in the **except** clause. After running that code, the execution continues as normal.
Note that once the execution jumps to the code in the **except** clause, it does not return to the **try** clause. Instead it just continues moving down as normal.

Enough for today, stay tuned for the practice project.
