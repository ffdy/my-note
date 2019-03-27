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
eyJoaXN0b3J5IjpbMjYyNjU1NjQ3LDgxMzIwODMwMywtMTU4Nz
g3NTIxOSwzMTgyMTA4MywtMTI3NjQ5MjQ5MywtMjMwNjE3NjAs
MTI3NDM1ODk0NCwxMTg2NDQ4ODExXX0=
-->