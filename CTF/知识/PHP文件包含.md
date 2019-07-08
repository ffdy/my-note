php文件包含漏洞，大致套路是这样滴：
```php
<?php 
	$file = $_GET["file"]; 
	... ... 
	include($file); 
?>
```
> 例如：test.php大致内容如上，通过访问http://xxx/test.php?file=xxx.php，则$file=xxx.php，也就是说：include(xxx.php)，而对于php的include()函数，会获取指定文件的内容，在执行前将代码插入到test.php文件中。而如果被包含的文件中无有效的php代码，则会直接输出无效的文件内容。通常利用无效代码这一点来将文件内容输出。

`file=php://filter/read=convert.base64-encode/resource=index.php`
[php://协议](http://php.net/manual/zh/wrappers.php.php)
https://www.freebuf.com/column/148886.html
https://www.freebuf.com/column/183226.html
https://www.jianshu.com/p/0a8339fcc269
https://chybeta.github.io/2007/06/22/%E6%B5%85%E8%B0%88php%E4%BC%AA%E5%8D%8F%E8%AE%AE%E5%8F%8A%E5%9C%A8CTF%E6%AF%94%E8%B5%9B%E4%B8%AD%E7%9A%84%E5%BA%94%E7%94%A8/
https://blog.csdn.net/qq_33904831/article/details/78814567
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTMyMjc4NzQ0MSwtMjEwMjcyMTcwMSwtOD
gyODkzNjY3LC02NTg5MTkxMjcsMTIzNjg1MjY0NV19
-->