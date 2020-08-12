# Docker
Docker实战

## [Install on Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
## [Install on CentOS7](https://docs.docker.com/engine/install/centos/)
1. 设置阿里云镜像仓库

```shell script
yum-config-manager \
> --add-repo \
> http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

```

2. 更新软件包索引
```shell script
yum makecache fast
```

3. 安装docker
```shell script
yum install docker-ce docker-ce-cli containerd.io
```

4. 启动docker
```shell script
systemctl start docker
```

5. 卸载docker
```shell script
yum remove docker-ce docker-ce-cli containerd.io

rm -rf /var/lib/docker
```
6. 配置阿里云容器镜像服务
```shell script
登录阿里云 --> 容器镜像服务 ---> 镜像加速器
```

## [Docker 常用命令](./command.md)

## [Docker 镜像](./images.md)

## [Docker 容器数据卷](./volumes.md)