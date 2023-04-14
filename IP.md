## IP list
```
10.10.110.3 - Completed
10.10.110.5 - Completed
10.10.110.10 - Completed
10.10.110.12 - Completed
10.10.110.20 - Completed
10.10.110.25 - Completed
10.10.110.45 - Completed
10.10.110.58 - Completed
10.10.110.60 - Completed
10.10.110.78 - Completed
10.10.110.102 - Completed
10.10.110.119 - Completed
10.10.110.205 - Completed
10.10.110.213 - Completed
10.10.110.254
```
## Password Enumeration
```
crackmapexec smb 10.10.110.0/24 -u mrb3n -p 'W3lc0me123!!!'
SMB         10.10.110.58    445    SQL01-HERA       [*] Windows Server 2016 Standard 14393 x64 (name:SQL01-HERA) (domain:SQL01-Hera) (signing:False) (SMBv1:True)
SMB         10.10.110.3     445    DC01-PHOBOS      [*] Windows 10.0 Build 17763 x64 (name:DC01-PHOBOS) (domain:genesis.local) (signing:True) (SMBv1:False)
SMB         10.10.110.20    445    WS01-ERIS        [*] Windows 10.0 Build 18362 x64 (name:WS01-ERIS) (domain:WS01-Eris) (signing:False) (SMBv1:False)
SMB         10.10.110.5     445    SRV01-ARTEMIS    [*] Windows 10.0 Build 17763 x64 (name:SRV01-ARTEMIS) (domain:SRV01-Artemis) (signing:False) (SMBv1:False)
SMB         10.10.110.25    445    WS02-ATHENA      [*] Windows 10.0 Build 19041 x64 (name:WS02-ATHENA) (domain:WS02-Athena) (signing:False) (SMBv1:False)
SMB         10.10.110.58    445    SQL01-HERA       [+] SQL01-Hera\mrb3n:W3lc0me123!!! 
SMB         10.10.110.3     445    DC01-PHOBOS      [+] genesis.local\mrb3n:W3lc0me123!!! 
SMB         10.10.110.20    445    WS01-ERIS        [-] WS01-Eris\mrb3n:W3lc0me123!!! STATUS_LOGON_FAILURE 
SMB         10.10.110.5     445    SRV01-ARTEMIS    [+] SRV01-Artemis\mrb3n:W3lc0me123!!! 
SMB         10.10.110.25    445    WS02-ATHENA      [+] WS02-Athena\mrb3n:W3lc0me123!!! 
SMB         10.10.110.213   445    WEBWIN01-APOLLO  [*] Windows Server 2012 R2 Standard 9600 x64 (name:WEBWIN01-APOLLO) (domain:WEBWIN01-Apollo) (signing:False) (SMBv1:True)
SMB         10.10.110.213   445    WEBWIN01-APOLLO  [-] WEBWIN01-Apollo\mrb3n:W3lc0me123!!! STATUS_LOGON_FAILURE 
```
## Shares
```
SMB         10.10.110.5     445    SRV01-ARTEMIS    [+] SRV01-Artemis\mrb3n:W3lc0me123!!! 
SMB         10.10.110.58    445    SQL01-HERA       [+] Enumerated shares
SMB         10.10.110.58    445    SQL01-HERA       Share           Permissions     Remark
SMB         10.10.110.58    445    SQL01-HERA       -----           -----------     ------
SMB         10.10.110.58    445    SQL01-HERA       ADMIN$                          Remote Admin
SMB         10.10.110.58    445    SQL01-HERA       C$                              Default share
SMB         10.10.110.58    445    SQL01-HERA       IPC$                            Remote IPC
SMB         10.10.110.58    445    SQL01-HERA       Users           READ            
SMB         10.10.110.58    445    SQL01-HERA       zach            READ            
SMB         10.10.110.25    445    WS02-ATHENA      [+] WS02-Athena\mrb3n:W3lc0me123!!! 
SMB         10.10.110.20    445    WS01-ERIS        [-] WS01-Eris\mrb3n:W3lc0me123!!! STATUS_LOGON_FAILURE 
SMB         10.10.110.3     445    DC01-PHOBOS      [+] Enumerated shares
SMB         10.10.110.3     445    DC01-PHOBOS      Share           Permissions     Remark
SMB         10.10.110.3     445    DC01-PHOBOS      -----           -----------     ------
SMB         10.10.110.3     445    DC01-PHOBOS      ADMIN$                          Remote Admin
SMB         10.10.110.3     445    DC01-PHOBOS      Backups         READ            
SMB         10.10.110.3     445    DC01-PHOBOS      C$                              Default share
SMB         10.10.110.3     445    DC01-PHOBOS      IPC$            READ            Remote IPC
SMB         10.10.110.3     445    DC01-PHOBOS      NETLOGON        READ            Logon server share 
SMB         10.10.110.3     445    DC01-PHOBOS      Software        READ            
SMB         10.10.110.3     445    DC01-PHOBOS      SYSVOL          READ            Logon server share 
SMB         10.10.110.5     445    SRV01-ARTEMIS    [+] Enumerated shares
SMB         10.10.110.5     445    SRV01-ARTEMIS    Share           Permissions     Remark
SMB         10.10.110.5     445    SRV01-ARTEMIS    -----           -----------     ------
SMB         10.10.110.5     445    SRV01-ARTEMIS    ADMIN$                          Remote Admin
SMB         10.10.110.5     445    SRV01-ARTEMIS    C$                              Default share
SMB         10.10.110.5     445    SRV01-ARTEMIS    Documents       READ,WRITE      
SMB         10.10.110.5     445    SRV01-ARTEMIS    IPC$            READ            Remote IPC
SMB         10.10.110.5     445    SRV01-ARTEMIS    Users           READ            
SMB         10.10.110.25    445    WS02-ATHENA      [+] Enumerated shares
SMB         10.10.110.25    445    WS02-ATHENA      Share           Permissions     Remark
SMB         10.10.110.25    445    WS02-ATHENA      -----           -----------     ------
SMB         10.10.110.25    445    WS02-ATHENA      ADMIN$                          Remote Admin
SMB         10.10.110.25    445    WS02-ATHENA      backup          READ,WRITE      
SMB         10.10.110.25    445    WS02-ATHENA      C$                              Default share
SMB         10.10.110.25    445    WS02-ATHENA      IPC$            READ            Remote IPC
SMB         10.10.110.25    445    WS02-ATHENA      Users           READ 
```
## admin.ovpn
```
sudo openvpn admin.ovpn
[sudo] password for p3ta: 
Options error: Unrecognized option or missing or extra parameter(s) in admin.ovpn:4: ncp-disable (2.6.0)
Use --help for more information.
```
```
sed -i.bak '/ncp-disable/d' admin.ovpn
```
We now have a tun1
```
tun1: flags=4305<UP,POINTOPOINT,RUNNING,NOARP,MULTICAST>  mtu 1500
        inet 10.0.1.10  netmask 255.255.255.0  destination 10.0.1.10
        inet6 fe80::718f:beb1:b84e:ce3c  prefixlen 64  scopeid 0x20<link>
        unspec 00-00-00-00-00-00-00-00-00-00-00-00-00-00-00-00  txqueuelen 500  (UNSPEC)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
## Routes
```
route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         192.168.1.1     0.0.0.0         UG    100    0        0 eth0
10.0.1.0        0.0.0.0         255.255.255.0   U     0      0        0 tun1
10.10.14.0      0.0.0.0         255.255.254.0   U     0      0        0 tun0
10.10.110.0     10.10.14.1      255.255.255.0   UG    0      0        0 tun0
172.16.1.0      10.0.1.1        255.255.255.0   UG    0      0        0 tun1
172.16.216.0    0.0.0.0         255.255.255.0   U     0      0        0 vmnet1
172.17.0.0      0.0.0.0         255.255.0.0     U     0      0        0 docker0
192.168.1.0     0.0.0.0         255.255.255.0   U     100    0        0 eth0
192.168.176.0   0.0.0.0         255.255.255.0   U     0      0        0 vmnet8
```
Lets do a ping sweep on 172.16.1.0/24
```
172.16.1.1
172.16.1.11
172.16.1.15
172.16.1.19
172.16.1.52
```


