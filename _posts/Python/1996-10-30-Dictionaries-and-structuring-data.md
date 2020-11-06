---
layout : post
title : "Python - Dictionaries and Structuring Data"
author : Seif-Allah
subtitle : "Dictionaries, Using Data Structures to Model Real-World Things"
category : Python 
header-img : "img/intro.png"
tags : [programming language, development, python, introduction]
---
### The Dictionary Data Type
Like a list, a **dictionary** is a collection of many values. But unlike indexes for lists, indexes for dictionaries can use many different data types, not just integers. Indexes for dictionaries are called **keys**, and a key with its associated value is called a **key-value pair**.
In code, a dictionary is typed with braces {}.
<br>
<script src="https://gist.github.com/pu71n/f0e79038bbe1f766afc714d40f7298c5.js"></script>

Dictionaries can still use integer values as keys, just like lists use integers for indexes, but they do not have to start at 0 and can be any number.

### Dictionaries vs. Lists
Unlike lists, items in dictionaries are unordered. There's no first item in a dictionary. 
Trying to access a key that does not exist in a dictionary will result in a **keyError** message, much like a list's "out-of-range" IndexError. 
Though Dictionaries are not ordered, the fact that you can have arbitrary values for the keys allows you to organize you data in a powerful ways.

### The keys(), values(), and items() Methods 
There are three dictionary methods that will return list-like values of the dictionary'keys, values, or both keys and values: **keys(), values()** and **items()**.
The values returned by these methods are not true lists: They cannot be modified and do not have and **append()** method. But these data types (dict\_keys, dict\_values, and dict\_items, respectively) can be used in **for** loops. 
Here's an example : 
<br>
<script src="https://gist.github.com/pu71n/4cdffdae93fedbe51e38cd793ad8da95.js"></script>
Using the keys(), values() and items() methods, a for loop can iterate over the keys, values, or key-value pairs in a dictionay, respectively. Notice that the values in the dict\_items value returned by the items() method are tuples of the key and value. 

If you want a true list from one of these methods, pass its list-like return value to the list() function. If you want a true list from the one of these methods, pass its list-like return value to the list() function. 

You can also use the multiple assignement trick in a for loop to assign the key and a value to seperate variables.
Here's an example : 
<br>
<script src="https://gist.github.com/pu71n/4450808471e4407903661f346c2ca005.js"></script>

### Checking Whether a Key or Value Exists in a Dictionary
Recall from the previous post that the **in** and **not in** operators can check whether a value exists in a list. You can also use these operators to see whether a certain key or values exists in a dictionary. Like for example : 
<br>
<script src="https://gist.github.com/pu71n/be5750fddd92ab8c8815bdc388a18579.js"></script>

We can use "'president' in whoami" without the keys() method and get the same answer.

### The get() Method 
It's tedious to check whether a key exists in a dictionary before accessing that key's value. Fortunatly, dictionaries have a get() method that takes two arguments : the key of the value to retrieve and a fallback value to return if that key does not exist. 
### The setdefault() Method 
You'll often have to set a value in a dictionary for a certain key only if that key does not already have a value. The **setdefault()** method offers a way to do this in one line of code. The first argument passed to the method is the key to check for, and the second argument is the value to set at that key if the key does not exist. If the key does exist, the **setdefault()** method returns the key's value. 

Here's an example : 
<br>
<script src="https://gist.github.com/pu71n/1ca8b06756806367ca823efa075c69a7.js"></script>

The setdefault() method is a nice shortcut to ensure that a key exists. 

Enough for today, see you tomorrow <3
