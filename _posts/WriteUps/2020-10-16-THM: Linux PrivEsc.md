---
title : TryHackMe: Linux Privelage Escalation | Service Exploits
author : Seif-Allah
description : Linux PrivEsc using running servives
category : CTF-WriteUp
layout : post
---
### <mark style='background-color:white'>Exploiting any service which is running as root will give you Root!</mark>
The famous EternalBlue and SambaCry exploit, exploited smb service which generally runs as root.
In our case, I ran *ps -aux* command and found out that 
- - - 
root      1714  0.0  4.7 163420 24120 ?        Sl   12:41   0:00 /usr/sbin/mysqld --basedir=/usr --datadir=/var/lib/mysql --user=root --pid-file=/var/run/mysqld/mysql
- - - 
So as mentioned, we can use a famous exploit that takes advantage of User Defined Functions (UDFs) to run system commands as root via the MySQL service.
MySQL UDF Dynamic Library exploit lets you execute arbitrary commands from the mysql shell. If mysql is running with root privileges, the commands will be executed as root. 
