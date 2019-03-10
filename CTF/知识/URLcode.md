主要对字符串进行URL解码，返回已解码的字符串  
1、ASP中的用法：Server.UrlDecode(“内容”) 例如：

<% response.write Server.UrlDecode("%E5%B7%A5%E5%85%B7%E7%BD%91") %>

2、PHP中的用法：urldecode(“内容”) 例如：

<? echo urldecode("%E5%B7%A5%E5%85%B7%E7%BD%91")?>

3、JSP中的用法：URLDecoder.decode(“内容”) 例如：

<% java.net.URLDecoder.decode("%E5%B7%A5%E5%85%B7%E7%BD%91"); %>

4、javascript中的用法 例如：

decodeURI("%E5%B7%A5%E5%85%B7%E7%BD%91");

5、Python中的用法 例如：

import urllib2 urllib2.unquote("%E5%B7%A5%E5%85%B7%E7%BD%91")
<!--stackedit_data:
eyJoaXN0b3J5IjpbNDQ1MjczNjk1XX0=
-->