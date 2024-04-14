02 docker部署QAC


# 1.环境docker安装

```shell
sudo apt install docker.io
```

# 2.docker添加sudo权限

```shell
sudo groupadd docker
sudo gpasswd -a ${USER} docker
sudo service docker restart
```

# 3.修改docker配置

Linux/ubuntu在/etc/docker/daemon.json（若无该文件则创建以{}包围如下代码）增加如下配置：

/etc/docker/daemon.json

```shell
{
  "registy-mirrors": ["http://qiyb9988.mirror.aliyuncs.com"]
}
```

修改完以上配置后，重启docker服务

```shell
systemctl daemon-reload
systemctl restart docker
```

# 4.编译dockerfile

```dockerfile
FROM ubuntu:18.04
RUN apt update -y
WORKDIR /QAC
COPY . /QAC
RUN chmod +x *.run
RUN ./Helix-QAC-2022.2-3045-Linux.run --quiet
RUN ./M3CM-3.3.0-Linux.run --quiet
RUN rm -rf *.run
```

# 5.构建

```shell
docker build -t qac:latest .
```

# 6.启动容器

```shell
docker run -itd --name qac --rm -v /home/ubuntu/jenkins/project:/home/ubuntu/jenkins qac
```

