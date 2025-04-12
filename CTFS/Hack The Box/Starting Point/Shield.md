# Shield

## Nmap scan
```bash 
# Nmap 7.92 scan initiated Tue Sep 21 15:13:59 2021 as: nmap -sC -sV -p- -T4 -oN nmap/scan 10.10.10.29
Nmap scan report for 10.10.10.29 (10.10.10.29)
Host is up (0.27s latency).
Not shown: 65533 filtered tcp ports (no-response)
PORT     STATE SERVICE VERSION
80/tcp   open  http    Microsoft IIS httpd 10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
3306/tcp open  mysql   MySQL (unauthorized)
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Tue Sep 21 15:21:32 2021 -- 1 IP address (1 host up) scanned in 452.85 seconds
```


## Enumeration
### Fuzzing
`ffuf -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://10.10.10.29/FUZZ`
- Discover /wordpress directory
- 



```txt

username:sandra
domain: megacorp.local
password: Password1234!

NTLM: 29ab86c5c4d2aab957763e5c1720486d



```