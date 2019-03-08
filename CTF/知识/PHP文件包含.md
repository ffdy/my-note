php文件包含漏洞，大致套路是这样滴：
```php
<?php 
	$file = $_GET["file"]; 
	... ... 
	include($file); 
?>
```
> 例如：test.php大致内容如上，通过访问http://xxx/test.php?file=xxx.php，则$file=xxx.php，也就是说：include(xxx.php)，而对于php的include()函数，会获取指定文件的内容，在执行前将代码插入到test.php文件中。而如果被包含的文件中无有效的php代码，则会直接输出无效的文件内容。通常利用无效代码这一点来将文件内容输出。

[php://协议](http://php.net/manual/zh/wrappers.php.php)
https://www.freebuf.com/column/148886.html
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTg4Mjg5MzY2NywtNjU4OTE5MTI3LDEyMz
Y4NTI2NDVdfQ==
-->