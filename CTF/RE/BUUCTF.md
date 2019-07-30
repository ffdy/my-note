## CrackRTF
```python
from hashlib import sha1
ans='6e32d0943418c2c33385bc35a1470250dd8923a9'
bns=''
b='@DBApp'
a=''
c=''
for i in range(1000000):
    c=str(i)
    a=''
    if(len(c)<6):
        a+='0'*(6-len(c))
    a+=c+b
    #print(a)
    bns=sha1(a.encode()).hexdigest()
    #print(bns)
    if(bns==ans):
        print(i)
        break
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjA3MzE4MTA4MF19
-->