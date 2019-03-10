# Python加解密
```py 
import base64
s=b'asdf' //必须是bytes类型
s=base64.b64encode(s) //base64加密
s=base64.b64decode(s) //base64解密 
```
```py
import hashlib
s=b'asdf'
md5=hashlib.md5()
md5.update(s)
md5.hex
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDg5ODIzODQyXX0=
-->