编译的时候需要设置启用调试-g： `g++ -g test.cpp -o test -lcurl`
开始调试 `gdb binName`
如果执行二进制文件需要入参 `binName --config=xxx` 则调试指令为`gdb --args binName --config=xxx`
也可以`gdb`进入后，使用`file binName`进入调试
一般调试流程，打断点，然后`run`，然后n看每一行
打断点方式：
```bash
b 方法名
#b main()
#b PayCenterNotifyImp::doRequest
b 文件+代码行号，文件路径是相对于main函数文件的路径
#b test.cpp:8
#b PayCenterLogic/test.cpp:18
【现在用不到，暂不记录】另外还有条件断点、内存地址断点、观察点（Watchpoints）、捕捉点（Catchpoints）、线程断点
```

退出调式 `quit`

# 一些指令
```bash
GDB是GNU调试器，用于调试C和C++程序。GDB提供的命令有很多，以下是一些常用的GDB调试命令：

1. 启动程序：
- `start`：启动程序，在main函数的第一行代码处暂停。
2. 查看代码：
- `list(l)` ：显示源代码的环境。
- `info(i) break`：查看断点信息。
3. 设置断点：
- `break(b)`：在特定行设置断点。
- `info breakpoints`：查看所有设置的断点。
4. 控制程序执行：
- `run`：开始或继续执行程序。
- `next(n)`：执行下一行代码，跳过函数调用。
- `step(s)`：执行下一行代码，进入函数调用。
- `continue(c)`：继续执行程序直到下一个断点。
- `finish`：执行当前函数直到函数返回。
- `until`：执行直到当前循环结束。
- `return`：指定函数返回值并结束函数执行。
5. 查看变量：
- `print(p)`：打印变量的值。
- `info(i) locals`：查看当前栈帧的所有变量。
6. 修改变量：
- `set var=value`：修改变量的值。
7. 其他命令：
- `backtrace`：查看调用堆栈。
- `frame`：切换堆栈帧。
- `quit(q)`：退出GDB。
- `help`：获取帮助信息。
```

# 在vscode中使用gdb

按F5创建launch.json文件，右下角添加配置，选这gdb启动

program改为二进制文件路径
args为参数
例如
"program": "${workspaceFolder}/HelloServer/HelloServer",
"args": ["--config=${workspaceFolder}/HelloServer/config.conf"],