# Ubuntu 网络配置

首次提交：2018-02-06[🍉](https://github.com/Watermelon-Chen)

本文针对Ubuntu系统的有线连接上网的网络配置，且特指从一个已经设置好拨号上网的无线路由器的LAN口引出至Ubuntu主机的网口的情况。

首先，检查主机网卡名称（除了lo之外的即是网卡名称，通常为eth0），在终端输入（下同）：
  
```
cd /proc/sys/net/ipv4/conf/ls
```
随后，修改主机的静态网络配置
  
```
sudo nano /etc/network/interfaces
```
在打开的interfaces文件中，修改内容如下：

```
#interfaces（5）file used by ifup(8) and ifdown(8) 
#auto lo
#iface lo inet loopback
auto  eth0
iface eth0 inet static
      address 192.168.1.79
      netmask 255.255.255.0
      gateway 192.168.1.1
      dns-nameservers 8.8.8.8
```
其中，auto表示开机自启动；eth0为网卡名称；静态数据分别为ipv4地址、网络掩码、网关、DNS地址。这些数据可在连接该无线路由器的其他主机上查看，DNS地址若有两个，则再加一行dns-nameservers。修改完之后保存并退出。

添加域名服务器：
 
```
sudo nano /etc/resolv.conf
```
在里面添加

```
nameserver 8.8.8.8
```
这里DNS地址在interfaces中已经配置过，也许可以跳过这一步，但我没试过。

  配置完成后，重启网络服务：
  
```
ifdown eth0
ifup eth0
```
如果报错：interface eth0 not configured，改为

```
sudo ifconfig eth0 down
sudo ifconfig eth0 up
```
到这里，网络配置就算完成了。如果还是不行，可以试试动态获取IP:

```
sudo dhclient
```
或者修改interfaces文件为
```
#interfaces（5）file used by ifup(8) and ifdown(8) 
#auto lo
#iface lo inet loopback
auto  eth0
iface eth0 inet dhcp
```
这时，resolv.conf中肯定要配置DNS了，并且也需要重启网络服务。