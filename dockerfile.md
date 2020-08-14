## DockerFile
DockerFile就是用来构建docker镜像的构建文件！命令脚本。

构建步骤：
1. 编写一个dockerfile文件
2. docker build构建成为一个镜像
3. docker run 运行镜像
4. docker push 发布镜像（DockerHub 阿里云镜像仓库）


基本知识： 
1. 每个保留关键字（指令）都是大写的
2. 执行从上到下顺序执行
3. #表示注释
4. 每一个指令都会创建提交一个新的镜像曾（最后一层为一个可写container）


dockerfile是面向开发的，docker镜像逐渐成为了企业交付的标准。  

基本概念：  
1. dockerfile：构建文件
2. dockerImages：通过dockerfile生成的镜像
3. docker容器：镜像运行起来提供服务


### dockerfile指令
```shell script
FROM      # 基础镜像，一从这里开始构建
MAINTAINER   # 镜像是谁写的   姓名+邮箱
RUN       # 镜像构建的时候需要执行的命令
ADD       # 其他资源
WORKDIR   # 工作目录
VOLUME    # 挂载目录
EXPOSE    # 暴露端口
CMD       # 容器启动的时候要运行的命令 （只有最后一个会生效）
ENTRYPOINT # 容器启动的时候要运行的命令 （可以追加）
ENV
```

### 实战：
```shell script
FROM centos

MAINTAINER melon<1603871134@qq.com>

ENV MYPATH /usr/local

WORKDIR $MYPATH

RUN yum -y  install vim
RUN yum -y  install net-tools

EXPOSE 80

CMD echo $MYPATH
CMD echo "------------end------------"
CMD /bin/bash
```

```shell script
docker build -f mycentos_dockerfile -t name:tag .
```

查看镜像的构建过程：
```shell script
[root@Melon dockerfiles]# docker history 08393e824c32
IMAGE               CREATED             CREATED BY                                      SIZE                COMMENT
08393e824c32        9 days ago          /bin/sh -c #(nop)  CMD ["nginx" "-g" "daemon…   0B
<missing>           9 days ago          /bin/sh -c #(nop)  STOPSIGNAL SIGTERM           0B
<missing>           9 days ago          /bin/sh -c #(nop)  EXPOSE 80                    0B
<missing>           9 days ago          /bin/sh -c #(nop)  ENTRYPOINT ["/docker-entr…   0B
<missing>           9 days ago          /bin/sh -c #(nop) COPY file:0fd5fca330dcd6a7…   1.04kB
<missing>           9 days ago          /bin/sh -c #(nop) COPY file:1d0a4127e78a26c1…   1.96kB
<missing>           9 days ago          /bin/sh -c #(nop) COPY file:e7e183879c35719c…   1.2kB
<missing>           9 days ago          /bin/sh -c set -x     && addgroup --system -…   63.3MB
<missing>           9 days ago          /bin/sh -c #(nop)  ENV PKG_RELEASE=1~buster     0B
<missing>           9 days ago          /bin/sh -c #(nop)  ENV NJS_VERSION=0.4.2        0B
<missing>           9 days ago          /bin/sh -c #(nop)  ENV NGINX_VERSION=1.19.1     0B
<missing>           9 days ago          /bin/sh -c #(nop)  LABEL maintainer=NGINX Do…   0B
<missing>           9 days ago          /bin/sh -c #(nop)  CMD ["bash"]                 0B
<missing>           9 days ago          /bin/sh -c #(nop) ADD file:3af3091e7d2bb40bc…   69.2MB
```

### 实战：Tomcat镜像
```shell script

FROM centos

MAINTAINER melon<1603871134@qq.com>

COPY readme.txt /usr/local/readme.txt

ADD apache-tomcat-8.5.57-embed.tar.gz /usr/local
ADD jdk-8u261-linux-x64.tar.gz /usr/local

RUN yum -y install vim

ENV MYPATH /usr/local
WORK $MYPATH

ENV JAVA_HOME /usr/local/jdk1.8.0_261
ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV CATALINA_HOME /usr/local/apache-tomcat-8.5.57-embed
ENV CATALINA_BASH /usr/local/apache-tomcat-8.5.57-embed
ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin

EXPOSE 8080
CMD /usr/local/apache-tomcat-8.5.57-embed/bin/startup.sh && tail -F /url/local/apache-tomcat-8.5.57-embed/bin/logs/catalina.out

```

```shell script
docker build -t diytomcat:1.0 .

Step 1/15 : FROM centos
 ---> 0d120b6ccaa8
Step 2/15 : MAINTAINER melon<1603871134@qq.com>
 ---> Using cache
 ---> 636148cd5933
Step 3/15 : COPY readme.txt /usr/local/readme.txt
COPY failed: stat /var/lib/docker/tmp/docker-builder313482487/readme.txt: no such file or directory
[root@Melon mytomcat]# vim readme.txt
[root@Melon mytomcat]# docker build -t diytomcat:1.0 .
Sending build context to Docker daemon  149.9MB
Step 1/15 : FROM centos
 ---> 0d120b6ccaa8
Step 2/15 : MAINTAINER melon<1603871134@qq.com>
 ---> Using cache
 ---> 636148cd5933
Step 3/15 : COPY readme.txt /usr/local/readme.txt
 ---> a8986bf7f479
Step 4/15 : ADD apache-tomcat-8.5.57-embed.tar.gz /usr/local
 ---> d1f54734284a
Step 5/15 : ADD jdk-8u261-linux-x64.tar.gz /usr/local
 ---> 88c2b44dac51
Step 6/15 : RUN yum -y install vim
 ---> Running in 7502f7cc63f6
CentOS-8 - AppStream                            3.3 MB/s | 5.8 MB     00:01
CentOS-8 - Base                                 1.2 MB/s | 2.2 MB     00:01
CentOS-8 - Extras                               7.2 kB/s | 7.0 kB     00:00
Dependencies resolved.
================================================================================
 Package             Arch        Version                   Repository      Size
================================================================================
Installing:
 vim-enhanced        x86_64      2:8.0.1763-13.el8         AppStream      1.4 M
Installing dependencies:
 gpm-libs            x86_64      1.20.7-15.el8             AppStream       39 k
 vim-common          x86_64      2:8.0.1763-13.el8         AppStream      6.3 M
 vim-filesystem      noarch      2:8.0.1763-13.el8         AppStream       48 k
 which               x86_64      2.21-12.el8               BaseOS          49 k

Transaction Summary
================================================================================
Install  5 Packages

Total download size: 7.8 M
Installed size: 31 M
Downloading Packages:
(1/5): gpm-libs-1.20.7-15.el8.x86_64.rpm        849 kB/s |  39 kB     00:00
(2/5): vim-filesystem-8.0.1763-13.el8.noarch.rp 3.9 MB/s |  48 kB     00:00
(3/5): vim-enhanced-8.0.1763-13.el8.x86_64.rpm   10 MB/s | 1.4 MB     00:00
(4/5): vim-common-8.0.1763-13.el8.x86_64.rpm     23 MB/s | 6.3 MB     00:00
(5/5): which-2.21-12.el8.x86_64.rpm             226 kB/s |  49 kB     00:00
--------------------------------------------------------------------------------
Total                                           4.3 MB/s | 7.8 MB     00:01
warning: /var/cache/dnf/AppStream-02e86d1c976ab532/packages/gpm-libs-1.20.7-15.el8.x86_64.rpm: Header V3 RSA/SHA256 Signature, key ID 8483c65d: NOKEY
CentOS-8 - AppStream                             71 kB/s | 1.6 kB     00:00
Importing GPG key 0x8483C65D:
 Userid     : "CentOS (CentOS Official Signing Key) <security@centos.org>"
 Fingerprint: 99DB 70FA E1D7 CE22 7FB6 4882 05B5 55B3 8483 C65D
 From       : /etc/pki/rpm-gpg/RPM-GPG-KEY-centosofficial
Key imported successfully
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                        1/1
  Installing       : which-2.21-12.el8.x86_64                               1/5
  Installing       : vim-filesystem-2:8.0.1763-13.el8.noarch                2/5
  Installing       : vim-common-2:8.0.1763-13.el8.x86_64                    3/5
  Installing       : gpm-libs-1.20.7-15.el8.x86_64                          4/5
  Running scriptlet: gpm-libs-1.20.7-15.el8.x86_64                          4/5
  Installing       : vim-enhanced-2:8.0.1763-13.el8.x86_64                  5/5
  Running scriptlet: vim-enhanced-2:8.0.1763-13.el8.x86_64                  5/5
  Running scriptlet: vim-common-2:8.0.1763-13.el8.x86_64                    5/5
  Verifying        : gpm-libs-1.20.7-15.el8.x86_64                          1/5
  Verifying        : vim-common-2:8.0.1763-13.el8.x86_64                    2/5
  Verifying        : vim-enhanced-2:8.0.1763-13.el8.x86_64                  3/5
  Verifying        : vim-filesystem-2:8.0.1763-13.el8.noarch                4/5
  Verifying        : which-2.21-12.el8.x86_64                               5/5

Installed:
  gpm-libs-1.20.7-15.el8.x86_64         vim-common-2:8.0.1763-13.el8.x86_64
  vim-enhanced-2:8.0.1763-13.el8.x86_64 vim-filesystem-2:8.0.1763-13.el8.noarch
  which-2.21-12.el8.x86_64

Complete!
Removing intermediate container 7502f7cc63f6
 ---> e725a7d370a2
Step 7/15 : ENV MYPATH /usr/local
 ---> Running in abcd1adcaefa
Removing intermediate container abcd1adcaefa
 ---> fd57796774f4
Step 8/15 : WORKDIR $MYPATH
 ---> Running in 1984205aca97
Removing intermediate container 1984205aca97
 ---> bf27f04ea6bd
Step 9/15 : ENV JAVA_HOME /usr/local/jdk1.8
 ---> Running in aa3bb7097b7c
Removing intermediate container aa3bb7097b7c
 ---> 51c369aa4a39
Step 10/15 : ENV CLASSPATH $JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar
 ---> Running in 6b2405a18266
Removing intermediate container 6b2405a18266
 ---> 912679a1120c
Step 11/15 : ENV CATALINA_HOME /usr/local/apache-tomcat-8.5.57-embed
 ---> Running in c01fa024c0ea
Removing intermediate container c01fa024c0ea
 ---> 4fe0c4f407d3
Step 12/15 : ENV CATALINA_BASH /usr/local/apache-tomcat-8.5.57-embed
 ---> Running in 0788e80b86ca
Removing intermediate container 0788e80b86ca
 ---> 4cd7a0065052
Step 13/15 : ENV PATH $PATH:$JAVA_HOME/bin:$CATALINA_HOME/lib:$CATALINA_HOME/bin
 ---> Running in bb0215aa7e13
Removing intermediate container bb0215aa7e13
 ---> 252348eee49e
Step 14/15 : EXPOSE 8080
 ---> Running in 2f0a50c2cdcb
Removing intermediate container 2f0a50c2cdcb
 ---> 4c5c48a99ed5
Step 15/15 : CMD /usr/local/apache-tomcat-8.5.57-embed/bin/startup.sh && tail -F /url/local/apache-tomcat-8.5.57-embed/bin/logs/catalina.out
 ---> Running in 905e51498b59
Removing intermediate container 905e51498b59
 ---> 83d296d7c104
Successfully built 83d296d7c104
Successfully tagged diytomcat:1.0
```

运行测试：
```shell script
docker run -d -p 9090:8080 --name melon_tomcat -v /home/melon/build/tomcat/test:/usr/local/apache-tomcat-8.5.57-embed/webapps/test -v /home/melon/build/tomcat/tomcatlogs/:/usr/local/apache-tomcat-8.5.57-embed/logs diytomcat:1.0
```


发布镜像到docker hub
```shell script
docker login

docker push
```

发布镜像到阿里云服务器
```shell script
1. 登录阿里云
2. 容器镜像服务
3. 创建命令空间
4. 创建镜像仓库(本地镜像仓库)
5. 参考阿里云教程


1. 登录阿里云Docker Registry
$ sudo docker login --username=17379395811 registry.cn-shanghai.aliyuncs.com
用于登录的用户名为阿里云账号全名,密码为开通服务时设置的密码

您可以在访问凭证页面修改凭证密码

2. 从Registry中拉取镜像
$ sudo docker pull registry.cn-shanghai.aliyuncs.com/melon/test:[镜像版本号]
3. 将镜像推送到Registry
$ sudo docker login --username=17379395811 registry.cn-shanghai.aliyuncs.com
$ sudo docker tag [ImageId] registry.cn-shanghai.aliyuncs.com/melon/test:[镜像版本号]
$ sudo docker push registry.cn-shanghai.aliyuncs.com/melon/test:[镜像版本号]
请根据实际镜像信息替换示例中的[ImageId]和[镜像版本号]参数

4. 选择合适的镜像仓库地址
从ECS推送镜像时,可以选择使用镜像仓库内网地址,推送速度将得到提升并且将不会损耗您的公网流量

如果您使用的机器位于VPC网络,请使用 registry-vpc.cn-shanghai.aliyuncs.com 作为Registry的域名登录,并作为镜像命名空间前缀。
5. 示例
使用"docker tag"命令重命名镜像,并将它通过专有网络地址推送至Registry,

$ sudo docker images
REPOSITORY                                                         TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
registry.aliyuncs.com/acs/agent                                    0.7-dfb6816         37bb9c63c8b2        7 days ago          37.89 MB
$ sudo docker tag 37bb9c63c8b2 registry-vpc.cn-shanghai.aliyuncs.com/acs/agent:0.7-dfb6816
使用"docker images"命令找到镜像,将该镜像名称中的域名部分变更为Registry专有网络地址,

$ sudo docker push registry-vpc.cn-shanghai.aliyuncs.com/acs/agent:0.7-dfb6816
```
