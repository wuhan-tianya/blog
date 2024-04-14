Docker


# 1.Docker 简介和安装

### Docker 是什么

Docker 是一个应用打包、分发、部署的工具
你也可以把它理解为一个轻量的虚拟机，它只虚拟你软件需要的运行环境，多余的一点都不要，
而普通虚拟机则是一个完整而庞大的系统，包含各种不管你要不要的软件。

### 跟普通虚拟机的对比

| 特性   | 普通虚拟机                                                   | Docker                                               |
| ------ | ------------------------------------------------------------ | ---------------------------------------------------- |
| 跨平台 | 通常只能在桌面级系统运行，例如 Windows/Mac，无法在不带图形界面的服务器上运行 | 支持的系统非常多，各类 windows 和 Linux 都支持       |
| 性能   | 性能损耗大，内存占用高，因为是把整个完整系统都虚拟出来了     | 性能好，只虚拟软件所需运行环境，最大化减少没用的配置 |
| 自动化 | 需要手动安装所有东西                                         | 一个命令就可以自动部署好所需环境                     |
| 稳定性 | 稳定性不高，不同系统差异大                                   | 稳定性好，不同系统都一样部署方式                     |

### 打包、分发、部署

**打包**：就是把你软件运行所需的依赖、第三方库、软件打包到一起，变成一个安装包
**分发**：你可以把你打包好的“安装包”上传到一个镜像仓库，其他人可以非常方便的获取和安装
**部署**：拿着“安装包”就可以一个命令运行起来你的应用，自动模拟出一摸一样的运行环境，不管是在 Windows/Mac/Linux。	

### Docker 部署的优势

常规应用开发部署方式：自己在 Windows 上开发、测试 --> 到 Linux 服务器配置运行环境部署。

> 优点：确保了不同机器上跑都是一致的运行环境，不会出现我机器上跑正常，你机器跑就有问题的情况。

### Docker 通常用来做什么

- 应用分发、部署，方便传播给他人安装。特别是开源软件和提供私有部署的应用
- 快速安装测试/学习软件，用完就丢（类似小程序），不把时间浪费在安装软件上。例如 Redis / MongoDB / ElasticSearch / ELK
- 多个版本软件共存，不污染系统，例如 Python2、Python3，Redis4.0，Redis5.0
- Windows 上体验/学习各种 Linux 系统

### 重要概念：镜像、容器

**镜像**：可以理解为软件安装包，可以方便的进行传播和安装。
**容器**：软件安装后的状态，每个软件运行环境都是独立的、隔离的，称之为容器。

# 2.docker安装

## docker的基本组成

![img](https://gimg2.baidu.com/image_search/src=http%3A%2F%2Fimg-blog.csdnimg.cn%2Fimg_convert%2F287b359dc6f80f03900e627a650521a1.png&refer=http%3A%2F%2Fimg-blog.csdnimg.cn&app=2002&size=f9999,10000&q=a80&n=0&g=0n&fmt=auto?sec=1666275694&t=dcbbc9ea76480c35c685643cbb498195)

**镜像（image）：**

docker镜像就好比是一个魔板，可以通过这个模板来创建容器服务，通过这个镜像可以创建多个容器（最终服务运行或者项目运行就是在容器中）

**容器（container）：**

docker利用容器技术，独立运行一个或者一个应用，通过镜像来创建的

启动，停止，删除，基本删除

目前可以把这个容器理解为就是一个简易的linux系统

**仓库（repository）：**

仓库就是存放镜像的地方，仓库分为公有仓库和私有仓库

docker hub（默认是国外的）

阿里云。。。都有容器服务器（配置镜像加速）

## 安装docker

> 环境准备

1. 需要会一点点的linux的基础
2. ubuntu
3. 我们使用Xshell连接远程服务器进行操作

> 环境查看

```shell
ubuntu@VM-12-15-ubuntu:~$ uname -a
Linux VM-12-15-ubuntu 5.4.0-96-generic #109-Ubuntu SMP Wed Jan 12 16:49:16 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux
ubuntu@VM-12-15-ubuntu:~$ 
```

```shell
ubuntu@VM-12-15-ubuntu:~$ cat /etc/os-release 
NAME="Ubuntu"
VERSION="20.04 LTS (Focal Fossa)"
ID=ubuntu
ID_LIKE=debian
PRETTY_NAME="Ubuntu 20.04 LTS"
VERSION_ID="20.04"
HOME_URL="https://www.ubuntu.com/"
SUPPORT_URL="https://help.ubuntu.com/"
BUG_REPORT_URL="https://bugs.launchpad.net/ubuntu/"
PRIVACY_POLICY_URL="https://www.ubuntu.com/legal/terms-and-policies/privacy-policy"
VERSION_CODENAME=focal
UBUNTU_CODENAME=focal
```

> 安装

帮助文档：

```shell
# 1.卸载旧版本的docker
sudo apt-get remove docker docker-engine docker.io containerd runc
# 2.设置存储库
sudo apt-get update
sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
# 3.添加 Docker 的官方 GPG 密钥
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
# 3.设置存储库
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
# 5.安装docker
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin
# 6.启动docker
systemctl start docker
# 7.使用docker version
ubuntu@VM-12-15-ubuntu:~$ docker version
Client:
 Version:           20.10.12
 API version:       1.41
 Go version:        go1.16.2
 Git commit:        20.10.12-0ubuntu2~20.04.1
 Built:             Wed Apr  6 02:14:38 2022
 OS/Arch:           linux/amd64
 Context:           default
 Experimental:      true

Server:
 Engine:
  Version:          20.10.12
  API version:      1.41 (minimum version 1.12)
  Go version:       go1.16.2
  Git commit:       20.10.12-0ubuntu2~20.04.1
  Built:            Thu Feb 10 15:03:35 2022
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.5.9-0ubuntu1~20.04.4
  GitCommit:        
 runc:
  Version:          1.1.0-0ubuntu1~20.04.1
  GitCommit:        
 docker-init:
  Version:          0.19.0
  GitCommit: 
  
# 8.hello-world
docker run hello-world
ubuntu@VM-12-15-ubuntu:~$ docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
2db29710123e: Pull complete 
Digest: sha256:62af9efd515a25f84961b70f973a798d2eca956b1b2b026d0a4a63a3b0b6a3f2
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
 
 # 9.查看hello-world镜像
 docker images
```

## 阿里云进行加速

1. 安装/升级docker客户端

推荐安装1.10.0以上版本的docker客户端

2. 配置镜像加速器

针对docker客户端大于1.10.0的用户

您可以通过修改daemon配置文件/etc/docker/daemon.json来使用加速期

```shell
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json << 'EOF'
{
  "registy-mirrors": ["http://qiyb9988.mirror.aliyuncs.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

## Docker run的运行流程

![image-20221024235229282](../../../blog/source/picture/image-20221024235229282.png)

## 底层原理

### docker是怎么工作的

docker是一个client-server结构的系统，docker的守护进程运行在主机上，通过socket从客户端访问

docker-server接收到docker-client指令，就会执行这个

![image-20220924133740588](../../../blog/source/picture/image-20220924133740588.png)

### docker为什么比虚拟机快

1. docker有着比虚拟机更少的抽象层
1. docker利用宿主机的内核，VM需要是Guest Os

![image-20220924134112529](../../../blog/source/picture/image-20220924134112529.png)

所以新建一个人容器的时候，docker不需要像虚拟机一样重新加载一个操作系统的内核，避免引导，虚拟机是加载Guest Os，分钟级别的，而docker是利用宿主机的操作系统，省略了这个复杂的过程，秒级的

# 3.docker常用命令

## 帮助信息

```shell
docker version     # 显示docker的版本信息
docker info        # 显示docker的系统信息，包括镜像和容器的数量
docker cmd --help  # 帮助命令
```

## 镜像命令

**docker images**

```shell
ubuntu@VM-12-15-ubuntu:~$ docker images
REPOSITORY    TAG          IMAGE ID       CREATED         SIZE


# 解释
REPOSITORY   镜像的仓库源
TAG					 镜像的标签
IMAGE ID     镜像的id
CREATED      镜像的创建时间
SIZE         镜像的大小

可选项
  -a, --all             列出所有的镜像
  -q, --quiet           只显示镜像的id
```

**docker search 搜索镜像**

```shell
ubuntu@VM-12-15-ubuntu:~$ docker search mysql
NAME                            DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql                           MySQL is a widely used, open-source relation…   13216     [OK]       
mariadb                         MariaDB Server is a high performing open sou…   5054      [OK]       
phpmyadmin                      phpMyAdmin - A web interface for MySQL and M…   631       [OK]       
percona                         Percona Server is a fork of the MySQL relati…   587       [OK]

# 可选项，通过搜索来过滤
--filter=STARS=3000  # 搜索出来的镜像就是START大于3000

ubuntu@VM-12-15-ubuntu:~$ docker search mysql --filter=STARS=3000
NAME      DESCRIPTION                                     STARS     OFFICIAL   AUTOMATED
mysql     MySQL is a widely used, open-source relation…   13216     [OK]       
mariadb   MariaDB Server is a high performing open sou…   5054      [OK]  

```

**docker pull 下载镜像**

```shell
# 下载镜像 docker pull镜像名[:tag]
ubuntu@VM-12-15-ubuntu:~$ docker pull mysql
Using default tag: latest  # 如果不写tag，默认就是latest
latest: Pulling from library/mysql 
051f419db9dd: Pull complete  # 分层下载，docker image的核心 联合文件系统
7627573fa82a: Pull complete 
a44b358d7796: Pull complete 
95753aff4b95: Pull complete 
a1fa3bee53f4: Pull complete 
f5227e0d612c: Pull complete 
b4b4368b1983: Pull complete 
f26212810c32: Pull complete 
d803d4215f95: Pull complete 
d5358a7f7d07: Pull complete 
435e8908cd69: Pull complete 
Digest: sha256:b9532b1edea72b6cee12d9f5a78547bd3812ea5db842566e17f8b33291ed2921  # 签名
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest  # 真实地址

# 等价于它
docker pull mysql
docker pull docker.io/library/mysql:latest

# 指定版本下载
```

**docker rmi 删除镜像**

```shell
ubuntu@VM-12-15-ubuntu:~$ docker rmi -f python  # 删除指定容器
ubuntu@VM-12-15-ubuntu:~$ docker rmi -f python3 python2  # 删除多个容器
ubuntu@VM-12-15-ubuntu:~$ docker rmi -f $(docker images)  # 删除全部的容器
```

## 容器命令

**说明：我们有了镜像才可以创建容器，linux，下载一个centos镜像来测试学习**

```shell
docker pull centos
```

**新建容器并启动**

```shell
docker run [可选参数] image

# 参数说明
--name="Name"   容器名字 tomcat01 tomcat02，用来区分容器
-d              后台方式运行
-it							使用交互方式运行，进入容器查看内容
-p							指定容器的端口 -p  8080:8080
		-p ip：主机端口：容器端口
		-p 主机端口：容器端口（常用）
		-p 容器端口
		容器端口
-P							随机指定端口

# 测试，启动并进入容器
ubuntu@VM-12-15-ubuntu:~$ docker run -it centos /bin/bash 
[root@ec81b04d3561 /]# ls  # 查看容器内的centos，基础版本，很多命令都是不完善的
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

# 从容器中的退回主机
[root@ec81b04d3561 /]# exit
exit
ubuntu@VM-12-15-ubuntu:~$ 
```

**列出所有运行的容器**

```shell
# docker ps 命令
	 # 列出当前正在运行的容器
-a # 列出当前正在运行的容器+带出历史运行过的容器
-n=？ # 显示最近创建的容器
-q	# 只显示容器的编号

ubuntu@VM-12-15-ubuntu:~$ docker ps -a
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS                          PORTS                                                  NAMES
ec81b04d3561   centos         "/bin/bash"              2 minutes ago   Exited (0) About a minute ago                                                          priceless_gould
5ab88b501192   7b94cda7ffc7   "docker-entrypoint.s…"   3 weeks ago     Up 3 weeks                      33060/tcp, 0.0.0.0:3300->3306/tcp, :::3300->3306/tcp   per-admin-flask
ubuntu@VM-12-15-ubuntu:~$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED       STATUS       PORTS                                                  NAMES
5ab88b501192   7b94cda7ffc7   "docker-entrypoint.s…"   3 weeks ago   Up 3 weeks   33060/tcp, 0.0.0.0:3300->3306/tcp, :::3300->3306/tcp   per-admin-flask
ubuntu@VM-12-15-ubuntu:~$ 
```

**退出容器**

```shell
exit  # 直接容器停止并退出
Ctrl + P + Q # 容器不停止退出
```

**删除容器**

```shell
docker rm 容器id								# 删除指定的容器，不能删除正在运行的容器，如果强制删除， rm -f
docker rm -f $(docker ps -aq)  # 删除所有的容器
docker ps -a -q ｜ xargs docker rm  # 删除所有的容器
```

**启动和停止容器**

```shell
docker start 容器id    	# 启动容器
docker restart 容器id   # 重启容器
docker stop 容器id			# 停止正在运行的容器 
docker kill 容器id			# 强制停止当前容器
```

## 常用其他命令

**后台启动容器**

```shell
# 命令 docker run -d 容器名
ubuntu@VM-12-15-ubuntu:~$ docker run -d centos
30c9d966f12d04590c683cb9aaf4f2792143e341457882b7a28fbaa04b8e4287

# 问题docker ps，发现centos停止了

# 常见的坑，docker容器使用后台运行，就必须要有一个前台进程，docker发现没有应用，就会自动停止
# nginx，容器启动后，发现自己没有提供服务，就会立刻停止，就是没有程序了
```

**查看日志**

```shell
docker logs -f -t --tail 容器,没有日志

# 自己编写一段shell脚本
ubuntu@VM-12-15-ubuntu:~$ docker run -d centos /bin/bash -c "while true;do echo xdj;sleep 1;done"

# 显示日志
-tf
--tail number  # 显示的日志条数
ubuntu@VM-12-15-ubuntu:~$ docker logs -tf --tail 10 a436289744f8
```

**查看容器中的进程信息**

```shell
# 命令 docker top 容器id
ubuntu@VM-12-15-ubuntu:~$ docker top a436289744f8
UID                 PID                 PPID                C                   STIME               TTY                 TIME                CMD
root                2687289             2687268             0                   16:26               ?                  
root                2687995             2687289             0                   16:29               ?                  
```

**查看镜像中的元数据**

```shell
[
    {
        "Id": "a436289744f8b6eff4aa9d9a257d7337e59c3dc7bb95e0ac4a16088b96cc9d6e",
        "Created": "2022-09-24T08:26:05.604732532Z",
        "Path": "/bin/bash",
        "Args": [
            "-c",
            "while true;do echo xdj;sleep 1;done"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 2687289,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2022-09-24T08:26:05.926463582Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:5d0da3dc976460b72c77d94c8a1ad043720b0416bfc16c52c45d4847e53fadb6",
        "ResolvConfPath": "/var/lib/docker/containers/a436289744f8b6eff4aa9d9a257d7337e59c3dc7bb95e0ac4a16088b96cc9d6e/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/a436289744f8b6eff4aa9d9a257d7337e59c3dc7bb95e0ac4a16088b96cc9d6e/hostname",
        "HostsPath": "/var/lib/docker/containers/a436289744f8b6eff4aa9d9a257d7337e59c3dc7bb95e0ac4a16088b96cc9d6e/hosts",
        "LogPath": "/var/lib/docker/containers/a436289744f8b6eff4aa9d9a257d7337e59c3dc7bb95e0ac4a16088b96cc9d6e/a436289744f8b6eff4aa9d9a257d7337e59c3dc7bb95e0ac4a16088b96cc9d6e-json.log",
        "Name": "/trusting_herschel",
        "RestartCount": 0,
        "Driver": "overlay2",
        "Platform": "linux",
        "MountLabel": "",
        "ProcessLabel": "",
        "AppArmorProfile": "docker-default",
        "ExecIDs": null,
        "HostConfig": {
            "Binds": null,
            "ContainerIDFile": "",
            "LogConfig": {
                "Type": "json-file",
                "Config": {}
            },
            "NetworkMode": "default",
            "PortBindings": {},
            "RestartPolicy": {
                "Name": "no",
                "MaximumRetryCount": 0
            },
            "AutoRemove": false,
            "VolumeDriver": "",
            "VolumesFrom": null,
            "CapAdd": null,
            "CapDrop": null,
            "CgroupnsMode": "host",
            "Dns": [],
            "DnsOptions": [],
            "DnsSearch": [],
            "ExtraHosts": null,
            "GroupAdd": null,
            "IpcMode": "private",
            "Cgroup": "",
            "Links": null,
            "OomScoreAdj": 0,
            "PidMode": "",
            "Privileged": false,
            "PublishAllPorts": false,
            "ReadonlyRootfs": false,
            "SecurityOpt": null,
            "UTSMode": "",
            "UsernsMode": "",
            "ShmSize": 67108864,
            "Runtime": "runc",
            "ConsoleSize": [
                0,
                0
            ],
            "Isolation": "",
            "CpuShares": 0,
            "Memory": 0,
            "NanoCpus": 0,
            "CgroupParent": "",
            "BlkioWeight": 0,
            "BlkioWeightDevice": [],
            "BlkioDeviceReadBps": null,
            "BlkioDeviceWriteBps": null,
            "BlkioDeviceReadIOps": null,
            "BlkioDeviceWriteIOps": null,
            "CpuPeriod": 0,
            "CpuQuota": 0,
            "CpuRealtimePeriod": 0,
            "CpuRealtimeRuntime": 0,
            "CpusetCpus": "",
            "CpusetMems": "",
            "Devices": [],
            "DeviceCgroupRules": null,
            "DeviceRequests": null,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
            "OomKillDisable": false,
            "PidsLimit": null,
            "Ulimits": null,
            "CpuCount": 0,
            "CpuPercent": 0,
            "IOMaximumIOps": 0,
            "IOMaximumBandwidth": 0,
            "MaskedPaths": [
                "/proc/asound",
                "/proc/acpi",
                "/proc/kcore",
                "/proc/keys",
                "/proc/latency_stats",
                "/proc/timer_list",
                "/proc/timer_stats",
                "/proc/sched_debug",
                "/proc/scsi",
                "/sys/firmware"
            ],
            "ReadonlyPaths": [
                "/proc/bus",
                "/proc/fs",
                "/proc/irq",
                "/proc/sys",
                "/proc/sysrq-trigger"
            ]
        },
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/86bc5fcfa2964e3b4a6d4fb354a2cd6a07e9d96067dd53ace54847741961beb1-init/diff:/var/lib/docker/overlay2/56d8913431531f1fee44928723c86cb14779f8f005430ba5b40499601dc3bc24/diff",
                "MergedDir": "/var/lib/docker/overlay2/86bc5fcfa2964e3b4a6d4fb354a2cd6a07e9d96067dd53ace54847741961beb1/merged",
                "UpperDir": "/var/lib/docker/overlay2/86bc5fcfa2964e3b4a6d4fb354a2cd6a07e9d96067dd53ace54847741961beb1/diff",
                "WorkDir": "/var/lib/docker/overlay2/86bc5fcfa2964e3b4a6d4fb354a2cd6a07e9d96067dd53ace54847741961beb1/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "a436289744f8",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
            ],
            "Cmd": [
                "/bin/bash",
                "-c",
                "while true;do echo xdj;sleep 1;done"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20210915",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "6f1dfa9022bc64531a4018d0cac5b7fba4babca4afdcb680d2e91689730184cc",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/6f1dfa9022bc",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "009f0d194b1d03a6b91cc82b90ffedf5b2d097e3aa16810c5e82358ddd77ee21",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.3",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:03",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "dd5b8f7e1dc3c8cdaac96374d8758ae7f6a938be7a416322041771bb7eb27bd5",
                    "EndpointID": "009f0d194b1d03a6b91cc82b90ffedf5b2d097e3aa16810c5e82358ddd77ee21",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.3",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:03",
                    "DriverOpts": null
                }
            }
        }
    }
]
ubuntu@VM-12-15-ubuntu:~$ 
```

**进入当前正在运行的容器**

```shell
# 我们通常容器都是使用后台方式运行的，需要进入容器，修改一些配置

# 命令
docker exec -it 容器id bash shell

ubuntu@VM-12-15-ubuntu:~$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED         STATUS         PORTS                                                  NAMES
a436289744f8   centos         "/bin/bash -c 'while…"   8 minutes ago   Up 8 minutes                                                          trusting_herschel
5ab88b501192   7b94cda7ffc7   "docker-entrypoint.s…"   3 weeks ago     Up 3 weeks     33060/tcp, 0.0.0.0:3300->3306/tcp, :::3300->3306/tcp   per-admin-flask
ubuntu@VM-12-15-ubuntu:~$ docker exec -it a436289744f8
"docker exec" requires at least 2 arguments.
See 'docker exec --help'.

Usage:  docker exec [OPTIONS] CONTAINER COMMAND [ARG...]

Run a command in a running container
ubuntu@VM-12-15-ubuntu:~$ docker exec -it a436289744f8 /bin/bash
[root@a436289744f8 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var

# 方式二
docker attach 容器id

ubuntu@VM-12-15-ubuntu:~$ docker attach a436289744f8
正在执行当前的代码。。。。

# docker exec   # 进入容器后开启一个新的终端，可以在里面操作（常用）
# docker attach # 进入容器正在执行的终端，不会启动新的进程
```

**从容器内拷贝文件到主机上**

```shell
docker cp 容器id：容器内路径 目的主机路径


# 将这文件拷贝出来到主机上
ubuntu@VM-12-15-ubuntu:~$ docker exec -it a436289744f8 /bin/bash
[root@a436289744f8 /]# ls
bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
[root@a436289744f8 /]# cd /home/
[root@a436289744f8 home]# touch xdj.txt
[root@a436289744f8 home]# exit
exit
ubuntu@VM-12-15-ubuntu:~$ docker cp a436289744f8:/home/xdj.txt .
ubuntu@VM-12-15-ubuntu:~$ ls
blog  nohup.out  project  test  xdj.txt  密码.txt

# 拷贝是一个手动过程，未来我们使用-V卷的技术，可以实现
```

## 小结

![image-20220924164954461](../../../blog/source/picture/image-20220924164954461.png)

```shell
attach      Attach local standard input, output, and error streams to a running container  # 当前shell下attach连接指定运行镜像
build       Build an image from a Dockerfile  # 通过Dockerfile定制镜像
commit      Create a new image from a container's changes  # 提交当前容器为新的镜像
cp          Copy files/folders between a container and the local filesystem  # 从容器中拷贝指定文件或者目录到宿主机中
create      Create a new container  # 创建一个新的容器，同run，但不启动容器
diff        Inspect changes to files or directories on a container's filesystem  # 查看docker容器变化
events      Get real time events from the server  # 从docker服务获取容器实时事件
exec        Run a command in a running container  # 在已存在的容器上运行命令
export      Export a container's filesystem as a tar archive  # 导出容器的内容流作为一个tar归档文件【对应import】
history     Show the history of an image  # 展示一个镜像形成历史
images      List images  # 列出系统当前镜像
import      Import the contents from a tarball to create a filesystem image  # 从tar包中的内容创建一个新的文件系统镜像【对应export】
info        Display system-wide information  # 显示系统相关信息
inspect     Return low-level information on Docker objects  # 查看容器详细信息
kill        Kill one or more running containers  # kill指定docker容器
load        Load an image from a tar archive or STDIN  # 从一个tar包中加载一个镜像【对应save】
login       Log in to a Docker registry  # 注册或者登陆一个docker源服务器
logout      Log out from a Docker registry  # 从当前docker registry退出
logs        Fetch the logs of a container  # 输出当前容器日志信息
pause       Pause all processes within one or more containers  # 暂停容器
port        List port mappings or a specific mapping for the container # 查看映射端口对应的容器内部源端口
ps          List containers  # 列出容器列表
pull        Pull an image or a repository from a registry  # 从docker镜像源服务器拉去指定镜像
push        Push an image or a repository to a registry  # 推送指定镜像或者库镜像至docker源服务器
rename      Rename a container  # 重命名一个容器
restart     Restart one or more containers  # 重启运行的容器
rm          Remove one or more containers  # 移除一个或者多个容器
rmi         Remove one or more images  # 移除一个或者多个镜像【无容器使用该镜像才可以删除，否则需删除相关容器才可以继续或者-f强制删除】
run         Run a command in a new container  # 创建一个新的容器并运行一个命令
save        Save one or more images to a tar archive (streamed to STDOUT by default)  # 保存一个镜像为一个tar包【对应load】
search      Search the Docker Hub for images  # 在docker hub中搜索镜像
start       Start one or more stopped containers  # 启动容器
stats       Display a live stream of container(s) resource usage statistics  # 显示容器资源使用统计信息的实时流
stop        Stop one or more running containers  # 停止容器
tag         Create a tag TARGET_IMAGE that refers to SOURCE_IMAGE  # 给源中镜像打标签
top         Display the running processes of a container  # 查看容器中运行的进程信息
unpause     Unpause all processes within one or more containers  # 取消暂停容器
update      Update configuration of one or more containers  # 更新一个或者多个容器的配置
version     Show the Docker version information  # 查看docker版本号
wait        Block until one or more containers stop, then print their exit codes  # 截取容器停止时的退出状态值
```

docker的命令是十分多的，上面我们学习的那些都是最常用的容器和镜像的命令，之后我们还会学习很多命令

# 4.docker镜像

## 镜像是什么

镜像是一个轻量级、可执行的独立软件包，用来打包软件运行环境和基于运行环境开发的软件，它包含运行某个软件系统所需的所有内容，包括代码、运行时、库、环境变量和配置文件

## docker镜像加载原理

> ​	UnionFS（联合文件系统）

UnionFS（联合文件系统）：Union文件系统（UnionFS）是一种分层、轻量级并且高性能的文件系统，它支持对文件系统的修改作为一次提交来一层层的叠加，同时可以将不同目录挂载到同一个虚拟文件系统下。Union文件系统是docker镜像的基础。镜像可以用分层来继承，基于基础镜像（没有父镜像），可以制作各种具体的应用镜像

特性：一次同时加载多个文件系统，但从外面看起来，只能看到一个文件系统，联合加载会把各层文件系统叠加起来，这样最终的文件系统会包含所有底层的文件和目录

> docker镜像加载原理

docker的镜像实际由一层一层的文件系统组成，这种层级的文件系统UnionFS

bootfs（boot file system）主要包含bootload和kernel，bootloader主要引导加载kernel，Linux刚启动时会加载bootfs文件系统，在docker镜像的最底层时bootfs。这一层与我们典型的Linux/Unix系统是一样的，包含boot加载器和内核。当boot加载完成之后整个内核就都在内存中了，此时内存的使用权已由bootfs转交给内核，此时系统也会卸载bootfs。

rootfs（root file system），在bootfs之上，包含的就是典型Linux系统中的/dev，/proc，/bin，/etc等标准目录和文件。rootfs就是各种不同的操作系统发行版，比如ubuntu和centos等。

平时我们安装虚拟机的Centos都是好几个G，为什么docker这里才200M

centos        latest       5d0da3dc9764   12 months ago   231MB

对于一个精简的OS，rootfs可以很小，只需要包含最基本的命令、工具和程序库就可以了，因为底层直接用Host的kernel，自己只需提供rootfs就可以了。由此可见对于不同的linux发行版，bootfs基本是一致的，rootfs会有差别，因此不同的发行版可以共用bootfs。

## 分层理解

> 分层的镜像

我们可以去下载一个镜像，注意观察下载的日志输出，可以看到是一层一层的在下载

```shell
ubuntu@VM-12-15-ubuntu:~$ docker pull mysql
Using default tag: latest
latest: Pulling from library/mysql 
051f419db9dd: Pull complete
7627573fa82a: Pull complete 
a44b358d7796: Pull complete 
95753aff4b95: Pull complete 
a1fa3bee53f4: Pull complete 
f5227e0d612c: Pull complete 
b4b4368b1983: Pull complete 
f26212810c32: Pull complete 
d803d4215f95: Pull complete 
d5358a7f7d07: Pull complete 
435e8908cd69: Pull complete 
Digest: sha256:b9532b1edea72b6cee12d9f5a78547bd3812ea5db842566e17f8b33291ed2921  # 签名
Status: Downloaded newer image for mysql:latest
docker.io/library/mysql:latest  # 真实地址
```



思考：为什么docker镜像要采用这种分层的结构呢？

最大的好处，我们觉得莫过于是资源共享。比如有多个镜像都从相同的base镜像构建而来，那么宿主机只需要在磁盘上保留一份base镜像，同时内存中也只需要加载一份base镜像，这样就可以为所有的容器服务了，而且镜像的每一层都可以被共享。

查看镜像分层可以通过docker inspect images命令

```shell
ubuntu@VM-12-15-ubuntu:~$ docker inspect redis:latest 
[
    {
        "Id": "sha256:3e42dd4e79c7b6e416d06dde0de3e8b6cc73bf8f59dea9d3f784ac63cf4665a9",
        "RepoTags": [
            "redis:latest"
        ],
        "RepoDigests": [
            "redis@sha256:9bc34afe08ca30ef179404318cdebe6430ceda35a4ebe4b67d10789b17bdf7c4"
        ],
        "Parent": "",
        "Comment": "",
        "Created": "2022-08-02T12:40:06.412979617Z",
        "Container": "5096cecc5130602f4b743a9791c7cbde7a6e827c383609689f4b41f6ce5b52b8",
        "ContainerConfig": {
            "Hostname": "5096cecc5130",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "6379/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "GOSU_VERSION=1.14",
                "REDIS_VERSION=7.0.4",
                "REDIS_DOWNLOAD_URL=http://download.redis.io/releases/redis-7.0.4.tar.gz",
                "REDIS_DOWNLOAD_SHA=f0e65fda74c44a3dd4fa9d512d4d4d833dd0939c934e946a5c622a630d057f2f"
            ],
            "Cmd": [
                "/bin/sh",
                "-c",
                "#(nop) ",
                "CMD [\"redis-server\"]"
            ],
            "Image": "sha256:6be94c1612ab7a099aa87c4af3490344e7d9b7f7fe92a706322c3786750b0bc1",
            "Volumes": {
                "/data": {}
            },
            "WorkingDir": "/data",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": {}
        },
        "DockerVersion": "20.10.12",
        "Author": "",
        "Config": {
            "Hostname": "",
            "Domainname": "",
            "User": "",
            "AttachStdin": false,
            "AttachStdout": false,
            "AttachStderr": false,
            "ExposedPorts": {
                "6379/tcp": {}
            },
            "Tty": false,
            "OpenStdin": false,
            "StdinOnce": false,
            "Env": [
                "PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",
                "GOSU_VERSION=1.14",
                "REDIS_VERSION=7.0.4",
                "REDIS_DOWNLOAD_URL=http://download.redis.io/releases/redis-7.0.4.tar.gz",
                "REDIS_DOWNLOAD_SHA=f0e65fda74c44a3dd4fa9d512d4d4d833dd0939c934e946a5c622a630d057f2f"
            ],
            "Cmd": [
                "redis-server"
            ],
            "Image": "sha256:6be94c1612ab7a099aa87c4af3490344e7d9b7f7fe92a706322c3786750b0bc1",
            "Volumes": {
                "/data": {}
            },
            "WorkingDir": "/data",
            "Entrypoint": [
                "docker-entrypoint.sh"
            ],
            "OnBuild": null,
            "Labels": null
        },
        "Architecture": "amd64",
        "Os": "linux",
        "Size": 116807889,
        "VirtualSize": 116807889,
        "GraphDriver": {
            "Data": {
                "LowerDir": "/var/lib/docker/overlay2/a897e876e4ed1b04bf45bba6a61a000e838ee8d1e8a9cb96f851cde973f872a4/diff:/var/lib/docker/overlay2/d0b4e7c57d8c5c6de49a617328481c09accd881b0bb718fef0f114429a027334/diff:/var/lib/docker/overlay2/8b7ce84af01c33a70b5c43456fb83f37c1872fe5feb43a347ab3fea2878b8900/diff:/var/lib/docker/overlay2/843f0bfd9d149704c23743521a00a361bc50feefad650ac32c5cc744e32e9e8c/diff:/var/lib/docker/overlay2/11e3c9bdd100eb0dda8bd858b95ee8e0c87a63fe77f081a7b2ae6b20fd81bd11/diff",
                "MergedDir": "/var/lib/docker/overlay2/70342a1eeb6233166413bf9b39fe35c573bd048420e2e2be8cdce710a7a55584/merged",
                "UpperDir": "/var/lib/docker/overlay2/70342a1eeb6233166413bf9b39fe35c573bd048420e2e2be8cdce710a7a55584/diff",
                "WorkDir": "/var/lib/docker/overlay2/70342a1eeb6233166413bf9b39fe35c573bd048420e2e2be8cdce710a7a55584/work"
            },
            "Name": "overlay2"
        },
        "RootFS": {
            "Type": "layers",
            "Layers": [
                "sha256:92a4e8a3140f7a04a0e5a15793adef2d0e8889ed306a8f95a6cfb67cecb5f212",
                "sha256:a43bd94dc1da04cd0e0aa2ab59c6ebd4f327ec7303d479d9f94d16a2e33bb389",
                "sha256:be653abfb1ebb6c7a687e92bce6da4b4d77181d79da6c4809ac01ba2559b6cb9",
                "sha256:e65bbf45efd90444923a4b489a0cb0e2670d0220bf8a548c78d52dbe07d33d4c",
                "sha256:106dd27bf61fdf52af51a0a01e2ac84447aebf266da5881f76b03e24273c94f9",
                "sha256:03598b7f0dd4e725142c684d29310e099ff6d31d3b769a72bb578837d6594a8b"
            ]
        },
        "Metadata": {
            "LastTagTime": "0001-01-01T00:00:00Z"
        }
    }
]
ubuntu@VM-12-15-ubuntu:~$ 
```



**理解：**

所有的docker镜像都起始于一个基础镜像层，当进行修改或增加新的内容时，就会在当前镜像层之上，创建新的镜像层。

举一个简单的例子，假如基于Ubuntu Linux 16.04创建一个新的镜像，这就是新镜像的第一层；如果在该镜像中添加python包，就会在基础镜像层之上创建第二个镜像层；如果继续添加一个安全补丁，就会创建第三个镜像层。

该镜像当前已经包含3个镜像层，如下图所示：

![image-20220925122713353](../../../blog/source/picture/image-20220925122713353.png)

在添加额外的镜像层的同时，镜像始终保持是当前所有镜像的组合，理解这一点非常重要，下图举例，每个镜像层中包含3个文件，而镜像包含了来自两个镜像层的6个文件。

![image-20220925123307672](../../../blog/source/picture/image-20220925123307672.png)

> 特点

docker镜像都是只读的，当容器启动时，一个新的可写层被加载到镜像的顶部

这一层就是我们通常说的容器层，容器之下的都叫镜像层

## commit镜像

```shell
docker commit 提交容器成为一个新的副本

# 命令和git类似
docker commit -m='提交的描述信息' -a='作者' 容器id 目标镜像名：[TAG]
```

## 容器数据卷

### 什么是容器数据卷

数据卷就是宿主机上的一个目录或者文件，当容器目录和数据卷目录绑定后，对方的修改会立即同步。

一个数据卷可以被多个容器同时挂载

一个容器也可以被挂载多个数据卷

Docker容器删除后，在容器中产生的数据也会随之销毁，所以使用数据卷避免这种情况

### 使用数据卷

> 方式一：直接使用命令来挂在 -v

```shell
docker run -it -v 宿主机目录:容器内目录

ubuntu@VM-12-15-ubuntu:~$ docker run -it -v /home/ceshi:/home centos /bin/bash

# 启动起来时候我们可以通过docker inspect 容器id
```

![image-20220925163657008](/Users/xiongdejian/Library/Application Support/typora-user-images/image-20220925163657008.png)

**注意事项：**

1. 目录必须是绝对路径     

2. 如果目录不存在，会自动创建    

3. 可以挂载多个数据卷

### 具名和匿名挂载

```shell
# 匿名挂载
-v 容器路径
docker run -d -P --name xxx -v /etc/nginx nginx

# 查看所有的volume的情况
ubuntu@VM-12-15-ubuntu:~$ docker volume ls

# 这里发现，这种就是匿名挂载，我们的-v只写了容器内的路径，没有写容器外的路径
DRIVER    VOLUME NAME
local     0ca484a5fe6c8a85b4486edf8e3aa4d101d0323700e3c743051a916d46c47c12

# 具名挂载
ubuntu@VM-12-15-ubuntu:~$ docker run -d --name xxx -v xxx:/etc/nginx nginx
ubuntu@VM-12-15-ubuntu:~$ docker volume ls
DRIVER    VOLUME NAME
local     xxx

# 通过-v 卷名：容器内的路径
# 查看一下这个卷
docker volume inspect xxx
```

所有docker容器内的卷，没有指定目录的情况下都是在`/var/lib/docker/volumes/xxxx/`

我们通过具名挂载可以方便的找到我们的一个卷，大多数情况在使用的`具名挂载`

```shell
# 如何确定是具名挂载还是匿名挂载，还是指定路径挂载
-v 容器内的路径 # 匿名挂载
-v 卷名：容器内的路径  # 具名挂载
-v /宿主机路径：容器内路径 # 指定路径挂载
```

**拓展：**

```shell
# 通过-v指定容器内路径，ro rw改变读写权限
ro readonly  # 只读
rw readwrite # 可读可写

# 一旦这个设置了容器权限，容器对我们挂载出来的内容就有限定了
docker run -d -P --name nginx02 -v xxx:/etc/nginx:ro nginx
docker run -d -P --name nginx02 -v xxx:/etc/nginx:rw nginx

# ro 只要看到ro就说明这个路径只能通过宿主机来操作，容器内部无法操作
```

### 初识Dockerfile

Dockerfile就是用来构建docker镜像的构建文件，命令脚本

通过这个脚本可以生成镜像，镜像是一层一层的，脚本就是一个一个命令

```shell
# 创建一个dockerfile文件，名字可以随机 建议Dockerfile
# 文件中的内容 指令（大写）参数
FROM centos

VOLUME ["volume01","volume02"]

CMD echo "---"

CMD /bin/bash

# 这里的每个命令，就是镜像的一层
```

### 数据卷容器

如果用户需要在多个容器之间共享一些持续更新的数据，最简单的方式是使用数据卷容器。
数据卷容器也是一个容器，但是它的目的是专门提供数据卷给其他容器挂载

```shell
# 1.新建数据卷容器
docker run -it -d --name db_data -v /db_data centos

# 2.用容器db1测试数据卷容器是否可用
docker run -it -d --name db1 --volumes-from db_data centos
docker exec -it db1 bash
cd db_data
## 使用echo命令向db1_test.txt中写入内容
echo "db1 xdj test" > db1_test.txt
```

容器之间配置信息的传递，数据卷容器的生命周期一直持续到没有容器使用为止。但是一旦持久化到了本地，这个时候本地的数据是不会删除的

## Dockerfile

### Dockerfile介绍

dockerfile是用来构建docker镜像的文件，命令参数脚本

构建步骤：

1. 编写一个dockerfile文件
2. docker build构建成为一个镜像
3. docker run运行镜像
4. docker push发布镜像

很多官方镜像都是基础包，很多功能没有，我们通常会自己搭建自己的镜像

### Dockerfile构建过程

**基础知识：**

1. 每个保留关键字（指令）都是必须是大写字母
2. 执行从上到下顺序执行
3. #表示注释
4. 每一个指令都会创建提交一个新的镜像层，并提交

dockerfile是面向开发的，我们以后要发布项目，做镜像

dockerfile：构建文件，定义了一切的步骤，源代码

dockerimage：通过Dockerfile构建生成的镜像，最终发布和运行的产品

docker容器：容器就是镜像运行起来提供服务器

### Dokerfile指令

```shell
FROM  					# 基础镜像，一切从这里开始构建
MAINTAINER  		# 镜像是谁写的
RUN 						# 镜像构建的时候需要运行的命令
ADD 						# 步骤，添加内容，会自动解压
WORKDIR					# 镜像的工作目录
VOLUME					# 挂载的目录
EXPOST					# 暴露端口的配置
CMD							# 指定这个容器启动的时候要运行的命令
ENTRYPOINT			# 指定这个容器启动的时候要运行的命令，可以追加命令
ONBUILD					# 当构建一个被继承Dockerfile，这个时候就会运行ONBUILD的指令，触发命令
COPY						# 类似ADD，将我们的文件拷贝到镜像中
ENV							# 构建的时候设置环境变量
```

> CMD和ENTRYPOINT的区别

测试CMD

```shell
# 编写dockerfile文件
ubuntu@VM-12-15-ubuntu:~/test$ vim test_cmd_dockerfile
FROM centos
CMD ["ls", "-a"]

# 构建镜像
ubuntu@VM-12-15-ubuntu:~/test$ docker build -f test_cmd_dockerfile -t test_cmd:1.0 .
Sending build context to Docker daemon  6.144kB
Step 1/2 : FROM centos
 ---> 5d0da3dc9764
Step 2/2 : CMD ["ls", "-a"]
 ---> Running in 7197d957458d
Removing intermediate container 7197d957458d
 ---> 8d3b996dd6dd
Successfully built 8d3b996dd6dd
Successfully tagged test_cmd:1.0

# run运行，发现我们的ls -a命令
ubuntu@VM-12-15-ubuntu:~/test$ docker run -it test_cmd:1.0 
.  ..  .dockerenv  bin  dev  etc  home  lib  lib64  lost+found  media  mnt  opt  proc  root  run  sbin  srv  sys  tmp  usr  var
ubuntu@VM-12-15-ubuntu:~/test$ 

# 想追加一个命令 -l ls -al
ubuntu@VM-12-15-ubuntu:~/test$ docker run 8d3b996dd6dd -l
docker: Error response from daemon: failed to create shim: OCI runtime create failed: runc create failed: unable to start container process: exec: "-l": executable file not found in $PATH: unknown.
ERRO[0000] error waiting for container: context canceled 

# cmd的清理下 -l 替换了CMD ["ls", "-a"]命令， -l不是命令所以报错
ubuntu@VM-12-15-ubuntu:~/test$ docker run 8d3b996dd6dd ls -al
total 56
drwxr-xr-x   1 root root 4096 Sep 27 14:50 .
drwxr-xr-x   1 root root 4096 Sep 27 14:50 ..
-rwxr-xr-x   1 root root    0 Sep 27 14:50 .dockerenv
lrwxrwxrwx   1 root root    7 Nov  3  2020 bin -> usr/bin
drwxrwxrwx   5 root root  340 Sep 27 14:50 dev
drwxrwxrwx   1 root root 4096 Sep 27 14:50 etc
drwxrwxrwx   2 root root 4096 Nov  3  2020 home
lrwxrwxrwx   1 root root    7 Nov  3  2020 lib -> usr/lib
lrwxrwxrwx   1 root root    9 Nov  3  2020 lib64 -> usr/lib64
drwxrwxrwx   2 root root 4096 Sep 15  2021 lost+found
drwxrwxrwx   2 root root 4096 Nov  3  2020 media
drwxrwxrwx   2 root root 4096 Nov  3  2020 mnt
drwxrwxrwx   2 root root 4096 Nov  3  2020 opt
dr-xr-xr-x 210 root root    0 Sep 27 14:50 proc
drwxrwxrwx   2 root root 4096 Sep 15  2021 root
drwxrwxrwx  11 root root 4096 Sep 15  2021 run
lrwxrwxrwx   1 root root    8 Nov  3  2020 sbin -> usr/sbin
drwxrwxrwx   2 root root 4096 Nov  3  2020 srv
dr-xr-xr-x  13 root root    0 Sep 27 14:50 sys
drwxrwxrwx   7 root root 4096 Sep 15  2021 tmp
drwxrwxrwx  12 root root 4096 Sep 15  2021 usr
drwxrwxrwx  20 root root 4096 Sep 15  2021 var
```

测试ENTRYPOINT

```shell
ubuntu@VM-12-15-ubuntu:~/test$ vim test_entrupoint_dockerfile
FROM centos
ENTRYPOINT ["ls", "-a"]

ubuntu@VM-12-15-ubuntu:~/test$ docker build -f test_entrupoint_dockerfile -t entrupoint:1.0 .
Sending build context to Docker daemon  7.168kB
Step 1/2 : FROM centos
 ---> 5d0da3dc9764
Step 2/2 : ENTRYPOINT ["ls", "-a"]
 ---> Running in c17914142dbd
Removing intermediate container c17914142dbd
 ---> 58dd681e3523
Successfully built 58dd681e3523
Successfully tagged entrupoint:1.0
ubuntu@VM-12-15-ubuntu:~/test$ docker run -it entrupoint:1.0 
.   .dockerenv  dev  home  lib64       media  opt   root  sbin  sys  usr
..  bin         etc  lib   lost+found  mnt    proc  run   srv   tmp  var

# 我们的追加命令，是直接拼接在我们的ENTRYPOINT命令的后面
ubuntu@VM-12-15-ubuntu:~/test$ docker run -it entrupoint:1.0 -l
total 56
drwxr-xr-x   1 root root 4096 Sep 27 14:54 .
drwxr-xr-x   1 root root 4096 Sep 27 14:54 ..
-rwxr-xr-x   1 root root    0 Sep 27 14:54 .dockerenv
lrwxrwxrwx   1 root root    7 Nov  3  2020 bin -> usr/bin
drwxrwxrwx   5 root root  360 Sep 27 14:54 dev
drwxrwxrwx   1 root root 4096 Sep 27 14:54 etc
drwxrwxrwx   2 root root 4096 Nov  3  2020 home
lrwxrwxrwx   1 root root    7 Nov  3  2020 lib -> usr/lib
lrwxrwxrwx   1 root root    9 Nov  3  2020 lib64 -> usr/lib64
drwxrwxrwx   2 root root 4096 Sep 15  2021 lost+found
drwxrwxrwx   2 root root 4096 Nov  3  2020 media
drwxrwxrwx   2 root root 4096 Nov  3  2020 mnt
drwxrwxrwx   2 root root 4096 Nov  3  2020 opt
dr-xr-xr-x 209 root root    0 Sep 27 14:54 proc
drwxrwxrwx   2 root root 4096 Sep 15  2021 root
drwxrwxrwx  11 root root 4096 Sep 15  2021 run
lrwxrwxrwx   1 root root    8 Nov  3  2020 sbin -> usr/sbin
drwxrwxrwx   2 root root 4096 Nov  3  2020 srv
dr-xr-xr-x  13 root root    0 Sep 27 14:54 sys
drwxrwxrwx   7 root root 4096 Sep 15  2021 tmp
drwxrwxrwx  12 root root 4096 Sep 15  2021 usr
drwxrwxrwx  20 root root 4096 Sep 15  2021 var
```

编写dockerfile文件，官方命令`Dockerfile`，build会自动寻找这个文件，就不需要-f指定了

## Docker网络

### 理解docker网络

四类网络

| Docker网络模式 | 配置                       | 说明                                                         |
| -------------- | :------------------------- | ------------------------------------------------------------ |
| host模式       | -- net=host                | 容器和宿主机共享Network namespace                            |
| container模式  | --net=container:NAME_or_ID | 容器和另一个容器共享Network namespace。k8s中的pod就是多个容器共享一个Netword namespace |
| none模式       | --net=none                 | 容器有独立的Netword namespace，单并没有对其进行任何网络设置，如分配veth pair和网络桥接，配置ip等 |
| bridge模式     | --net=bridge               | （默认为该模式）                                             |

#### host模式

host网络模式需要在容器创建时指定–network=host
**hots模式是和宿主机公用一个网段和端口，缺陷是会隔离性差，会占用宿主机的端口，做不到自定任意端口。优点，无需做网络策略，只要能访问到宿主机，就能访问到容器**

![在这里插入图片描述](../../../blog/source/picture/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5LiW5aSW5bmz5bi45Lq6,size_20,color_FFFFFF,t_70,g_se,x_16.png)

#### container模式

这个模式是两个容器之间可以相互通信，虽然是两个容器，但可以理解为在同一容器里面，可以用localhost访问。两个容器除了网络方面，其他的如文件系统、进程列表等还是隔离的。

![在这里插入图片描述](../../../blog/source/picture/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5LiW5aSW5bmz5bi45Lq6,size_20,color_FFFFFF,t_70,g_se,x_16-20221026215549993.png)

#### none模式

使用none模式，Docker容器拥有自己的Network Namespace，但是，并不为Docker容器进行任何网络配置。也就是说，这个Docker容器没有网卡、IP、路由等信息。需要我们自己为Docker容器添加网卡、配置IP等。

![在这里插入图片描述](../../../blog/source/picture/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5LiW5aSW5bmz5bi45Lq6,size_20,color_FFFFFF,t_70,g_se,x_16-20221026215613911.png)

#### bridge模式

![在这里插入图片描述](../../../blog/source/picture/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5LiW5aSW5bmz5bi45Lq6,size_18,color_FFFFFF,t_70,g_se,x_16.png)

bridge模式下容器没有一个公有ip,只有宿主机可以直接访问,外部主机是不可见的,但容器通过宿主机的NAT规则后可以访问外网。

优点：Docker Daemon 利用 veth pair 技术，在宿主机上创建两个虚拟网络接口设备，假设为veth0 和 veth1。而
veth pair 技术的特性可以保证无论哪一个 veth 接收到网络报文，都会将报文传输给另一方。

Docker Daemon 将 veth0 附加到 Docker Daemon 创建的 docker0
网桥上。保证宿主机的网络报文可以发往 veth0；

Docker Daemon 将 veth1 添加到 Docker Container 所属的 namespace 下，并被改名为
eth0。如此一来，保证宿主机的网络报文若发往 veth0，则立即会被 eth0 接收，实现宿主机到Docker Container网络的联通性；同时，也保证 Docker Container 单独使用 eth0，实现容器网络环境的隔离性

缺点：1.最明显的是，该模式下 Docker Container 不具有一个公有 IP，即和宿主机的 eth0 不处于同一个网段。导致的结果是宿主机以外的世界不能直接和容器进行通信。
2.虽然 NAT 模式经过中间处理实现了这一点，但是 NAT 模式仍然存在问题与不便，如：容器均需要在宿主机上竞争端口，容器内部服务的访问者需要使用服务发现获知服务的外部端口等。
3.另外 NAT 模式由于是在三层网络上的实现手段，故肯定会影响网络的传输效率。