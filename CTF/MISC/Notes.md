## outguess
是一个图片隐写软件，可以在github上下载：[https://github.com/crorvick/outguess](https://github.com/crorvick/outguess)，

根据说明编译使用

google找到的使用方法

**加密：**

**_outguess -k "my secret key" -d hidden.txt demo.jpg out.jpg_**

加密之后，demo.jpg会覆盖out.jpg,

hidden.txt中的内容是要隐藏的东西

  

**解密：**

**_outguess -k "my secret key" -r out.jpg hidden.txt_**

解密之后，解密内容放在hidden.txt中
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTMwNTA1NDA4Nl19
-->