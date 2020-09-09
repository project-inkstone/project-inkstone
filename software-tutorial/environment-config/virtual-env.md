#### virtualenv管理虚拟环境

1. virtualenv安装
	```
	pip install virtualenv
	```
2. 切换到安装虚拟环境的目录位置
3. 创建名为env_name的虚拟环境，指定python版本3.6

    ```
    virtualenv env_name --python=python3.6
    ```
4. 激活该虚拟环境
	```
	source env_name/bin/activate
	```
5. 退出虚拟环境
	```
	env_name/bin/deactivate
	```
6. 删除虚拟环境，只需要把该文件夹删掉即可

#### anaconda管理虚拟环境

1. 创建虚拟环境(env_name文件可以在Anaconda安装目录envs文件下找到)
	```
	conda create -n env_name python=3.6
	```
2. 激活虚拟环境
	```
	Linux:  source activate env_name
	Windows: activate env_name
	```
3. 退出虚拟环境
	```
	Linux下：source deactivate 
   Windows:deactivate env_name
	```
4. 删除虚拟环境
	```
	conda remove -n your_env_name --all
	```