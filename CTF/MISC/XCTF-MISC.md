# base64 隐写
```
def get_base64_diff_value(s1, s2):
    base64chars = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789+/'
    res = 0
    for i in xrange(len(s1)):
        if s1[i] != s2[i]:
            return abs(base64chars.index(s1[i]) - base64chars.index(s2[i]))
    return res

def solve_stego():

    with open('stego.txt', 'rb') as f:
        file_lines = f.readlines()

    bin_str = ''
    for line in file_lines:
        steg_line = line.replace('\n', '')
        norm_line = line.replace('\n', '').decode('base64').encode('base64').replace('\n', '')
        diff = get_base64_diff_value(steg_line, norm_line)

        pads_num = steg_line.count('=')
        if diff:
            bin_str += bin(diff)[2:].zfill(pads_num * 2)

        else:
            bin_str += '0' * pads_num * 2

    res_str = ''

    for i in xrange(0, len(bin_str), 8):

        res_str += chr(int(bin_str[i:i+8], 2))
    print res_str

solve_stego()
```
特征: 多个 base64 加密串放一堆

# PDF.js 使用
控制台 `document.documentElement.textContent`
# convert分离gif、montage图片拼接
我们可以先把gif分解开，kali的convert命令可以，也可以用其他的工具：convert glance.gif flag.png
用到一个工具：montage，这个工具用法很多，也很强大

用命令：montage flag*.png -tile x1 -geometry +0+0 flag.png

-tile是拼接时每行和每列的图片数，这里用x1，就是只一行

-geometry是首选每个图和边框尺寸，我们边框为0，图照原始尺寸即可
# PNG CRC 爆破
```python
import binascii
import struct

# IHDR: \x49\x48\x44\x52\x00\x00\x00\x00\x00\x00\x02\xF8\x08\x06\x00\x00\x00
# CRC: \x93\x2F\x8A\x6B

crc32key = 0x932F8A6B

with open("misc4.png", "rb") as pic:
    data = pic.read()
    for i in range(1024):
        info = data[12:16] + struct.pack('>i', i) + data[20:29]
        crc32result = binascii.crc32(info) & 0xffffffff
        if crc32result == crc32key:
            print(hex(i))

# 0x2c5
```
- （固定）八个字节89 50 4E 47 0D 0A 1A 0A为png的文件头
- （固定）四个字节00 00 00 0D（即为十进制的13）代表数据块的长度为13
- （固定）四个字节49 48 44 52（即为ASCII码的IHDR）是文件头数据块的标示（IDCH）
- （可变）13位数据块（IHDR)
    - 前四个字节代表该图片的宽
    - 后四个字节代表该图片的高
    - 后五个字节依次为：
    Bit depth、ColorType、Compression method、Filter method、Interlace method
- （可变）剩余四字节为该png的CRC检验码，由从IDCH到IHDR的十七位字节进行crc计算得到。

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTY0MDE1NDQ3Nyw2ODk2NzQ5MjAsLTEwNj
A1NzE5MjRdfQ==
-->