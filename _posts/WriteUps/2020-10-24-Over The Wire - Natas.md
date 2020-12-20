---
title: "OverTheWire - Natas"
author : Seif-Allah
layout: post
category : CTF-WriteUp
---


### Level 1 
View Source. 
**PASSWORD 1 : gtVrDuiDfck831PqWsLEZy5gyDz1clto**
### Level 2 
View Source.
**PASSWORD 2 : ZluruAthQk7Q2MqmDeTiUij2ZvWy2mBi**
### Level 3 
../files/users.txt
**PASSWORD 3 : sJIJNW6ucpu6HPZ1ZAchaDtwd7oGrD14**
### Level 4 
robots.txt
**PASSWORD 4 : Z9tkRkWmpt9Qr7XrR5jWRkgOU901swEZ**
### Level 5 
Here we need to set the referer header to : "http://natas5.natas.labs.overthewire.org/" to get the password. 
**PASSWORD 5 : iX6IOfmpN7AYOQGPwtn3fXpbaJVJcHfq**
### Level 6
We need to change the logged cookie value from 0 to 1 to get the password. 
**PASSWORD 6 : aGoY4q2Dc6MgDq4oL4YtoKtyAg9PeHa1**
### Level 7
The php script checks the secret included in secret.inc file with the secret typed in the form, checking this file we could find the secret : *FOEIUWGHFEEUHOFUOIU*
**PASSWORD 7 : 7z3hEENjQtflzgnT29q7wAvMNfZdh0i9**
### Level 8 
URL.
**PASSWORD 8 : DBfUBfqQG69KvJvJ1iAbMoIpwSNQ9bWe**
### Level 9 
Reversing the encoded secret : Hex -> Reverse -> Base64
Secret : *oubWYf2kBq*
**PASSWORD 9 : W0mMhUcRRnG8dcghE4qvk3JA9lGt8nDl**
### Level 10
Command injection. 
- - -
```bash
;cat ../../../../etc/natas_webpass/natas10
```
- - -
**PASSWORD 10 : nOpp1igQAkUzaI1GUUjzn1bFVj7xCNzu**
### Level 11 
This level was quite interesting, first I tried encoding this command *;cat ../../../../etc/natas_webpass/natas11* but it didn't work, I took a look to the source code again 
```
 if(preg_match('/[;|&]/',$key)) {
```
So it allows the slash, that means we could insert the file location with no problem, good!
```
grep -i $key dictionary.txt
```
(-i means case insensitive) 
Basically, this command search for $key in dictionary.txt, and we can insert our file location with no problem, What if we search for something in our file (I absolutely means /etc/natas_webpass/natas11) ? let's search for z for example ! 
Our payload will look like : 
```
z /etc/natas_webpass/natas11
```
Bingo ! 
Here's the password : U82q5TCMMQ9xuFoI3dYX61s7OZD9JKoK
