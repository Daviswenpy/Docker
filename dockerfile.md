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