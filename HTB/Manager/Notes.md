from the [[[Nmap scans]]  we see the domain name:==Manager.htb==


using [[Enumertion|Crackmapexec]]  we found the following users:
``` bash
crackmapexec smb 10.10.11.236 -u anonymous -p ""  --rid-brute 10000
```
users:
``` note
Zhong
Cheng
Ryan
Raven
JinWoo
ChinHae
Operator
```

