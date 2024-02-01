# Pickle Rick
## Nmap
``` bash
PORT   STATE SERVICE VERSION                                                                                             
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.6 (Ubuntu Linux; protocol 2.0)                                        
| ssh-hostkey:                                                                                                           
|   2048 a1:42:43:8c:6e:e9:01:62:db:07:92:8b:1b:72:d3:d9 (RSA)                                                           
|   256 ec:2c:a1:71:d9:98:4d:00:75:cc:23:de:08:94:68:ad (ECDSA)                                                          
|_  256 82:d3:1d:ab:19:1b:59:34:c7:09:57:91:b8:c2:f0:46 (ED25519)                                                                                                                                                                            
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))                                                                      
|_http-server-header: Apache/2.4.18 (Ubuntu)                                                                             
|_http-title: Rick is sup4r cool                                                                                                                                                                                                             
Warning: OSScan results may be unreliable because we could not find at least 1 open and 1 closed port                                                       
Aggressive OS guesses: Linux 3.10 - 3.13 (95%), Linux 5.4 (95%), ASUS RT-N56U WAP (Linux 3.4) (95%), Linux 3.16 (95%), Linux 3.1 (93%), Linux 3.2 (93%), AXIS 210A or 211 Network Camera (Linux 2.6.17) (92%), Sony Android TV (Android 5.0) 
(92%), Android 5.0 - 6.0.1 (Linux 3.4) (92%), Android 5.1 (92%)                                                                                                                                                                              
No exact OS matches for host (test conditions non-ideal).                                                                                                                                                                                    
Network Distance: 2 hops                                                                                                                                                                                                                     
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

## Enumerating the website 
- Username found in source code: R1ckRul3s
- Using gobuster we found `/login.php` and `/robots.txt`
- Try to login with username and random string found in robots.txt, we get access to panel
- We can run commands 


## Foothold
- Reverse shell with bash
- Read first flag there
- Read second flag in /home/rick

## Priv Esc
- Running sudo -l, we can run all commands as root
- Read flag in root folder with `sudo cat /root/3rd.txt`





