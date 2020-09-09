#### 修改hosts

1. 打开[DNS查询](http://tool.chinaz.com/dns/?type=1&host=github.com&ip=)，切换到DNS选项
2. 查询github.com和github.global.ssl.fastly.net
3. 选择TTL最小的DNS，复制ip地址，可以多复制几个。
4. 找到hosts所在的文件夹
    ```
	Windows 系统位于 C:\Windows\System32\drivers\etc\
    Mac（苹果电脑）系统hosts位于 /etc/
    Linux系统hosts位于 /etc/
    绝大多数Unix系统都是在 /etc/
    ```
5. 最好把hosts文件备份一下，把hosts文件拷贝一份后改个名即可。
6. 把复制好的IP地址添加到hosts文件中，类似于这个样子
	```
	140.82.113.3 github.com
	140.82.113.3 github.global.ssl.fastly.net
	```
7. 让hosts生效
	
	```
	Windows
    开始 -> 运行 -> 输入cmd -> 在CMD窗口输入  ： ipconfig /flushdns
    
    Linux
    终端输入 ：  sudo rcnscd restart
    
    Mac OS X终端输入 ：  sudo killall -HUP mDNSResponder
	 
	 其他：断网，再开网；
	 终极方法： 重启机器；
	```