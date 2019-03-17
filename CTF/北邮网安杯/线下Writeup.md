# 北邮网安杯 Writeup
**from scaryffy(zhangyunfe)**
## RE1
下载之后发现不是 .exe 文件
用 Winhex 打开,发现是 ELF 文件
![AeheFH.png](https://s2.ax1x.com/2019/03/17/AeheFH.png)
用 IDA 载入,提示为 64 位文件
![Aehapn.png](https://s2.ax1x.com/2019/03/17/Aehapn.png)

换成 IDA64 载入,找到主函数 main, 进入查看
发现奇怪的相关字符串
![AehXct.png](https://s2.ax1x.com/2019/03/17/AehXct.png)
继续查看,发现输入正确的输出字符串
![Ae4SHS.png](https://s2.ax1x.com/2019/03/17/Ae4SHS.png)
F5 反编译
得到源码
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTQ0MjQzNDM4N119
-->