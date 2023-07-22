- [AI开发软件环境综述](#ai开发软件环境综述)
  - [ubuntu操作系统的安装](#ubuntu操作系统的安装)
  - [抛弃bash, 拥抱zsh](#抛弃bash-拥抱zsh)
  - [软件包的使用](#软件包的使用)
  - [安装NVIDIAI GPU驱动](#安装nvidiai-gpu驱动)
    - [Windows:](#windows)
    - [Linux:](#linux)
  - [软件安装 : ssh](#软件安装--ssh)
  - [软件安装：Git](#软件安装git)
  - [效率软件](#效率软件)
  - [常用ubuntu命令](#常用ubuntu命令)
  - [Anaconda](#anaconda)
  - [Jupyter Lab](#jupyter-lab)
    - [本地使用](#本地使用)
    - [服务器使用](#服务器使用)
  - [Vscode配置](#vscode配置)


# AI开发软件环境综述

关于软件方面：

1.ubuntu操作系统的安装

2.抛弃bash,拥抱zsh

3.软件包管理器的使用，

4.安装NVIDIA GPU驱动

5.软件安装:Anaconda

6.软件安装:Python IDE

7.软件安装: ssh

8.软件安装:Git

9.其他效率软件安装

## ubuntu操作系统的安装

不同ubuntu版本的ISO File:  https://cn.ubuntu.com/download
注意: windows虚拟机中的显卡是物理CPU模拟出来的，没有调用物理GPU，所以虚拟机装ubuntu是无法进行深度学习训练。

Learn Unix:[https://www.tutorialspoint.com/unix](https://link.zhihu.com/?target=https%3A//www.tutorialspoint.com/unix/unix-what-is-shell.htm)

## 抛弃bash, 拥抱zsh

shell是指一种应用程序，这个应用程序提供了一个界面，用户通过这个界面访问操作系统内核的服务。shell脚本（shell script），是一种为shell编写的脚本程序。

当前主流的操作系统都支持shell编程:

**Windows PowerShell**的诞生就是要提供功能**相当于**UNIX系统的命令行壳程序（例如：sh、bash或csh），同时也内置脚本语言以及辅助脚本程序的工具。为了同时能用grep, awk, curl等工具，最好装一个cygwin或者mingw来模拟linux环境。

● Powershell: [https://docs.microsoft.com/zh-cn/](https://link.zhihu.com/?target=https%3A//docs.microsoft.com/zh-cn/)

● Cygwin:[http://www.cygwin.com/](https://link.zhihu.com/?target=http%3A//www.cygwin.com/)

● mingw:[http://www.mingw.org/](https://link.zhihu.com/?target=http%3A//www.mingw.org/)

`Mac OS`不仅带了`sh、bash`这两个最基础的解释器，还内置了`ksh`、`csh`、`zsh`等不常用的解释器`.macOS 10.15 Catalina`默认`shell`是`zsh`。

Linux默认安装就带了shell解释器，这里推荐用 `ohmyzsh`。

ohmyzsh:[https://github.com/ohmyzsh/ohmyzsh](https://link.zhihu.com/?target=https%3A//github.com/ohmyzsh/ohmyzsh) 113k Str & Efficiency & Automatic Suggestion and completion & color display....

## 软件包的使用

`Windows`常用`.exe`和`.msi`为代表的可执行文件来安装应用程序。这种方式需要手动交互介入，不方便。

微软推出了一款内置的软件包管理器 `winget`。用户无需在窗口中频繁点击，即可完成安装工作。

+ 软件包：`.exe`、`.msi`

+ 软件包管理器：`winget`[https://github.com/microsoft/winget-cli](https://link.zhihu.com/?target=https%3A//github.com/microsoft/winget-cli)

`Mac. `  常用homebrew, 必备神器

+ 软件包：.dmg

+ 软件包管理器：brew：[https://brew.sh/index_zh-cn](https://link.zhihu.com/?target=https%3A//brew.sh/index_zh-cn)

+ 清华源：[https://mirrors.tuna.tsinghua.edu.cn/help/homebrew/](https://link.zhihu.com/?target=https%3A//mirrors.tuna.tsinghua.edu.cn/help/homebrew/)

`Linux.apt`是一个linux高级工具，用于`debian`系软件包管理，主要用于自动从互联网的软件仓库中搜索、安装、升级、卸载软件或操作系统的高级工具（debian系的低级工具是dpkg）

常见安装方式：

1. 源码安装：./configure => make => make install

2. 二进制包安装：软件官方或第三方编译打包好的，解压就能用

3. 软件包管理器安装：.deb 、.rpm 、 .tar.gz
   
   常用软件包管理器：yum、zypper、dpkg、apt....
   
   Ubuntu Package Management:[https://ubuntu.com/server/docs/package-management](https://link.zhihu.com/?target=https%3A//ubuntu.com/server/docs/package-management)
   
   apt(Advanced Package Tool):debian系软件包管理.

软件仓库：aruman(Arch Linux)、yum(CentOS7)、apt(Ubuntu)

## 安装NVIDIAI GPU驱动

### Windows:

GF英伟达推出的一款显卡工具，GF连接到NVIDIA的云数据中心，根据的PC的CPU、GPU和显示器配置来下载最佳的游戏设置

手动查找NVIDIA:[https://www.nvidia.cn/Download/index.aspx?lang=cn](https://link.zhihu.com/?target=https%3A//www.nvidia.cn/Download/index.aspx%3Flang%3Dcn)

自动查找：GeForce Experience

### Linux:

1、手动查找NVIDIA:[https://www.nvidia.cn/Download/index.aspx?lang=cn](https://link.zhihu.com/?target=https%3A//www.nvidia.cn/Download/index.aspx%3Flang%3Dcn)

2、自动查找：NVIDIA:[https://www.nvidia.cn/Download/index.aspx?lang=cn](https://link.zhihu.com/?target=https%3A//www.nvidia.cn/Download/index.aspx%3Flang%3Dcn)

```text
Method1: PPA -- 这种方式并不能保证驱动的安全性与最新版
Method2: CUDA Toolkit => 官方文档 CUDA Installation Guide Linux  
    S1: Install repository meta-data
    S2: Installing the CUDA public GPG key
    S3: Update the Apt repository cache
    S4: Install Driver 
        apt-cache search nvidia-driver
        apt-get install nvidia-driver-435
    S5: reboot => 验证 nvidia-smi
    S6: Install CUDA => 验证
        apt-get install cuda 
        不要一直Yes. 有个地方需要NO, 选不安装驱动
    S7: 修改zsh
        export PATH=/usr/local/cuda-9.2/bin${PATH:+:${PATH}}
        export LD_LIBRARY_PATH=/usr/local/cuda-9.2/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
        export CUDA_HOME=/usr/local/cuda-9.2
    S8: 如何卸载安装
        apt-get --purge  autoremove nvidia*
        apt list --installed|grep cuda
        apt purge cuda-repo-ubuntu1804
        /usr/local/cuda-9.2/bin/cuda-uninstaller
        rm –f /usr/local/cuda-9.2
Method3: ubuntu-drivers --不推荐
```

## 软件安装 : ssh

```bash
sudo apt-get install openssh-server
ssh user@remote -p port
# 复制文件本地到远程服务器
scp -P port /path/to/local/file user@remote:/path/to/remote/file 
```

## 软件安装：Git

之前写过很多有关于git的文章了，这里不多赘述。

游戏：Learn Git Branching: [https://learngitbranching.js.org/?locale=zh_CN](https://link.zhihu.com/?target=https%3A//learngitbranching.js.org/%3Flocale%3Dzh_CN)

官方文档：[https://git-scm.com/doc](https://link.zhihu.com/?target=https%3A//git-scm.com/doc)

## 效率软件

XMind, Dash...

## 常用ubuntu命令

GPU：

```bash
# watch 持续看 -n1间隔1s刷新一次
watch -n1 nvidia-smi
```

CPU：

```bash
htop
```

## Anaconda

Anaconda下载地址: https://www.anaconda.com/products/distribution 

建议安装到SSD固态上加快读写速度。

这里针对远程服务器的安装：

由于公司/学校Al集群一般是没有图形化界面。需要右键”64-Bit(x86) installer"复制anaconda的下载链接地址。

step1:根据IP/user_id/port链接服务器

```bash
ssh frank@18.19.124.245 -p 2022
```

step2:转到任意一个目录进行wget

```bash
cd Downloads
wget -c https: //cepo.anaconda.com/archive/Anaconda3-2021.11-Linux-x86_64.sh
```

step3:安装，采用默认Enter yes yes即可

```bash
sh Anaconda3-2021.11-Linux-x86_64.sh
```

具体的创建环境这里略过，不过值得注意的是，对于MacOS由于没有NVDIA GPU也就只能安装CPU版本的pytorch。

**TensorFlow，Pytorch的安装**

TensorFlow: https://www.tensorflow.org/installsource?hl=zh-cn#gpu
TensorFlow不同版本有严格的CUDA对应要求,如果系统装了CUDA10.1去安装tensorflow-2.40会报错，要注意版本的对应。

Pytorch: https://pytorch.org/get-started/previous-versions/
相比于TensorFlow的静态图机制,基于动态图的Pytorch不同版本就不需要严格对应系统CUDA，在上述链接中可以找到不同版本的下载信息。

**Python软件库的选择**

anaconda: https://anaconda.org/anaconda/repo  里面有很多包，点开有具体安装的命令。

Pypi org: https://pypi.org 同上，建议使用pip

**替换清华源**
地址: https://mirrors.tuna.tsinghua.edu.cn

step1:激活自己的Anaconda Environment

step2:对于windows系统需要生成.condarc文件，并全局搜索找到.condarc文件

```bash
conda config -set show_channel_ruls yes
```

step3:对于MacOS/ubuntu系统在homelxxx（xxx为自己用户名）找到.condarc文件，替换清华源地址

```bash
vim / home/wlsh/ .condarc
```

## Jupyter Lab

### 本地使用

对于windows/macOS/ubuntu有图形界面的系统

+ 可以通过菜单栏点开Aanaconda的图标，得到上面的界面，然后点开Jupyter Lab

+ 可以在终端直接输入jupyter lab 即可直接打开

### 服务器使用

step1:  通过IP/use_id/port连接集群

```bash
ssh frank@10.19.124.245 -p 2022
```

step2: 激活自己的Anaconda Environment

```bash
conda activate Env_name
```

step3: 安装jupyterlab并添加Environment到Jupyterlab

```bash
pip install jupyterlab
python -m ipykernel install --user --name Env_name
```

step4: 生成 jupyter lab 配置文件

```bash
jupyter lab --generate-config
```

终端显示生成的配置文件位于

```bash
/home/frank/.jupyter/jupyter_lab_config.py
```

step5: 修改登录密码

```bash
Jupyter lab password
```

step6: 主机IP/端口设置，编辑文件在最后面添加三行。

```bash
vim /home/frank/.jupyter/jupyter_lab_config.py
c.NotebookApp.ip='*'# 允许任何IP链接
c.NotebookApp.open_browser=False #不打开图形界面
c.NotebookApp.port=8888 # 初始端口，如果冲突它会自行替换
```

这三行的意思分别是允许任何IP链接.集群不打开jupyter lab的图形界面,初始服务器端口IP如果遇到冲突会自行替换。

step7: 启动jupyter lab并挂载到服务器的后台并查看其结果输出，查看后可以找到IP。然后把localhost替换为服务器IP，在自己笔记本浏览器打开即可。

```bash
nohup jupyter lab --port=8891 & cat nohup.out
```

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/03/14-16-47-12-1678783631.png)

step8: 如果想关闭，只需找到进行关闭即可。

```bash
ps -ef | grep jupyter lab
Kill -9 XXXX
```



## Vscode配置

推荐插件：

+ 简体中文=>汉化界面

+ Python =>管理切换python环境

+ Remote-SSH =>服务器链接

+ Remote-SSH: Editing Configuration Files

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/03/14-17-39-19-1678786757.png)

远程配置

配置SSH - 官方文档：Remote Development Tips and Tricks

```bash
ssh-keygen -t rsa -b 4096 #生成mac/ubuntu的local public key
ssh-copy-id -p 2022 frank@10. 19.124.245#复制此key到远程主机
```

以上是针对py文件的配置，而jupyter notebook 的配置与次有一点差别，但不是很大。自行搜索解决即可。依旧是服务器开启一个端口挂载后台，然后点击Jupyter本地--> 现有url-->把localhost替换为服务器IP输入即可。

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/03/14-17-40-13-1678786799.png)

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/03/14-17-40-31-1678786830.png)

![](https://raw.githubusercontent.com/timerring/scratchpad2023/main/2023/03/14-17-40-52-1678786847.png)

此外，还可以配置markdown以及latex环境。注意：latex还需要安装编译器，windows安装TexLive，Mac安装macTex即可。




