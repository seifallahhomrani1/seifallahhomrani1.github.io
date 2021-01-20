---
title: "Java - Data Types"
author : Seif-Allah
layout: post
category : Java
description : Java - Data types (Primitive) and Unicode System.
image : /assets/images/java/oop_concepts.png
toc : true 
---

Java is *statically-typed* programming language.

Data types specify the different sizes and values that be stored in the variable. There are two types of data types in Java :

### Java Primitive Data Types

There are 8 types of primitive data types  : 
- boolean false
- byte 0
- char '\u0000' | Java uses 2 byte because it uses Unicode system instead of ASCII code system and the \u0000 is the lowest range of unicode system.
- short 0
- int 0
- long 0L
- float 0.0f
- double 0.0d

### Java Operators

There are many types of operators in Java which are given below : 
- Unary Operator  (expr++ / expr-- | ++expr / --expr | ~(bitwise complement) !)
- Arithmetic Operator (* / % + -)
- Shift (The Java left shift operator << is used to shift all of the bits in a value to the left side of a specified number of times.)
- Relational (Comparison & equality)
- Bitwise (& ^ |)
- Logical (AND, OR)
- Ternary ( ? : )
- Assignment (short + short = int : Compile Time Error -> Solution : TypeCasting)



### Unicode System 

Unicode is a universal international standard character encoding that is capable of representing most the world's written languages.

**Why java uses Unicode system ?**
Before Unicode, there were many language standards: 
- ASCII (American Standard Code for Information Interchange) for the United States. 
- ISO 8859-1 for Western European Language.
- KOI-8 for Russian.
- GB18030 and BIG-5 for chinese.

This caused two major problems which is : 
- A particular code value corresponds to different letters in the various language standards. 
- The encodings for languages with large character sets have variable length. Some common characters are encoded as single bytes, other require two or more byte.

The solution for those problems is the Unicode System.
The lowest value : \u0000
The highest value : \uFFFF




Resources : https://www.javatpoint.com/java-data-types
