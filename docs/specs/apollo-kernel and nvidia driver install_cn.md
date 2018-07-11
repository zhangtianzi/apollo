### 硬件
IPC：Nuvo-5095GC
GPU: NVIDIA GTX-1050Ti

### 问题1
apollo官方推荐使用ubuntu14.04,系统安装完之后，屏幕分辨率非常低，需要安装安装N卡驱动。按照apollo-kernel安装文档，应该先安装apollo-kernel，再使用如下命令安装N卡驱动：

```
sudo bash install-nvidia.sh
```
脚本文件请点击 [这里](https://github.com/ApolloAuto/apollo-kernel/blob/master/linux/install-nvidia.sh)

但是，部分用户发现，此方法安装好N卡驱动后，虽然使用nvidia-smi命令可以查看到驱动已经正常安装，但屏幕分辨率依然很低。且 X Server Settings中无法切换到N卡驱动。

### 问题2 
系统安装完毕之后，直接安装N卡驱动，具体方法请点击 [这里](https://blog.csdn.net/CosmosHua/article/details/76644029) 。接下来，安装apollo-kenel，你会发现屏幕分辨率突然变回装N卡驱动之前的样子了。因为，N卡驱动不支持real-time kernel. 
### 解决方法
经过尝试，本人找到一种可行的方法，在上述硬件环境中亲测有效，步骤如下：
- 安装apollo-kernel后(N卡驱动尚未安装)，下载N卡驱动，这里笔者使用的版本和apollo官方文档的一致：
NVIDIA-Linux-x86_64-375.39.run  ，下载该驱动文件，地址：[http://us.download.nvidia.com/XFree86/Linux-x86_64/375.39/](http://us.download.nvidia.com/XFree86/Linux-x86_64/375.39/)

- 关闭X-Window服务，进入命令行界面并登陆：
```
sudo service lightdm stop
```
- 给驱动文件赋予可执行权限：

```
sudo chmod +x NVIDIA-Linux-x86_64-375.39.run
```

- 安装。后面的参数非常重要，不可省略，特别是最后一个！！

```
sudo ./NVIDIA-Linux-x86_64-384.59.run –no-opengl-files –no-x-check -no-kernel-module
```
### 说明
- 遇到此问题的用户不多，github也有相关[issue](https://github.com/ApolloAuto/apollo/issues/1872)
- 笔者对参数 -no-kernel-module 所带来的影响还未测试，后续在使用过程遇到问题会在此更新
- 其他硬件环境尚未测试
- 进一步核实，在上述硬件环境中，[这个文档](https://github.com/ApolloAuto/apollo/blob/master/docs/specs/Software_and_Kernel_Installation_guide.md)中的步骤无法正常安装。笔者已更新此文档。
- 目前可以参照本文档进行安装，也可以参照 [更新后的文档](https://github.com/zhangtianzi/apollo/blob/master/docs/specs/Software_and_Kernel_Installation_guide.md)
