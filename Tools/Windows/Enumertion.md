
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


## Kerbrute
we can use the tool for the following:
- **bruteuser** - Bruteforce a single user's password from a wordlist
- **bruteforce** - Read username:password combos from a file or stdin and test them
- **passwordspray** - Test a single password against a list of users
- **userenum** - Enumerate valid domain usernames via Kerberos
```bash
./kerbrute_linux_amd64 userenum -d lab.ropnop.com usernames.txt
```
