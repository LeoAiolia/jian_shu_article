>1、打开终端
2、输入`$DevToolsSecurity --status`

查看状态，如果是
```
Developer mode is currently disabled.
```
那就是你目前每次使用xcode跑模拟器时都需要输入密码。

>3、终端输入`$DevToolsSecurity --enable` 此时会弹出密码输入框，输入密码后
终端打印信息：
```
Developer mode is now enabled.
```
之后每次运行模拟器就不需要输入密码了。

4、如果要恢复以前每次运行都要输密码）输入：`$DevToolsSecurity --disable` 提示框输入密码，状态就又修改回去了
