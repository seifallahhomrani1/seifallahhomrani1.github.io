---
title: "OverTheWire - Bandit"
author : Seif-Allah
layout: post
category : CTF-WriteUp
---
### Basic Introduction : 
The Bandit wargame is aimed at absolute beginners. It will teach the basics needed to be able to play other wargames. 


### Level 0 -> Level 1
This level aims to teach us about the **ssh** command, which is a program for logging into a remote machine and for executing commands on a remote machine. 
To log into the game, run: 
- - -
```bash
ssh bandit.labs.overthewire.org -p 2220 -l bandit0
```
- - -	
bandit0's password is **bandit0**.
After logging-in, run **ls** command which will list all the files in the current directory, you'll find out that there's a *readme* file that contains our password for the next level as mentioned in description, use **cat** command to read it. 

**PASSWORD LEVEL1 : boJ9jbbUNNfktd78OOpsqOltutMc3MY1**

### Level 1 -> Level 2
After logging in and executing the **ls** command, I found that there's a file named "-". I used the **cat** command to read it but it shows a blinking cursor, which keeps on printing what I wrote, It's very obvious that the dash "-" is a special charachter used to refer to the stdout, So we need to skip it by using **/**.
- - -
```bash
cat ./-
``` 
- - -
**PASSWORD LEVEL2 : CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9**

### Level 2 -> Level 3
This level aims to teach us about the escape charachter and how to use it. 
- - -
```bash
cat spaces\ in\ this\ filename
```
- - -
**PASSWORD LEVEL3 : UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK**


### Level 3 -> Level 4
