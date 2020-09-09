### Ubuntu

#### cuda安装

1. 准备文件
	- cuda-repo-ubuntu16.04-10.0_local_xxx_amd64.deb [cuda地址](https://developer.nvidia.com/cuda-toolkit-archive)
	- cudnn-10.0-linux-x64-v7.4.1.5 [cudnn地址](https://developer.nvidia.com/rdp/cudnn-archive)

2. 安装cuda
	- 安装cuda
        ```shell
        sudo dpkg -i cuda-xxx.deb
        ```
	     
	- 安装key
		
		```shell
		sudo apt-key add /var/cuda-xxx.pub
		```
	- 更新
	
		```shell
		sudo apt update
		```
	
	- 安装
		```shell
		sudo apt-get install cuda
		```
3. 将cuda添加到环境变量中
	```
	vi .bashrc
	```
	```
	export PATH="/usr/local/cuda/bin:$PATH"
	export LD_LIBRARY_PATH="/usr/local/cuda/lib64:$LD_LIBRARY_PATH"
	```
	```
	source ~/.bashrc
	```
4. 完成[安装完成后，会自动把显卡驱动安装好]

#### cudnn
**cudnn要与cuda版本相匹配** cuda10.0<=>cudnn7.4.1
1. 切换到cudnn.tgz压缩包路径

2. 解压cudnn文件，生成一个cuda文件夹

3. 进入到cuda文件夹

4. 把cudnn文件拷贝到已经安装的cuda/include目录下
	```
	sudo cp include/cudnn.h /usr/local/cuda/include/
	sudo cp lib64/libcudnn* /usr/local/cuda/lib64/
	```
	
5. 设置环境变量，指定cudnn的位置
	```
	export LD_LIBRARY_PATH="/usr/local/cuda/lib64:$LD_LIBRARY_PATH"
	```
	```
	source ~/.bashrc
	```
	
### Windows

#### cuda 安装

1. 准备文件如ubuntu

2. 运行cuda.exe，精简和自定义皆可

3. 环境变量配置

   - 环境变量->系统变量

   - 新建，要新建四个变量，而且要一个一个添加【粗体是变量，灰体是变量的值】

     **CUDA_PATH**
     `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.1`

     **CUDA_PATH_V10_1**
     `C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.1`

     **NVCUDASAMPLES_ROOT**
     `C:\ProgramData\NVIDIA Corporation\CUDA Samples\v10.1`

     **NVCUDASAMPLES10_1_ROOT**
     `C:\ProgramData\NVIDIA Corporation\CUDA Samples\v10.1`

   - 配置好后，确定
   		```cmd
   		nvcc -V
   		nvidia-smi
   	```

#### cudnn


- bin目录 拷贝至C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.1\bin

- include目录 C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.1\include

- lib\x64目录 C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v10.1\lib\x64

