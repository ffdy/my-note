# 签到题
关注微信公众号：Bugku即可
# 这是一张单纯的图片
用 winhex 打开，发现最后有奇怪的字符
`&#107;&#101;&#121;&#123;&#121;&#111;&#117;&#32;&#97;&#114;&#101;&#32;&#114;&#105;&#103;&#104;&#116;&#125;`
发现是一种编码
在 Chrome 搜索框中解码（回车）得到 flag
# 隐写
png 文件入门题
hex 修改高度得到 flag
# telent
用 wireshark 打开
追踪 TCP 流得到 flag
# 眼见非实(ISCCCTF)
解压得到 Word 文档,打开发现文件损坏
猜测文件中加了东西
解压 Word 文档, 没有发现什么奇怪的文件
打开所有的文件查找 `{` 得到 flag
# 啊哒
解压得到 jpg 文件
用 010 打开,在最后发现 flag.txt
用 binwalk 分析发现 flag.txt
foremost 还原,得到一个 zip 文件
解压发现需要密码
继续分析原图片,发现 hex 值中有一段 16 进制数 `73646E6973635F32303138` 解码得到 `sdnisc_2018` 
即为密码,解压 zip 得到 flag
# 又一张图片,还单纯吗

# 想蹭网现解开密码
`crunch 11 11 -t 1391040%%%% -o  pass.txt`
`aircrack-ng  -a2  wifi.cap  -w  password.txt`
# Linux2
`strings brave`
find KEY
# 细心的大象
查看属性，有奇怪的备注
base64 解码
foremost 1.jpg 得到 rar 
hex 修改高度
<!--stackedit_data:
eyJoaXN0b3J5IjpbOTAxOTQwMzg2LC0zNzgwMDAyNzcsODEzMj
A4MzAzLC0xNTg3ODc1MjE5LDMxODIxMDgzLC0xMjc2NDkyNDkz
LC0yMzA2MTc2MCwxMjc0MzU4OTQ0LDExODY0NDg4MTFdfQ==
-->