## web2
查看源码,得 flag:`KEY{Web-2-bugKssNNikls9100}`

## 计算器 
要求提交算式得结果,用 JS 验证
查看源码,发现可能是 `code.js`
 然后进入查看,发现 flag:`flag{CTF-bugku-0032}`

## 

## 求 Getshell
后缀名黑名单检测和类型检测

如果是walf严格匹配，通过修改Content-type后字母的大小写可以绕过检测，使得需要上传的文件可以到达服务器端，而服务器的容错率较高，一般我们上传的文件可以解析。然后就需要确定我们如何上传文件，这里将文件的后缀名改为.jpg和.png都不可行，在分别将后缀名修改为php2, php3, php4, php5, phps, pht, phtm, phtml（php的别名），发现只有php5没有被过滤，成功上传，得到flag
![](https://upload-images.jianshu.io/upload_images/9172841-0b4859adfbdad510.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/702/format/webp)

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzNDgyNDc1ODIsLTE3NDUwOTI2MjgsLT
QyODcyNTgwNF19
-->