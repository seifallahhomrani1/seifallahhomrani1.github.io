---
title : "KnightCTF: Most Secure Calculator 1/2"
author : Seif-Allah
description : "KnightCTF: Most Secure Calculator Writeup (50+250 Points)"
layout : post
category: CTF-WriteUp
tags : [CTF,WriteUp,Medium]
image : /assets/images/ctf/knight/rsz_seccalc.png
---


## Introduction

KnightCTF 2022 was a beginner level CTF hosted yesterday, The web challenges were basically easy and I was playing solo just for fun. The following is a detailed writeup for "Most Secure Calculator 1/2" challs since they have the highest points of all other challs (The first one has : 50 Pts, and the second has 250 Pts). 

## Most Secure Calculator - 1

![sec1](/assets/images/ctf/knight/sec1.png)

The first chall has an input expecting a math formula, let's try inject a ' to see if any error happens.

![sec1_error](/assets/images/ctf/knight/sec1_error.png)

Well, based on the debug error, there's an **eval** function evaluating the input as PHP code. eval() is well known for its unsafe use. 

![sec1_eval](/assets/images/ctf/knight/sec1_eval.png)

As mentioned in the documentation, eval alows execution of arbitrary PHP code, so let's run some code : [system("ls");](https://book.hacktricks.xyz/pentesting/pentesting-web/php-tricks-esp/php-useful-functions-disable_functions-open_basedir-bypass)


![sec1_code](/assets/images/ctf/knight/sec1_code.png)

Running : system("cat flag.txt") will prints the flag: 

![sec1_flag](/assets/images/ctf/knight/sec1_flag.png)

## Most Secure Calculator - 2

![sec2](/assets/images/ctf/knight/sec2.png)

Same as the first chall, but this one it executes some input filter to execute only numbers and symbols

![sec2_comment](/assets/images/ctf/knight/sec2_comment_.png)

The main idea was to try to find a way to inject some code only by numbers, doing some google-fu results in this [stackoverflow thread](https://stackoverflow.com/questions/27468974/php-convert-an-octal-characters-to-string)

So basically, we can input octal characters inside double quoted string and this will decoded and executed by the eval(), noice! 

Running some Octal encoding of "system"("ls"); using CyberChef, will result : "\163\171\163\164\145\155"("\154\163"); (I've replaced the ",(,; characters to reduce the payload size since they don't get filtered), and here we are : 

![sec2_ls](/assets/images/ctf/knight/sec2_ls.png)

Final payload: "\163\171\163\164\145\155"("\143\141\164\040\146\154\141\147\056\164\170\164");

![sec2_flag](/assets/images/ctf/knight/sec2_flag.png)


## Conclusion

PHP sucks, Fact! 