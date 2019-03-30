## 签到题
关注微信公众号：Bugku即可
## 这是一张单纯的图片
用 winhex 打开，发现最后有奇怪的字符
`&#107;&#101;&#121;&#123;&#121;&#111;&#117;&#32;&#97;&#114;&#101;&#32;&#114;&#105;&#103;&#104;&#116;&#125;`
发现是一种编码
在 Chrome 搜索框中解码（回车）得到 flag
## 隐写
png 文件入门题
hex 修改高度(第 21 到 24 字节)得到 flag
## telent
用 wireshark 打开
追踪 TCP 流得到 flag
## 眼见非实(ISCCCTF)
解压得到 Word 文档,打开发现文件损坏
猜测文件中加了东西
解压 Word 文档, 没有发现什么奇怪的文件
打开所有的文件查找 `{` 得到 flag
## 啊哒
解压得到 jpg 文件
用 010 打开,在最后发现 flag.txt
用 binwalk 分析发现 flag.txt
foremost 还原,得到一个 zip 文件
解压发现需要密码
继续分析原图片,发现 hex 值中有一段 16 进制数 `73646E6973635F32303138` 解码得到 `sdnisc_2018` 
即为密码,解压 zip 得到 flag
## 又一张图片,还单纯吗
010 打开,发现后面粘连了文件
binwalk 分析,发现确实粘连了东西
foremost 还原得到另一张图片
上面有 flag
## 猜
是一张只有半边脸的人像,提示是某人的名字的全拼,直接 Google 搜图
这个人是刘亦菲,得到 flag
## 宽带信息泄露
下载到一个 bin 文件,因为是宽带信息,所以用 RouterPassView 打开
提示中是用户名,查找 username 得到 flag 
## 隐写2
用 010 打开,发现 flag.rar 字样
用 binwalk 分析,发现粘连
foremost 还原得到加密的 flag.rar 和一个提示 jpg 文件
rar 解压发现不是 rar 文件,file 查看发现是 zip 文件
现在有三个方法打

1. linux 工具暴力破解  `fcrackzip -b -l 3-3 -c1 -v flag.zip` 
2. ARCHPR 暴力破解
3. 想出提示中的密码(考验脑洞的时刻到了)
解出密码是 871
解压,又得到一张图片,还是 010 打开
发现最后有 flag ,base64 解密
要把 @ 改为 a,大概是为了防止有人直接搜索 flag 得答案吧~~(出题人**)~~
## 多种方法解决
下载得到一个 exe 文件
但是无法运行,010 打开,发现是 base64 加密的图片
直接复制到 Chrome 搜索框中回车
得到一个二维码
QR_Research 扫码得 flag
## 闪的好快
得到一个二维码的 gif 文件,随便扫会得到不同的字母
`convert masterGO.gif a.png` 将其每一帧分离
依次扫描得到 flag 
## come_game
下载得到一个 exe 文件
打开是一个游戏
玩了一下,发现应该不可能通过~~(you can you up)~~
退出发现根本没有关闭选项
只能强行结束进程.....
发现生成了三个无后缀的文件
经过分析 
DeathTime 记录死亡次数
temp 不知道是啥,好像一直没变
save1 是存档一,第 3 字节存储关卡数,最后一关是 35 
 在显示 game over 之前截屏~~(考验单身手速的时候到了)~~
 得到 flag
## 白哥的鸽子
010 打开 jpg 文件在最后发现多余字符
分析为栅栏密码,栅栏数为 3
解密得 flag
## linux
解压得到 flag 文件
用 strings 命令分离字符串发现最后有 key
得到 flag
## 隐写3
下载得到 png 文件
图片看起来缺了一半
010 打开,修改高度(第 21 到 24 字节)
得到 flag
## 做个游戏(08067CTF)
得到一个 jar 包,打开是一个游戏
你要控制一个不明物体存活 60 秒
这里有一个 Bug,不知道是不是故意的,你可以把不明物体移动到最右侧
不过必须刚好 60 秒才能得到 flag
用 Java Decompiler 打开,找到相关的源码,发现 flag
中间有一段密文,看到 = 号,推测是 base64 
base64 解密,得到 flag
## 想蹭网现解开密码
提示很明显,并且只有 4 位需要爆破
直接 linux 下爆破
先生成字典
`crunch 11 11 -t 1391040%%%% -o  pass.txt`
上 aircrack-ng 爆破
`aircrack-ng  -a2  wifi.cap  -w  pass.txt`
得到手机号,提交 flag
## Linux2
和上一道 linux 一样,strings 命令即可
又因为提示了 KEY
`strings brave | grep -i key #-i 忽略大小写`
得到 flag
## 账号被盗了
打开链接,点击 getflag 跳转到了 cookieflag.php 回显 you are no admin
由 cookieflag 想到 cookie
查看 cookie,发现 isadmin 字段为 false 
burp 抓包,修改 isadmin 为 true,发包得到一个 123.exe 文件
打开是一个 CF 刷枪软件,应该没法用了
一开始以为是逆向,动静态分析半天,然后看了网上的 Writeup
发现是 Wireshark 抓包,emmmmm
随便填完信息,提交
在 Wireshark 中追踪 TCP 流,发现两个 base64 加密的密文
解密得到网易邮箱的账号密码
登陆,发现 flag 已经被之前的人改了
emmmmmm,我还能说啥呢
翻了别人的 Writeup,得到了 flag
顺便把邮箱里 flag 改回去了
## 细心的大象
解压,7-zip 报告头部错误
查看图片属性，有奇怪的备注
base64 解码得到一个字符串
strings 分离,发现有一个疑似 flag 的字符串
鼓捣半天,不知道是啥
010 打开,发现末尾粘连有东西
binwalk 分析,发现粘连了一个 rar 文件
foremost 1.jpg 得到 rar 
解压需要密码,猜测是刚刚的字符串,果然
又得到一张图片 2.png
打开 eog 报错,CRC 校验不通过
用 win7 自带图片查看器打开,发现与上面的隐写的图片一样
010 修改高度,得到 flag
上面 strings 分离得到的字符串只是个幌子,emmmm
7-zip 报错应该是因为图片 CRC 值有问题 ~~(我猜的)~~
## 爆照(08067CTF)
得到一个 8.jpg 文件
查看属性,没有发现
010 打开,后面粘连了东西
foremost 还原得到一个 zip 文件
解压得到多个图片文件,其中 88 上明显有一个 bilibili
又因为提示 flag 分三段
说明剩下的图片中还有两个字符串,继续分析
foremost 分离 8888 得到一个 zip 文件
解压得到一个二维码,扫描得 panama
888 查看属性,在备注上发现 base64 密文
解密得 silisili
三个按源文件名字长度排序按题目要求连起来,得到 flag
## 猫片(安恒)
下载得到 png 文件
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTY2ODU3MzEzOCwtMTA0NDg4MTczNiwtMT
Q3Njk5NDU3MiwtMjAwOTYxNTY1Nyw5OTA4MTU3MTAsLTEzMTM5
Nzk0MTQsOTc2MzYwMiw4MzI5OTI0NzMsLTMxMjc4NjgzMCwtMT
U3NTEyNjAwNSwxMTgxMDc4MjA4LDIwNDQyMzY4NjIsLTE5ODI0
NDU4MDMsMzg5NDI4MTUwLDIwNTQ1OTkyMTUsODUzMzYyOTUwLD
E0NjYyMzEwNTIsMTk1NzQ3NDk2NSwtMTY1NDgyMDEwNywtODQ3
MjQ5OTIwXX0=
-->