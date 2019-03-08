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
直接构造 payload"

## 求 Getshell
后缀名黑名单检测和类型检测

如果是walf严格匹配，通过修改Content-type后字母的大小写可以绕过检测，使得需要上传的文件可以到达服务器端，而服务器的容错率较高，一般我们上传的文件可以解析。然后就需要确定我们如何上传文件，这里将文件的后缀名改为.jpg和.png都不可行，在分别将后缀名修改为php2, php3, php4, php5, phps, pht, phtm, phtml（php的别名），发现只有php5没有被过滤，成功上传，得到flag
![](https://upload-images.jianshu.io/upload_images/9172841-0b4859adfbdad510.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/702/format/webp)

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIwNjkxMzIzNTcsLTEyMTkwNTUwMzksLT
E3NDUwOTI2MjgsLTQyODcyNTgwNF19
-->