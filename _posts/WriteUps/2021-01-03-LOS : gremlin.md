---
title: "Lord Of SQLi : gremlin Writeup"  
author: Seif-Allah
layout: post
Description : "'Lord of SQL Injection', a site where you can learn about SQL injection vulnerabilities while capturing dungeons'"
category : CTF-WriteUp
image: /assets/images/writeups/los/sql1.png
---
<br>

<img src="/assets/images/writeups/los/sql1.png" style="  width: 100%; max-width: 600px; height: auto;"/>

**Resources :** [SQL INJECTION - Introduction](/databases/2020/11/24/SQL-Injection.html)

### Introduction 
'Lord of SQL Injection', a site where you can learn about SQL injection vulnerabilities while capturing dungeons. 
LoS provides 49 stepwise SQLinjection challenges.
Challenges are about SQLinjection at mysql, sqlite, mssql, mongodb, webapp what protected by WAF.
- Site origin Country : Korea
- Language : English 


### Gremlin 
Given a php code : 
- - -
```php
 <?php
  include "./config.php";
  login_chk();
  $db = dbconnect();
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[id])) exit("No Hack ~_~"); // do not try to attack another table, database!
  if(preg_match('/prob|_|\.|\(\)/i', $_GET[pw])) exit("No Hack ~_~");
  $query = "select id from prob_gremlin where id='{$_GET[id]}' and pw='{$_GET[pw]}'";
  echo "<hr>query : <strong>{$query}</strong><hr><br>";
  $result = @mysqli_fetch_array(mysqli_query($db,$query));
  if($result['id']) solve("gremlin");
  highlight_file(__FILE__);
?> 
```
- - -
### Explanation 

-> *Challenge preparation*
First of all, it includes this php file called *config.php*, then it checks if you are logged in or not. After that, it connect to the database. 

-> *The fun begins*
[preg_match](https://www.php.net/manual/en/function.preg-match.php) is a php function that performs a regular expression match. If the provided regex found in the *id* or *pw* fields it exits with "No Hack ~_~" message? 


-> *REGEX explanation*
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


Then, with the provided *id* and *pw*, the SQL query is ready to be executed, and it is printed out using the following *echo*. 
Using the [mysqli_fetch_array](https://www.php.net/manual/en/mysqli-result.fetch-array.php) function, which fetch a result row as an associative, a numeric array, or both.

Finally, it checks if the id has a value, if it has, the challenge is solved ! 

I just want to mention that the *id* and *pw* field are gonna be passed in the url like this : ?id=
If you've read my article mentioned above, you can solve this challenge easily by closing the id field using an apostrophe then executing a condition that returns true as a result like 1=1. 
So the final result will be : 
?id=' OR 1=1 
Then we need to comment out the *pw* field so we insert a double dash followed by a space (Black Space = %20 : URL encoding) or a hashtag(hashtag = %23 : URL encoding )
?id=' OR 1=1 --%20
So the final query will be  : 
```sql
select id from prob_gremlin where id='' OR 1=1 -- ' and pw=''
```
GREMLIN CLEAR ! 
