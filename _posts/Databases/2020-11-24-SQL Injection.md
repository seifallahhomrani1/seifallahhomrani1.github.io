---
title: "SQLi - SQL Injection"  
author: Seif-Allah
layout: post
Description : Introduction to SQL Injection
category : Databases
image: /assets/images/database/sql.png
---

<br>

<img src="/assets/images/database/sql.png" style="  width: 100%; max-width: 600px; height: auto;"/>

### What is SQL injection 

SQL injection is a *web security* vulnerability that allows an attacker to interfere with the queries that an application makes to its database. It generally allows an attacker to view data that they are not normally able to retrieve. This might include data belonging to other users, or any other data that the application itself is able to access. In many cases, an attacker can modify or delete this data, causing persistent changes to the application content or behavior. In some situations, an attacker can escalate an SQL injection attack to compromise the underlying server or other back-end infrastructure, or perform a denial-of-service attack.

<iframe width="560" height="315" src="https://www.youtube.com/embed/_jKylhJtPmI" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


### Results of a successful SQL injection attack 

The attacker can access to sensitive data (passwords, credit card details, personal information ...) Many high-profile data breaches in recent years have been the result of SQL injection attacks, leading to reputational damage and regulatory fines. In some cases, an attacker can obtain a persistent backdoor into an organization's systems, leading to a long-term compromise that can go unnoticed for an extended period.


### SQL injection examples 
There are a wide variety of SQL injection vulnerabilities, attacks and techniques, which arise in different situations. Some common SQL injection examples include: 

- **Retrieving hidden data**: where you can modify an SQL query to return additional results. 



- **Subverting application logic**: where you can change a query to interfere with the application's logic.
> Lab : [Subverting application logic](https://portswigger.net/web-security/sql-injection/lab-login-bypass)
Consider an application that lets users log in with a username and password. If a user submits the username *wiener* and the password *bluecheese*, the application checks he credentials by performing the following SQL query: 
```sql 
SELECT * FROM users WHERE username = 'wiener' AND password = 'bluecheese'  
```
If the query returns the details of a user, then the login is successful, otherwise, it's rejected.

Here, an attacker can log in as any user without a password simply by using the SQL comment sequence -- to remove the password check from the WHERE clause of the query. For example, submitting the username *administrator'-- and a blank password results in the following query: 
```sql 
SELECT * FROM users WHERE username = 'administrator'--'AND password = ''
```
This query returns the user whose username is administrator and successfully logs th attacker in as that user.

- Union attacks: where you can retrieve data from different database tables.

In cases where the results of an SQL query are returned within the application's responses, an attacker can leverage an SQL injection vulnerability to retrieve data from other tables within the database. This is done using the UNION keyword, which lets you execute an additional SELECT query and append the results to the original query. 

For example, if an application executes the following query containing the user input "Gifts": 
```sql
SELECT name, description FROM products WHERE category = 'Gifts'
```
then an attacker can submit the input:
' UNION SELECT username, password FROM users--
This will cause the application to return all usernames and passwords along with the names and descriptions of products.


To carry out an SQL injection UNION attack, you need to ensure that your attack meets these two requirements. This generally involves figuring out: 
- How many columns are being returned from the original query ? 
- Which columns returned from the original query are of a suitable data type to hold the results from the injected query ?

1. *Determining the number of columns required in an SQL injection UNION attack* : 
First of all, you can inject a series of ORDER BY clauses and incrementing the specified column index until an error occurs. For example, assuming the injection point is a quotes string within the WHERE clause of the original query, you would submit: 
' ORDER BY 1--
' ORDER BY 2--
' ORDER BY 3--
' ORDER BY 4--
Until the database returns an error such as :
*The ORDER BY position number 3 is out of range of items in the select list.*
The second method involves submitting a series of UNION SELECT payloads specifying a different number of null values:
' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
' UNION SELECT NULL,NULL,NULL--
etc.

If the number of nulls does not match the number of columns, the database returns an error such as:
*All queries combined using a UNION, INTERSECT or EXCEPT operator must have an equal number of expressions in their target lists.*


Again, the application might actually return this error message, or might just return a generic error or no results. When the number of nulls matches the number of columns, the database returns an additional row in the result set, containing null values in each column. The effect on the resulting HTTP response depends on the application's code. If you are lucky, you will see some additional content within the response, such as an extra row on a HTML table. Otherwise, the null values might trigger a different error, such as a *NullPointerException*. Worst case, the response might be indistinguishable from that which is caused by an incorrect number of nulls, making this method of determining the column count ineffective.

### Why using NULL ?

The reason for using NULL as the values returned from the injected SELECT query is that the data types in each column must be compatible between the original and the injected queries. Since NULL is convertible to every commonly used data type, using NULL maximizes the chance that the payload will succeed when the column count is correct. 

### Oracle Syntax 
On Oracle, every SELECT query must use the FROM keyword and specify a valid table. There is a built-in table on Oracle Called DUAL which can be used for this purpose. So the injected queries on Oracle would need to look like : ' UNION SELECT NULL FROM DUAL--
### Comments 
The payloads described use the double-dash comment sequence -- to comment out the remainder of the original query following the injection point. On MySQL, the double-dash sequence must be followed by a space. Alternatively, the hash character # cna be used to identify a comment.



