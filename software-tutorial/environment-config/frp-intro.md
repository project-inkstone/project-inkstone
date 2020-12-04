> 本篇为实验室远程环境的简单配置，足够用于平常的远程学习。阿里服务器等都有提供学生优惠，有能力的建议还是租用公网ip并按本教程配置为佳。

本文转载于本人的博客[🐧](https://qiyuan-z.github.io/)

## 为何要用 

我们并不是每天都会接触到实验室内网环境。当不在学校时，如何访问内网的资源成了一个头疼的问题。本文旨在提出一种内网穿透解决方案，在外网环境下优雅的访问到内网的任何端口。即使身离学校也可以方便的修改内网模型，访问内网计算资源。 

## 特殊需要 

- 一台公网机（有公网ip的vps， 比如阿里云服务器）

- frp 端口转发软件 :[下载地址](https://github.com/fatedier/frp/releases)

## 安装

windows不用说，这里讲Ubuntu的安装

选择相应版本，右键复制链接

![](https://blog-1300912400.cos.ap-shanghai.myqcloud.com/frp/Snipaste_2020-02-16_15-19-58.jpg)

打开终端，输入`wget 复制的链接地址`即可开始下载

![](https://blog-1300912400.cos.ap-shanghai.myqcloud.com/frp/Snipaste_2020-02-16_15-25-08.jpg)

下载完成后，找到安装包，解压即可，解压命令为`tar -zxvf 包名`

**公网机、内网机都需要安装，且frp版本要一致。**

## 配置

### 公网机配置

修改 **frps.ini** 文件，这里使用了最简化的配置：

```bash
# frps.ini
[common]
bind_port = 7000 #这个端口代表内网机连到公网所需端口，默认7000，可自己定义
```

Windows 启动 frps：

```
./frps.exe -c ./frps.ini
```

Ubuntu 启动 frps:

```
./frps -c ./frps.ini
```

Ubuntu 在后台开启运行 frp 服务

```text
nohup ./frps -c frps.ini >/dev/null 2>&1 & 
```

### 内网机器配置

修改 **frpc.ini** 文件，假设 **frps** 所在服务器的公网 IP 为 x.x.x.x；

```
# frpc.ini
[common]
server_addr = x.x.x.x #这里可以是公网机的域名，也可以是ip
server_port = 7000 #刚刚公网机配置的与内网机交换的端口

[ssh]
type = tcp
local_ip = 127.0.0.1
local_port = 22 #本地端口号，本地需要转发的端口 默认22 可自己设定
remote_port = 6000  #远程端口号，将端口映射到公网相应的端口 可自己设定
```
Windows 启动 frpc：

```
./frpc.exe -c ./frpc.ini
```

Ubuntu 启动 frpc：

```
./frpc -c ./frpc.ini
```

此时，你就可以通过你的外网 IP + 远程端口号来实现对内网相关服务的访问了。

## 设置开机启动

```
sudo gedit /etc/systemd/system/frpc.service
```

按如下修改

```
[Unit]
Description=frpc daemon
After=syslog.target  network.target
Wants=network.target

[Service]
Type=simple
ExecStart=/usr/sbin/frp/frpc -c /etc/frp/frpc.ini
Restart= always
RestartSec=1min
ExecStop=/usr/bin/killall frpc


[Install]
WantedBy=multi-user.target
```

使用`sudo systemctl enable frpc.service`启用服务

服务器端（公网机器）同理

## 白嫖简化步骤
考虑到很多人负担不起公网机的价格，这里提供发布免费公网域名的网站
[frp免费公共服务器列表](http://www.frps.top/)

由于域名供应商已经配置好了公网机端，所以我们只需要配置我们需要连接的内网机即可

首先找到可用的服务器，比如

![](https://blog-1300912400.cos.ap-shanghai.myqcloud.com/frp/Snipaste_2020-02-16_15-47-16.jpg)

上面显示它所用的frp版本为0.14.1，所以我们也必须安装**相应的frp版本**在内网机上，否则由于软件版本不匹配，连不上。

按照之前的教程，根据所提供的信息，修改**frpc.ini**，如下：

![](https://blog-1300912400.cos.ap-shanghai.myqcloud.com/frp/TIM%E5%9B%BE%E7%89%8720200216154853.jpg)

这里注意0.17.0版本之前用的是`privilege_token = xxxx`，而之后的版本用的是`token = xxxx`，**远程端口号必须设置在服务商所提供的范围内**，如图中TCP/UDP端口为1000-65535，则再此区间任取端口填入就行。

设置完，你便可以通过相应的SSH软件，连接内网机器，主机名为公网域名，端口号为你设置的远程端口

![](https://blog-1300912400.cos.ap-shanghai.myqcloud.com/frp/TIM%E5%9B%BE%E7%89%8720200216155354.png)

有的服务商还提供了二级或三级域名，如图：

![](https://blog-1300912400.cos.ap-shanghai.myqcloud.com/frp/Snipaste_2020-02-16_15-58-29.jpg)

则 frpc.ini 配置如下格式

```
[common]
#server_addr一定要写域名形式，不要直接写IP地址
server_addr = frp1.chuantou.org
server_port = 7000
privilege_token = www.xxorg.com
# 标注你的代理名字，随便选择一个跟别人不一样即可
user = myname

[xxorg] # 可以自己取
type = http
local_ip = 127.0.0.1
local_port = 80 # 按照图中显示为80
# 自己取一个可用的子域名，你的访问地址将会是http://xxorg.frp1.chuantou.org
subdomain = xxorg

[tcp3389] # 可以自己取
type = tcp
local_ip = 127.0.0.1
local_port = 3389 # 自己设
remote_port = 53389 #自己设，图中范围为50000-60000
```

设置完，你便可以通过相应的SSH软件，连接内网机器

如：主机名为`http://xxorg.frp1.chuantou.org`，端口号为53389