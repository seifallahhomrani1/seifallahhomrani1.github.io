---
title: "Lord Of SQLi : goblin Writeup"  
author: Seif-Allah
layout: post
Description : "'Lord of SQL Injection', a site where you can learn about SQL injection vulnerabilities while capturing dungeons'"
category : CTF-WriteUp
image: /assets/images/writeups/los/goblin.png
---

<br>

<img src="/assets/images/writeups/los/goblin.png" style="  width: 100%; max-width: 600px; height: auto;"/>

**Previous WriteUp : [gremlin](/ctf-writeup/2021/01/03/LOS-gremlin.html)

### Provided code 
```php
<?php 
  //Challenge Preparation
  include "./config.php"; 
  login_chk(); 
  $db = dbconnect(); 
  // - - - - - - 
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[no])) exit("No Hack ~_~"); 
  if(preg_match('/\'|\"|\`/i', $_GET[no])) exit("No Quotes ~_~"); 
  $query = "select id from prob_goblin where id='guest' and no={$_GET[no]}"; 
  echo "<hr>query : <strong>{$query}</strong><hr><br>"; 
  $result = @mysqli_fetch_array(mysqli_query($db,$query)); 
  if($result['id']) echo "<h2>Hello {$result[id]}</h2>"; 
  if($result['id'] == 'admin') solve("goblin");
  highlight_file(__FILE__); 
?>
```

### Explanation 

-> *Challenge preparation*
First of all, it includes this php file called *config.php*, then it checks if you are logged in or not. After that, it connect to the database. 

-> *no field check* 
There's a double check for the *no* field, firstly it checks for : 

-> *First REGEX explanation*
/prob\|\_|\.|\(\)/i
**1st Alternative prob**
prob matches the characters prob literally (case insensitive)
**2nd Alternative _**
_ matches the character _ literally (case insensitive)
**3rd Alternative \.**
\. matches the character . literally (case insensitive)
**4th Alternative \(\)**
\( matches the character ( literally (case insensitive)
\) matches the character ) literally (case insensitive)
**Global pattern flags**
i modifier: insensitive. Case insensitive match (ignores case of [a-zA-Z])

-> *Second REGEX explanation*
/\'|\"|\`/i
1st Alternative \'
\' matches the character ' literally (case insensitive)
2nd Alternative \"
\" matches the character " literally (case insensitive)
3rd Alternative \`
\` matches the character ` literally (case insensitive)
Global pattern flags
i modifier: insensitive. Case insensitive match (ignores case of [a-zA-Z])


Then it executes the final query, and if result id == admin then the problem is solved, else, it returns "Hello {$result[id]}"

### Solution 

As mentioned before, we can't use quotes, but we can use another methods like : 

### SUBSTR() 
The [SUBSTR()](https://www.w3schools.com/sql/func_mysql_substr.asp) function extracts a substring from a string (starting at any position). 
The main idea is that we extract the first character of the id field that matches if it equals 97 (ASCII character of 'a'). 


-> *Payload* : /?no=0 or ascii(substr(id,1,1))=97


### ORDER BY
If we use this function, SQL will sort data by column according to asc. 

-> *Payload* : /?no=2 or 1 order by id

### LIMIT 1,1
This function will cut the rows from offset-index to (offset+amount)-index.

Start of index is 0,0. 

-> *Payload* : /?no=0 or 1 limit 1,1


Resources : [PortSwigger - Bypassing Common filters](https://portswigger.net/support/sql-injection-bypassing-common-filters)