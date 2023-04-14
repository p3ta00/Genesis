## NMAP
```
nmap -sCV -A -T4 172.16.1.15
Starting Nmap 7.93 ( https://nmap.org ) at 2023-04-14 11:58 PDT
Nmap scan report for 172.16.1.15
Host is up (0.078s latency).
Not shown: 989 closed tcp ports (conn-refused)
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2023-04-14 18:57:14Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: genesis-admin.local0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: genesis-admin.local0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped
Service Info: Host: DC02-TARTARUS; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: -56s
| smb2-time: 
|   date: 2023-04-14T18:57:19
|_  start_date: N/A
| smb2-security-mode: 
|   311: 
|_    Message signing enabled and required

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 22.86 seconds
```
