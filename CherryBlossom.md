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

```

---
## Initial Foothold
---

Initial Foothold Notes

---
## Privilege Escalation
----
