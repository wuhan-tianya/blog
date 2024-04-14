01 python安装虚拟环境


# 一、概述

​	有时候会在一台主机上安装多个不同的Python版本，用以运行不同时期开发的项目， 而在这些不同的Python版本上有时又会加装不同的库和包。因此需要一种工具来管理各个不同的Python版本和运行环境。

​	virtualenv工具可以为每个Python项目创建一个“独立隔离”的虚拟Python运行环境，而且每个项目都可以为自己独立的Python 运行环境加装不同的扩展包和库，而不影响其他项目。

​	在使用virtualenv之前，首先需要保证你的操作系统上已经安装了所需的Python版本。 比如，如果你在操作系统上已经安装了Python2.7和Python3.7，那么就可以利用 virtualenv克隆出多个多个Python2.7和Python3.7独立运行环境。 但是如果你还需要Python3.4环境，那么就必须先在操作系统上安装Python3.4版本。

# 二、Linux下安装Python虚拟环境

```shell
pip3 install virtualenv -i https://pypi.tuna.tsinghua.edu.cn/simple
```

​	一般Linux发行版都会已经安装好一个Python版本，你只需要再安装其它你需要的Python版本就行了。 以ubuntu为例，默认已安装好 Python2.7，你只要再安装一个 Python3.7 即可。 下面是虚拟环境的建立步骤：

## 1. 安装virtualenv

### （1）安装virtualenv

​	利用python3可以直接安装virtualenv（会默认安装到/usr/bin/virtualenv）

### （2）创建虚拟环境

​	使用virtualenv创建虚拟环境

```shell
virtualenv -p python3 venv
```

### （3）激活虚拟环境

```shell
source venv/bin/activate
(venv) ubtuntu@VM-12-15-ubuntu:
```

### （4）退出虚拟环境

```shell
deactivate
ubtuntu@VM-12-15-ubuntu:
```



