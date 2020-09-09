一. 临时使用：

可以在使用pip的时候在后面加上-i参数，指定pip源

```bash
pip install xxx -i https://pypi.tuna.tsinghua.edu.cn/simple
```

二. 永久修改：

linux:

修改 ~/.pip/pip.conf (没有就创建一个)， 内容如下：

```bash
[global]
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
```

windows:

直接在user目录中创建一个pip目录，如：C:\Users\xx\pip，在pip 目录下新建文件pip.ini，内容如下

```text
[global]
timeout = 6000
index-url = https://pypi.tuna.tsinghua.edu.cn/simple
trusted-host = pypi.tuna.tsinghua.edu.cn
```