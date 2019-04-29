# 1. win32批处理下，另开一个console执行进程X
使用start [/K|/C]，示例：
```bash
//--------------------------------------------------------------------------------------
//example 1: 新开console，执行dir命令完毕后，不自动关闭console
start cmd /K dir 

//--------------------------------------------------------------------------------------
//example 2: 新开console，执行dir命令完毕后，自动关闭console
start cmd /C dir 

//--------------------------------------------------------------------------------------
//example 3: 完整示例
start /D D:\webrtc-dev\dev_native\WebrtcICE\x64\Release cmd /K WebrtcICE.exe server
start /D D:\webrtc-dev\dev_native\WebrtcICE\x64\Release cmd /K WebrtcICE.exe client
```
注意：start命令是不会拥塞当前控制台的.bat执行的。

# 2. 指定start的启动目录
使用start /D，示例如下：
```bash
start /D c:\
```

# 3. 与linux后台运行(&)等价的start操作
使用start /b，示例如下：
```bash
in win32 console:
start /b iperf.exe > c:\iperf_multicast_server_logfile.txt

in linux console:
/root/iperf1.7 > /root/iperf_multicast_client_logfile.txt &
```
参考文献：http://blog.csdn.net/u012377333/article/details/41824787

# 4. 使用python脚本同时启动多console进程
参考文献：
1. https://stackoverflow.com/questions/6469655/how-can-i-spawn-new-shells-to-run-python-scripts-from-a-base-python-script
2. https://stackoverflow.com/questions/15899798/subprocess-popen-in-different-console
3. https://docs.python.org/2/library/subprocess.html

示例如下：
```python
//example 1:
from subprocess import Popen, CREATE_NEW_CONSOLE
Popen('cmd', creationflags=CREATE_NEW_CONSOLE)

//--------------------------------------------------------------------------------------
//example 2:
from sys import executable
from subprocess import Popen, CREATE_NEW_CONSOLE
Popen([executable, 'script.py'], creationflags=CREATE_NEW_CONSOLE)

//--------------------------------------------------------------------------------------
//example 3:
from subprocess import Popen, CREATE_NEW_CONSOLE
Popen('cmd dir', creationflags=CREATE_NEW_CONSOLE, cwd='c:\\')
Popen(['cmd', '/C', 'dir'], creationflags=CREATE_NEW_CONSOLE, cwd='c:\\')

//--------------------------------------------------------------------------------------
//example 4:
from shlex import split
from subprocess import Popen, CREATE_NEW_CONSOLE
cmd_1 = "cmd /K WebrtcICE.exe server";
cmd_2 = "cmd /K WebrtcICE.exe client";
args_1 = split(cmd_1);
args_2 = split(cmd_2);
Popen(args_1, creationflags=CREATE_NEW_CONSOLE, cwd="D:\\webrtc-dev\\dev_native\\WebrtcICE\\x64\\Release");
Popen(args_2, creationflags=CREATE_NEW_CONSOLE, cwd="D:\\webrtc-dev\\dev_native\\WebrtcICE\\x64\\Release");
```