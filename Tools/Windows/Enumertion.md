
## smbclient
used to enumerate SMB drives
```bash
#list direcroty
smbclient -L //10.10.11.222/

#connect to directory with no credentials
smbclient -N //10.10.11.222/directory
#to get all files in that direcortu
mask ""
recurse ON
prompt OFF
mget *

```


## enum4linux
can enumerate users and domains and drives


## crackmapexec
can brute force winrm , ssh ,mssql, rdp, smb, ldap