php文件包含漏洞，大致套路是这样滴：
```php
<?php 
	$file = $_GET["file"]; 
	... ... 
	include($file); ?>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjEyMDM3MDM3NV19
-->