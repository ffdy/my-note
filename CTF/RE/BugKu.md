## 入门逆向
拖进 IDA 发现初始化了一推变量，直接全部 R 得到 flag
##  easy_vb
拖进 IDA 看起来有壳，但是，往下看看，发现了 `MCTF{_N3t_Rev_1s_E4ay_}` 大喜，赶紧提交，然后错了。。。
回看题目，提交格式为 `flag{}` ，心想，哎呀**，入门题还要搞点坑让人跳，CTF 套路深，我还是溜了吧。
然后试了各种方法，啥都没发现。然后看了一篇题解，说是要把 `MCTF` 替换成 `flag`。
········
啥也别说了，CTF 套路深，我要回 OI。
提交 flag：`flag{_N3t_Rev_1s_E4ay_}`

## Easy_Re
拖进 IDA，F5 反编译
```cpp
int __cdecl main(int argc, const char **argv, const char **envp)
{
  int v3; // eax
  __int128 v5; // [esp+0h] [ebp-44h]
  __int64 v6; // [esp+10h] [ebp-34h]
  int v7; // [esp+18h] [ebp-2Ch]
  __int16 v8; // [esp+1Ch] [ebp-28h]
  char v9; // [esp+20h] [ebp-24h]

  _mm_storeu_si128((__m128i *)&v5, _mm_loadu_si128((const __m128i *)&xmmword_413E34));
  v7 = 0;
  v6 = qword_413E44;
  v8 = 0;
  printf("欢迎来到DUTCTF呦\n");
  printf("这是一道很可爱很简单的逆向题呦\n");
  printf("输入flag吧:");
  scanf("%s", &v9);
  v3 = strcmp((const char *)&v5, &v9);
  if ( v3 )
    v3 = -(v3 < 0) | 1;
  if ( v3 )
    printf(aFlag_0);
  else
    printf((const char *)&unk_413E90);
  system("pause");
  return 0;
}
```
就是输入一个字符串
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEzOTAzNjQwMjJdfQ==
-->