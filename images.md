## images 镜像
镜像： 镜像是一种轻量级、可执行的独立软件包。它包含运行某个软件的所有内容：代码，运行时，环境变量和配置文件。

### Docker镜像加载原理
1. UnionFS 联合文件系统  
    * 分层、轻量级、高性能文件系统
    * 将对系统的修改作为一次提交来一层层的叠加
    
2. BootFS Linux启动时会加载BootFS主要包括bootloader 和 kernel
3. RootFS 在BootFS之上，Linux系统中的/dev,/proc, /bin等标准目录和文件；RootFS就是各种不同的操作系统的发行版本。
4. Docker镜像加载原理

### commit 镜像
```shell script
docker commit --help

Usage:  docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]

Create a new image from a container's changes

Options:
  -a, --author string    Author (e.g., "John Hannibal Smith <hannibal@a-team.com>")
  -c, --change list      Apply Dockerfile instruction to the created image
  -m, --message string   Commit message
  -p, --pause            Pause container during commit (default true)
```