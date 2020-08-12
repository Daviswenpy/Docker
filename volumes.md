## 容器数据卷
容器的持久化和同步操作！容器之间也是可以数据共享的

### 使用数据卷
1. -v 挂载
```shell script
docker run -it -v /home/ceshi:/home centos /bin/bash
```
将主机上/home/ceshi目录挂载到容器的/home目录。

### 实战MySQL同步数据
1. 启动MySQL容器，需要数据挂载，需要配置密码
```shell script
# 官方测试
docker run --name some-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:tag

# 启动MySQL
docker run -d -p 3389:3306 -v /home/mysql/conf:/etc/mysql/conf.d -v /home/mysql/data:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=123456 --name mysql01 mysql

-d 后台运行
-p 指定端口
-v 卷挂载
-e 环境配置
--name 容器名
镜像名

# 测试远程连接
```

### 具名挂载、匿名挂载
```shell script
# 匿名挂载
-v 容器内路径
dockers run -d -P --name nginx01 -v /etc/nginx nginx

[root@Melon home]# docker volume ls
DRIVER              VOLUME NAME
local               29aa04eff74d5cbadc1a10a50bea9043fc5d979235e1ec58d84377aa62e7f56c


# 具名挂载
-v 卷名: 容器内路径

[root@Melon volumes]# docker run -d -P --name nginx02 -v nginx_volume:/etc/nginx nginx
214363c0fe52a820ca353f29224c8821ec70b2e06ec01909fcf35422c8847a7c
[root@Melon volumes]# docker volume ls
DRIVER              VOLUME NAME
local               nginx_volume
[root@Melon volumes]# docker volume inspect nginx_volume
[
    {
        "CreatedAt": "2020-08-12T15:00:55+08:00",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/nginx_volume/_data",
        "Name": "nginx_volume",
        "Options": null,
        "Scope": "local"
    }
]
```
没有指定目录的情况下都是挂载在到/var/lib/docker/volumes/XXXXX/_data

### 控制挂载读写权限
```shell script
ro : readonly  只读(容器内只读)
rw : readWrite 读写

docker run -d -P --name nginx03 /etc/nginx:rw nginx
```

### 数据卷容器
```shell script
--volumes-from 容器
备份机制, 双向拷贝的概念
```