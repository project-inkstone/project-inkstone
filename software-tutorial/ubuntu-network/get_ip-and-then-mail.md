> 实验室多了新的服务器（目前单卡最强），但并没有显示器，想通过ssh进行远程连接。但通过拨号连接时，ip地址是动态的，每次重连都会发生改变。
>
> 我开始的想法是将动态ip修改为静态ip，参考了一堆教程，并不行o(╯□╰)o
>
> 百度了下，最终思路是 服务器每分钟检测本地ip地址，若发生了变化，则将改变后的ip地址 通过邮件发送给我。

首次提交：2019-10-11 by [🐲](https://github.com/Jngwl)

#### 安装相关依赖

```bash
sudo apt-get install mutt
sudo apt-get install msmtp
```



#### 配置mutt

创建配置文件`/home/gwl/.muttc`，内容如下

```
set sendmail="/usr/bin/msmtp"
set use_from=yes
set realname="gwl"
set from=462549693@qq.com
set envelope_from=yes
```



#### 配置msmtp

创建配置文件`/home/gwl/.msmtprc`，内容如下

```
account default
host smtp.qq.net
from 462549693@qq.com
auth plain
user 462549693@qq.com
password xxxxxxx
logfile ~/.msmtp.log
```

由于password是明文，安全起见，我们要将该配置文件权限修改为只读

```bash
sudo chmod 600 .msmtprc
```



#### 发送测试邮件

```bash
echo "hello world" | mutt -s "this is a title" 462549693@qq.com
```

  

#### 定时检测ip地址变化

创建脚本文件`/home/gwl/update_ip.sh`，修改文件权限(!!!重要)`sudo chmod 777 /home/gwl/update_ip.sh`，脚本内容如下

```bash
#!/bin/bash
IPADDRESS=$(ifconfig ppp0 | sed -n 's/.*inet addr:\([^ ]*\).*/\1/p')

echo "Last check at: $(date)" >> updateip.log

if [[ "${IPADDRESS}" != $(cat ~/.current_ip) ]];then
if echo "${IPADDRESS}" | mutt -s "server's new ip" 462549693@qq.com ;then
echo "Ip change from $(cat ~/.current_ip) to ${IPADDRESS}" >> updateip.log
echo ${IPADDRESS} > ~/.current_ip
else
echo "Failed to send the mail, try again later." >> ~/updateip.log
fi
fi  
```

修改文件`/tec/crontab`,在最后加上一行，使上述脚本每分钟运行一次。

```bash
* * * * * gwl cd /home/gwl/ && ./update_ip.sh

# * * * * * gwl /home/gwl/update_ip.sh并不行，原因未知
```



- /home/gwl/updateip.log`：记录ip地址检测情况，脚本每运行一次，增加一项。

```bash
gwl@amax:~$ cat updateip.log
Last check at: 2019年 10月 10日 星期四 22:03:24 CST
Ip change from  to 10.25.165.63
Last check at: 2019年 10月 10日 星期四 22:10:55 CST
Ip change from 10.25.165.63 to 10.25.162.53
Last check at: 2019年 10月 10日 星期四 22:15:50 CST
Last check at: 2019年 10月 10日 星期四 22:21:13 CST
Ip change from 10.25.162.53 to 10.25.164.53
Last check at: 2019年 10月 10日 星期四 22:21:33 CST
Last check at: 2019年 10月 10日 星期四 22:21:57 CST
```

- `/home/gwl/sent`：记录邮件发送历史情况。

```bash
gwl@amax:~$ cat sent
From 462549693@qq.com Thu Oct 10 22:03:24 2019
Date: Thu, 10 Oct 2019 22:03:24 +0800
From: gwl <462549693@qq.com>
To: 462549693@qq.com
Subject: new ip
Message-ID: <20191010140324.GA12069@amax>
MIME-Version: 1.0
Content-Type: text/plain; charset=us-ascii
Content-Disposition: inline
User-Agent: Mutt/1.9.4 (2018-02-28)
Status: RO
Content-Length: 13
Lines: 1

10.25.165.63
```

- `/home/gwl/.current_ip`：记录电脑当前的ip

```bash
gwl@amax:~$ cat .current_ip
10.25.150.110
```



#### 参考链接

【1】[ubuntu下使用mutt和msmtp发送邮件的简单配置](https://www.cnblogs.com/xiazh/archive/2011/04/15/2016966.html)

【2】[ssh 如何登录 ip 会变的电脑](https://www.v2ex.com/t/139986)