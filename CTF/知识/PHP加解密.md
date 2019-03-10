# PHP加解密
```php
<?php
	$s='asdf';
	echo base64_encode($s); #base64加密
	echo base64_decode($s); #bsae64解密
	echo md5($s); #md5加密
	echo sha1($s); #sha1加密
?>
```
```php
<?php #urlwangqua加密
	$a=$_GET['a'];
	function fullescape($in){
		$out = '';
		for ($i=0;$i<strlen($in);$i++){
			$hex = dechex(ord($in[$i]));
			if ($hex=='')
			$out = $out.urlencode($in[$i]);
			else
			$out = $out .'%'.((strlen($hex)==1) ? ('0'.strtoupper($hex)):(strtoupper($hex)));
		}
		$out = str_replace('+','%20',$out);
		$out = str_replace('_','%5F',$out);
		$out = str_replace('.','%2E',$out);
		$out = str_replace('-','%2D',$out);
		return $out;
	}
	echo fullescape($a);
?>
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTc0MDk2NzQzM119
-->