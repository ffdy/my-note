## 格式化字符串漏洞
应用于 printf 家族的不规范使用
```cpp
printf("format",output);//标准格式
printf("%s",str);//这个很规范
printf(str);//虽然这个也可以输出,但会出现问题,即格式化字符串漏洞
```
format 参数告诉程序改用什么格式输出 str
比如 %s 就是把 str 以字符串形式输出
%d 以 int 类型输出

但是当没有 format 参数直接上 output
程序就会把 output 当做 format 参数使用
也就是说,如果 output 当中有如 %s %d 之类的格式化字符串,程序就会进行解析


常用基本的格式化字符串参数介绍(摘自 [https://blog.csdn.net/zz_Caleb/article/details/88980866](http://geekfz.cn/wp-content/themes/begin5.2/inc/go.php?url=https://blog.csdn.net/zz_Caleb/article/details/88980866))：
```
%c：输出字符，配上%n可用于向指定地址写数据。

%d：输出十进制整数，配上%n可用于向指定地址写数据。

%x：输出16进制数据，如%i$x表示要泄漏偏移i处4字节长的16进制数据，%i$lx表示要泄漏偏移i处8字节长的16进制数据，32bit和64bit环境下一样。

%p：输出16进制数据，与%x基本一样，只是附加了前缀0x，在32bit下输出4字节，在64bit下输出8字节，可通过输出字节的长度来判断目标环境是32bit还是64bit。

%s：输出的内容是字符串，即将偏移处指针指向的字符串输出，如%i$s表示输出偏移i处地址所指向的字符串，在32bit和64bit环境下一样，可用于读取GOT表等信息。

%n：将%n之前printf已经打印的字符个数赋值给偏移处指针所指向的地址位置，如%100×10$n表示将0x64写入偏移10处保存的指针所指向的地址（4字节），而%$hn表示写入的地址空间为2字节，%$hhn表示写入的地址空间为1字节，%$lln表示写入的地址空间为8字节，在32bit和64bit环境下一样。有时，直接写4字节会导致程序崩溃或等候时间过长，可以通过%$hn或%$hhn来适时调整。

%n是通过格式化字符串漏洞改变程序流程的关键方式，而其他格式化字符串参数可用于读取信息或配合%n写数据。
```
上面讲的比较抽象 ~~(至少我第一次看的时候没怎么懂,可能我比较菜吧,自闭)~~

举个栗子
```py
addr=0x080468cd
output=p32(addr)+'%10$n'
```
我们假设 printf 函数中的 output 偏移量为 10

如果我们把以上 output 输入
printf 函数就会把 output 前四个字节当做 %n 的写入地址将已经输出的字符个数写入其中,addr 长度为四个字节,所以结果就是 addr 所对的值变成 4

如果要让 addr 所对的值变为 8,只需要在 output 中加入四个字节
比如
```py
output=p32(addr)+'a'*4+'%10$n'
```
这个 $ 就相当于与一个标志,告诉程序偏移量终止
这样就可以绕过一些变量的判断

# x64
其中需要注意的就是 64位传参使用的是寄存器

> 从第一个到第六个依次保存在rdi，rsi，rdx，rcx，r8，r9。从第7个参数开始，接下来的所有参数都将通过栈传递

# scanf
如果`scanf`没加`&`的话，程序会默认从栈中读取`4`个字节的数据当做`scanf`取的地址

# execve
```python
# execve ("/bin/sh") 
# xor ecx, ecx
# mul ecx
# push ecx
# push 0x68732f2f   ;; hs//
# push 0x6e69622f   ;; nib/
# mov ebx, esp
# mov al, 11
# int 0x80

shellcode = ""
```
# GDB
```
常用命令： 
1. checksec 查看elf编译的保护选项。 
2. file [file] 加载objfile 
3. disas addr 对地址addr处的指令进行反汇编，addr可以是函数名 
4. b *addr 在addr处下一个断点 
5. x addr 查看addr处存储的数据值 
6. r 运行被调试的程序 
7. c 继续运行 
8. ni 单步执行不进入 
9. si 单步执行并进入
10.vmmap 得到虚拟映射地址
11. `pattern create size` 生成特定长度字符串
12. `pattern offset value` 定位字符串
```
不可见字符
```
# 千万别用 python -c "print '\x00\x61'" > input，\x00 不会被打印，不会写入文件
# 编写 Py 脚本
s = "\xa9\x06\x40\x00\x00\x00\x00\x00"
with open("input", "wb") as f:
    f.write(s)
# GDB 开始调试
r < input
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTA1OTQ3Mjk4NywxNzg2MjkyMjAwLDE2ND
UzMjMyMCwtMjA5MDEzODc3Ml19
-->