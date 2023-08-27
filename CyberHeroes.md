# Box Name

## CyberHeroes

## Overview
---
* [[#Enumeration]]
	* nmap

* [[#Initial Foothold]]

* [[#Privilege Escalation]]

---
## Enumeration
---

 - ## nmap
```bash
┌──(kn1ght㉿Kali)-[~/THM/CyberHeroes]
└─$ rustscan -a 10.10.105.28 --ulimit 5000 -- -sCV -oN nmap/initial 
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
Open 10.10.105.28:22
Open 10.10.105.28:80
[~] Starting Script(s)
[>] Script to be run Some("nmap -vvv -p {{port}} {{ip}}")

PORT   STATE SERVICE REASON  VERSION
22/tcp open  ssh     syn-ack OpenSSH 8.2p1 Ubuntu 4ubuntu0.4 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 c9:3d:5c:db:30:2e:c0:75:92:d7:42:4b:11:d2:cb:77 (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC5TGQktMwtVsLkGN//lzeUqB/waZRlpH7oX6QYSXxqpxnzGBveEzvwgo4XxACWLiWHs3GAH/+t5aN8QRo9XxSIISmuvqCuItpL5xWZ1IqUM+Qt/qQGg0CH2sbuIhaYJTMH41aXEWAzto80tYXfhns+9ikKj/3U02NoFcRL83BFWzX5y6hiSpznBUoPYYiv7umYgCp6Vvy7VWI6d6TE5IgNHXuLJ7wqdUX0RgzHWtfR1nmyZnM0emi/nCIEqG6P8wLZjsn3bH5InZbHteuLSJuSeWtn2pObNM57IrtpSOhNBHsAi2bffQUgRV17NlM2JXliEc+3MDgJW7bPvezhKiM9FMFSQAxfsnmRTbBQRLhiOEkvyly791LAVTW8lb1dC5Z6gNIlxzmi3HC1thiZ1XoJZ4YeO6IhEj4TJD/fiSAupDZSTzJtbKhIGiW/SD15GtALhioFbMiyiL/Z4FiWcj1+RcuaQz7ZaUqxUpg7Yb0il2/ElEukTb1o9nRvWiU2QJ0=
|   256 69:99:59:6e:f0:93:10:b3:a9:55:f5:f6:22:38:19:d3 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBAPdnSFzLxeHwunlbM0lYNWPUWOGB3mrW1+mP6Nf0V45JeYkIegjxy88ljiElfUW8WzkRJcilmZQsd8C+Kt1cQA=
|   256 9f:20:8d:3c:21:30:80:05:52:47:12:a1:44:d8:9d:e8 (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIAcAuUPNtUj8CUzCjK6kvutJ1fxNeqmVERYoegwdMm55
80/tcp open  http    syn-ack Apache httpd 2.4.48 ((Ubuntu))
| http-methods: 
|_  Supported Methods: OPTIONS HEAD GET POST
|_http-title: CyberHeros : Index
|_http-favicon: Unknown favicon MD5: 03983666D3C4B72ECAAB464BD200E6FA
|_http-server-header: Apache/2.4.48 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel


```

- ## HTTP 
![[Pasted image 20220527172853.png]]

![[Pasted image 20220527172949.png]]

## view-source:http://10.10.105.28/login.html

![[Pasted image 20220527172639.png]]

- ## login with creds h3ck3rBoi:SuperSecret@12345

![[Pasted image 20220527173113.png]]

## Initial Foothold
---

# NA

---
## Privilege Escalation
----
# NA