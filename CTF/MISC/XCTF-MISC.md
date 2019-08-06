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
<!--stackedit_data:
eyJoaXN0b3J5IjpbNjg5Njc0OTIwLC0xMDYwNTcxOTI0XX0=
-->