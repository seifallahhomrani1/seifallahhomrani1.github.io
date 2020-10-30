---
title: "OverTheWire - Bandit"
author : Seif-Allah
layout: post
category : CTF-WriteUp
Description : WriteUp for Bandit Wargame. Recommended for beginners. 
image : /assets/images/writeups/OTW/bandit/bandit_cover_sm2.png
---
### Basic Introduction : 
The [Bandit](https://overthewire.org/wargames/bandit/) wargame is aimed at absolute beginners. It will teach the basics needed to be able to play other wargames. 


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

Hidden files are the ones that their names starts with a dot. 
- - -
```bash 
bandit3@bandit:~/inhere$ ls -al
total 12
drwxr-xr-x 2 root    root    4096 May  7 20:14 .
drwxr-xr-x 3 root    root    4096 May  7 20:14 ..
-rw-r----- 1 bandit4 bandit3   33 May  7 20:14 .hidden
bandit3@bandit:~/inhere$ cat .hidden 
```
- - -
**PASSWORD LEVEL4 : pIwrPrtPN36QITSp3EQaw936yaFoFgAB**


### Level 4 -> Level 5 
Files that their names starts with a dash can be accessed using './xxxx' 
Using the cat command we can identify which file is human readable : File 7 

**PASSWORD LEVEL 5 : koReBOKuIDDepwhWk7jZC0RTdopnAYKh**

### Level 5 -> Level 6 

Using the find command to search for a file with a 1033c size and it's not executable : 
- - - 
```bash
bandit5@bandit:~/inhere$ find . -type f -size 1033c ! -executable 
./maybehere07/.file2
```
**PASSWORD LEVEL 6 : DXjZPULLxYr17uwoI01bNLQbtFemEgo7**

### Level 6 -> Level 7 

Using the information mentioned in the level description we can get the locate the file using the find command : 
- - -
```bash
bandit6@bandit:~$ find / -type f -size 33c -user bandit7 -group bandit6  2>/dev/null
/var/lib/dpkg/info/bandit7.password
```
- - -
**PASSWORD LEVEL 7 : HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs**

### Level 7 -> Level 8 

Piping the cat output into grep and searching for *millionth* word will print the password. 
- - - 
```bash
bandit7@bandit:~$ cat data.txt | grep millionth
millionth       cvX2JJa4CFALtqS87jk27qwqGhBM9plV
```
- - -
**PASSWORD LEVEL 8 : cvX2JJa4CFALtqS87jk27qwqGhBM9plV**

### Level 8 -> Level 9 

Sorting the output using sort command and piping it into uniq command so we can get the password from the file. 
- - -
```bash
bandit8@bandit:~$ cat data.txt | sort | uniq -u
UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR
```
- - -
**PASSWORD LEVEL9 : UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR**

### Level 9 -> Level 10 

In this level I just used the *strings* command to print all human-readable strings in the file. 

**PASSWORD LEVEL10 : truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk**

### Level 10 -> Level 11 

Using *base64* command with -d flag to get the password. 
- - -
```bash
bandit10@bandit:~$ base64 -d data.txt 
The password is IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR
```
- - -

**PASSWORD LEVEL11 : IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR**

### Level 11 -> Level 12 
In this level I used an external tool which is [CyberChef](https://gchq.github.io/CyberChef/#recipe=ROT13(true,true,55)&input=RWxkZXIgd2FuZDogbHJ6emY3angwbDM3YzBpdDNhODltMGFleA) to rotate the file content and get the password.

- - -
```bash 
bandit11@bandit:~$ cat data.txt 
Gur cnffjbeq vf 5Gr8L4qetPEsPk8htqjhRK8XSP6x2RHh

AFTER ROT13 :

The password is 5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu
```
- - -
**PASSWORD LEVEL12 : 5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu**

### Level 12 -> Level 13 
After doing a lot of decompression, here's the password : 
**PASSWORD LEVEL 13 : 8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL**

### Level 13 -> Level 14 
ssh bandit14@localhost -i sshkey.private
**PASSWORD LEVEL 14 : 4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e**

### Level 14 -> Level 15 
There's a server running in localhost waiting for us to send the current password to it. 
- - -
```bash
bandit14@bandit:~$ echo  '4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e' | nc -v localhost 30000
localhost [127.0.0.1] 30000 (?) open
Correct!
BfMYroe26WYalil77FoDi9qh59eK5xNr
```
- - -
**PASSWORD LEVEL 15 : BfMYroe26WYalil77FoDi9qh59eK5xNr**
### Level 15 -> Level 16
>OpenSSL comes with a client tool that you can use to connect to a secure server. The tool is similar to telnet or nc, in the sense that it handles the SSL/TLS layer but allows you to fully control the layer that comes next.
>To connect to a server, you need to supply a hostname and a port. For example:
```bash
$ openssl s_client -connect localhost:30001
```
**PASSWORD LEVEL 16 : cluFn7wTiGryunymYOu4RcffSxQluehd**

### Level 16 -> Level 17 
Doing the nmap scan using the *ssl-cert* script, we could identify which server speaks SSL, after giving it the current password, it responds with an *id_rsa*, so I save it inside a temporary directory and giving it the *400* permission and finally I used it to connect to the next level and retrieve the password ! 
**PASSWORD LEVEL 17 : xLYVMN9WE5zQ5vHacb0sZEVqbrp7nBTn**

### Level 17 -> Level 18
I've used the *diff* command to see the different passwords from the two files. 

**PASSWORDS LEVEL 18 : kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd**

### Level 18 -> Level 19 
Running the command from ssh will show you the password
- - - 
```bash
ssh bandit18@localhost cat readme
```
- - -
**PASSWORDS LEVEL 19 : IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x**


### Level 19 -> Level 20 
There's a SUID file inside home directory which is used to run commands as bandit20, so I used it to read the password. 
- - -
```bash 
bandit19@bandit:~$ ./bandit20-do 
Run a command as another user.
  Example: ./bandit20-do id
bandit19@bandit:~$ ./bandit20-do id
uid=11019(bandit19) gid=11019(bandit19) euid=11020(bandit20) groups=11019(bandit19)
bandit19@bandit:~$ ./bandit20-do cat /etc/bandit_pass/bandit20 
GbKksEFF4yrVs6il55v6gwY5aVje5f0j
```
- - -
**PASSWORD LEVEL 20 : GbKksEFF4yrVs6il55v6gwY5aVje5f0j**
### Level 20 -> Level 21 
For this level, I started an *nc listener* in the localhost that sends the current password after every connection. (I used tmux to be able to run multiple terminals)
- - -
```bash
bandit20@bandit:~$ echo GbKksEFF4yrVs6il55v6gwY5aVje5f0j | nc -nlvp 1234
listening on [any] 1234 ...
connect to [127.0.0.1] from (UNKNOWN) [127.0.0.1] 41732
gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr
```
- - -
**PASSWORD LEVEL 21 : gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr**

### Level 21 -> Level 22 
Looking at the cron file, it's clear that it print the password inside a temporary file every time the system reboots.
- - -
```bash
bandit21@bandit:/etc/cron.d$ cat cronjob_bandit22 
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
bandit21@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit22.sh 
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv


```
- - -
So I looked inside that temporary file to get the password : 
- - - 
```bash
bandit21@bandit:/etc/cron.d$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
Yk7owGAcWjwMVRwrTesJEwB7WVOiILLI
```
**PASSWORD LEVEL 22 : Yk7owGAcWjwMVRwrTesJEwB7WVOiILLI**

### Level 22 -> Level 23 
Same as previous level but this time the file is generated by md5sum. 
- - -
```bash 
bandit22@bandit:/etc/cron.d$ cat /usr/bin/cronjob_bandit23.sh
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```
- - -
So in order to find to find the file name I run the script locally after assigning *bandit23* to myname variable. 
- - -
```bash
bandit22@bandit:/etc/cron.d$ myname=bandit23
bandit22@bandit:/etc/cron.d$ echo I am user $myname | md5sum | cut -d ' ' -f 1
8ca319486bfbbc3663ea0fbe81326349
bandit22@bandit:/etc/cron.d$ cat /tmp/8ca319486bfbbc3663ea0fbe81326349
jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n
```
- - -
**PASSWORD LEVEL 23: jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n**

### Level 23 -> Level 24 
For this level, you have to make a simple script inside /var/spool/bandit24 directory 
- - -
```bash 
!#/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/txt.txt
```
- - -
Then we need to make it executable using *chmod +x* after that our password will be in txt.txt file. 
**PASSWORD LEVEL 24 : UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ**

### Level 24 -> Level 25 
For this level, we need a brute forcing script to find the 4-digit pincode. 
- - -  
```python
#!/usr/bin/env python3
# coding: utf-8import sys
import socketpincode = 0
password = "UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ"try:
    # Connect to server
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.connect(("127.0.0.1", 30002))
    
    # Print welcome message
    welcome_msg = s.recv(2048)
    print(welcome_msg)    # Try brute-forcing
    while pincode < 10000:
        pincode_string = str(pincode).zfill(4)
        message=password+" "+pincode_string+"\n"        # Send message
        s.sendall(message.encode())
        receive_msg = s.recv(1024)        # Check result
        if "Wrong" in receive_msg:
            print("Wrong PINCODE: %s" % pincode_string)
        else:
            print(receive_msg)
            break
        pincode += 1
finally:
    sys.exit(1)

```
- - -
By making it executable and running it, we can retrieve the pincode.

**PASSWORD LEVEL 25 : uNG9O58gUE7snukf3bvZ0rxhtnjzSGzG**

### Level 25 -> Level 26 
Looking at the /etc/passwd file, the Bandit26 is running /usr/bin/showtext script as a shell. 
- - - 
```bash
bandit25@bandit:~$ cat /usr/bin/showtext
#!/bin/sh

export TERM=linux

more ~/text.txt
exit 0
```
- - -
That means that we cannot actually run any command after logging in with the private key, So we need to find a vulnerability inside the script to use it and gain a shell. 
As mentioned in [SANS Article](https://www.sans.org/blog/escaping-restricted-linux-shells/), we can use more to run vim and then get the shell. 
This means that we have to resize the currently used terminal window in size and make it so small to enter the more environment. 
Once done, hit h to open the help menu. Resize the terminal window an take a look at the possible options.

The most interesting command here is v, which will start the editor vi at the current line. 
Type in :set shell=/bin/bash within vi and type in :shell then.

**PASSWORD LEVEL 26 : 5czgV9L3Xx8JPOyRbXh6lQbmIOWvPT6Z**

### Level 26 -> Level 27 

- - - 
```bash
bandit26@bandit:~$ ./bandit27-do 
Run a command as another user.
  Example: ./bandit27-do id
bandit26@bandit:~$ ./bandit27-do id
uid=11026(bandit26) gid=11026(bandit26) euid=11027(bandit27) groups=11026(bandit26)
bandit26@bandit:~$ ./bandit27-do cat /etc/bandit_pass/bandit27
3ba3118a22e93127a4ed485be72ef5ea
```
- - -
Easy. 
**PASSWORD LEVEL 27: 3ba3118a22e93127a4ed485be72ef5ea**

### Level 27 -> Level 28
For this level we need to clone the repository inside a temporary directory. 
- - -
```bash
git clone ssh://bandit27-git@localhost/home/bandit27-git/repo
```
- - -
**PASSWORD LEVEL 28 : 0ef186ac70e04ea33b4c1853d2526fa2**

### Level 28 -> Level 29 
I did the same as the previous level, but this time the password was removed from the README file, so I checked the log file and used -p flag to see the difference between the commits. 
- - -
```bash
$ git log -p -1
commit edd935d60906b33f0619605abd1689808ccdd5ee
Author: Morla Porla <morla@overthewire.org>
Date:   Thu May 7 20:14:49 2020 +0200

    fix info leak

diff --git a/README.md b/README.md
index 3f7cee8..5c6457b 100644
--- a/README.md
+++ b/README.md
@@ -4,5 +4,5 @@ Some notes for level29 of bandit.
 ## credentials
 
 - username: bandit29
-- password: bbc96594b4e001778eee9975372716b2
+- password: xxxxxxxxxx
```
- - -
**PASSWORD LEVEL 29 : bbc96594b4e001778eee9975372716b2** 

### Level 29 -> Level 30


After looking at the *README* file, it looks like there's an another branch out there, so I ran  *git branch -a* to list all branches. 
Changing to branch *dev* and running git show to show all commits.


- - -
```bash
$ git branch -a
* dev
  master
  production
  remotes/origin/HEAD -> origin/master
  remotes/origin/dev
  remotes/origin/master
  remotes/origin/sploits-dev

$ git checkout dev
Branch dev set up to track remote branch dev from origin.
Switched to a new branch 'dev'


$ git show
commit bc833286fca18a3948aec989f7025e23ffc16c07
Author: Morla Porla <morla@overthewire.org>
Date:   Thu May 7 20:14:52 2020 +0200

    add data needed for development

diff --git a/README.md b/README.md
index 1af21d3..39b87a8 100644
--- a/README.md
+++ b/README.md
@@ -4,5 +4,5 @@ Some notes for bandit30 of bandit.
 ## credentials
 
 - username: bandit30
-- password: <no passwords in production!>
+- password: 5b90576bedb2cc04c86a9e924ce42faf
```
- - -
**PASSWORD LEVEL 30 : 5b90576bedb2cc04c86a9e924ce42faf** 



### Level 30 -> Level 31 

This time *README* file and log file has nothing to show, so I tried to see *tagging*  
- - -
```bash
$ git tag 
secret
$ git show secret 
47e603bb428404d265f59c42920d81e5
```
- - - 
**PASSWORD LEVEL 31 : 47e603bb428404d265f59c42920d81e5**

### Level 31 -> Level 32 
Details are in README file. 
- - - 
```bash
andit31@bandit:/tmp/coffee32/repo$ git add key.txt -f
bandit31@bandit:/tmp/coffee32/repo$ git commit -a -m 'key file'
[master 6f5fbec] key file
 1 file changed, 1 insertion(+)
 create mode 100644 key.txt
bandit31@bandit:/tmp/coffee32/repo$ git push
Could not create directory '/home/bandit31/.ssh'.
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit31/.ssh/known_hosts).
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit31-git@localhost's password: 
Counting objects: 3, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 320 bytes | 0 bytes/s, done.
Total 3 (delta 0), reused 0 (delta 0)
remote: ### Attempting to validate files... ####
remote: 
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote: 
remote: Well done! Here is the password for the next level:
remote: 56a9bf19c63d650ce78e6ec0354ee45e
remote: 
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote: 
To ssh://localhost/home/bandit31-git/repo
 ! [remote rejected] master -> master (pre-receive hook declined)
error: failed to push some refs to 'ssh://bandit31-git@localhost/home/bandit31-git/repo'
```
- - -
**PASSWORD LEVEL 32 : 56a9bf19c63d650ce78e6ec0354ee45e**

### Level 32 -> Level 33 
So after logging in, it appears like everything we type is converted to uppercase (STDIN -> UPPERCASE (STDIN) -> STDOUT) so typing $0 will let us type directly to /bin/bash
> $0 Expands to the name of the shell or shell script. This is set at shell initialization. 

- - -
```bash
WELCOME TO THE UPPERCASE SHELL
>> $0
whoami
bandit33
cat /etc/bandit_pass/bandit33
c9c3199ddf4121b10cf581a98d51caee
```
- - -
**PASSWORD LEVEL 33 : c9c3199ddf4121b10cf581a98d51caee**

### LEVEL 33 
- - -
```bash
bandit33@bandit:~$ cat README.txt 
Congratulations on solving the last level of this game!

At this moment, there are no more levels to play in this game. However, we are constantly working
on new levels and will most likely expand this game with more levels soon.
Keep an eye out for an announcement on our usual communication channels!
In the meantime, you could play some of our other wargames.

If you have an idea for an awesome new level, please let us know!
```
- - -
