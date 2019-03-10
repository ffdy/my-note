# Python加解密
```py 
import base64
s=b'asdf' #必须是bytes类型
s=base64.b64encode(s) #base64加密
s=base64.b64decode(s) #base64解密
s=base64. 
s=base64.b32decode(s) #base32解密
```
```py
import hashlib
s=b'asdf'
md5=hashlib.md5() #创建md5方法
md5.update(s) #md5加密
md5.hexdigest() #回显md5值

sha1=hashlib.sha1() #创建sha1方法
sha1.update(s) #sha1加密
sha1.hexdigest() #回显sha1值
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTY2NTIwNjcwNl19
-->