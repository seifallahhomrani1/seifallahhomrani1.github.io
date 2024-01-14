---
title: "SQLi - Retrieving Hidden Data"  
author: Seif-Allah
layout: post
description : Retrieving Hidden Data using SQL injection
category : Databases
image: /assets/images/database/sql.png
---

> Lab : [Retrieving hidden data](https://portswigger.net/web-security/sql-injection/lab-retrieve-hidden-data)


Let's take this SQL query as an example : 
```sql
SELECT * FROM products WHERE category = 'Gifts' AND released = 1 
```
This SQL query asks the database to return: 

* all details
* from the products table
* where the category is Gifts 
* and released is 1

The restriction *released = 1* is being used to hide products that are not released. For unreleased products, presumably released = 0.


The application doesn't implement any defenses against SQL injection attacks (like input validation, parametrized queries, etc...), so an attacker can construct an attack like : 
```sql 
SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1 
```
The key thing here is that the double-dash sequence -- is a comment indicator in SQL, and means that the rest of the query is interpreted as a comment. This effectively removes the remainder of the query, so it no longer includes *AND released = 1*. This means taht all products are displayed, including unreleased products.

Going further, an attacker can cause the application to display all the products in any category, including categories that they don't know about, by submitting this url : 
https://insecure-website.com/products?category=Gifts'+OR+1=1-- 
This results in the SQL query: 
```sql
SELECT * FROM products WHERE category = 'Gifts' OR 1=1' -- AND released = 1
```
The modified query will return all items where either the category is Gifts, or 1 is equal to 1. Since 1=1 is always true, the query will return all items.
