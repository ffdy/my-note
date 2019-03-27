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
010 打开,发现后面粘连了文件
binwalk 分析,发现确实粘连了东西
foremost 还原得到另一张图片
上面有 flag
# 猜
是一张只有半边脸的人像,提示是某人的名字的全拼,直接 Google 搜图
这个人是刘亦菲,得到 flag
# 宽带信息泄露
下载到一个 bin 文件,因为是宽带信息,所以用 RouterPassView 打开
提示中是用户名,查找 username 得到 flag 
# 隐写2
用 010 打开,发现 flag.rar 字样
用 binwalk 分析,发现粘连
foremost 还原得到加密的 flag.rar 和一个提示 jpg 文件
rar 解压发现不是 rar 文件,file 查看发现是 zip 文件
现在有三个方法打

1. linux 工具暴力破解  `fcrackzip -b -l 3-3 -c1 -v flag.zip` 
2. ARCHPR 暴力破解
3. 解出提示中的密码
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
eyJoaXN0b3J5IjpbMTcxNjQyODk2NywtODk2NTM2OTc3LC01OD
Q2OTE1MDIsNDI5Mjg2NDY4LDkwMTk0MDM4NiwtMzc4MDAwMjc3
LDgxMzIwODMwMywtMTU4Nzg3NTIxOSwzMTgyMTA4MywtMTI3Nj
Q5MjQ5MywtMjMwNjE3NjAsMTI3NDM1ODk0NCwxMTg2NDQ4ODEx
XX0=
-->