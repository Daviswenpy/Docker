### 帮助命令
```shell script
* docker version
* docker info
* docker command --help
```

### 镜像命令
* docker images  
```shell script
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE

Options:  
  -a, --all             Show all images (default hides intermediate images)  
      --digests         Show digests  
  -f, --filter filter   Filter output based on conditions provided  
      --format string   Pretty-print images using a Go template  
      --no-trunc        Don't truncate output  
  -q, --quiet           Only show numeric IDs  
```

```shell script
REPOSITORY: 镜像的仓库源  
TAG: 镜像的标签  
IMAGE ID: 镜像的ID  
CREATE: 镜像的创建时间  
SIZE: 镜像的大小  
```


* docker search 搜索镜像  
```shell script
Options:  
  -f, --filter filter   Filter output based on conditions provided  
      --format string   Pretty-print search using a Go template  
      --limit int       Max number of search results (default 25)  
      --no-trunc        Don't truncate output 
```
 
      
* docker pull 下载镜像  
默认latest版本

* docker rmi 删除镜像  
```shell script
Remove one or more images

Options:  
  -f, --force      Force removal of the image  
      --no-prune   Do not delete untagged parents  
      
      
docker rmi -f $(docker images -aq) --- 删除所有镜像
```


### 容器命令

* docker run image  

常用参数：  
     --name string                    Assign a name to the container  
     -d, --detach                     Run container in background and print container ID  
     -i, --interactive                Keep STDIN open even if not attached  
     -t, --tty                        Allocate a pseudo-TTY   
     -p, --publish list               Publish a container's port(s) to the host  
     -P, --publish-all                Publish all exposed ports to random ports  
     
     
* docker ps 列出运行的容器  

List containers

Options:  
  -a, --all             Show all containers (default shows just running)  
  -f, --filter filter   Filter output based on conditions provided  
      --format string   Pretty-print containers using a Go template  
  -n, --last int        Show n last created containers (includes all states) (default -1)  
  -l, --latest          Show the latest created container (includes all states)  
      --no-trunc        Don't truncate output  
  -q, --quiet           Only display numeric IDs  
  -s, --size            Display total file sizes  
  
* 退出容器  
exit：容器停止并退出  
Ctrl+P+Q：容器不停止且退出  

* 容器删除  
docker rm 容器id  
docker rm -f 容器id  

* 启动和停止容器
```shell script
docker start 容器id
docker restart 
docker stop
docker skill
```

### 其他命令
* 后台启动容器
```shell script
docker run -d centos
```

* 查看日志
```shell script
docker logs --help

Usage:  docker logs [OPTIONS] CONTAINER

Fetch the logs of a container

Options:
      --details        Show extra details provided to logs
  -f, --follow         Follow log output
      --since string   Show logs since timestamp (e.g. 2013-01-02T13:23:37) or relative (e.g. 42m
                       for 42 minutes)
      --tail string    Number of lines to show from the end of the logs (default "all")
  -t, --timestamps     Show timestamps
      --until string   Show logs before a timestamp (e.g. 2013-01-02T13:23:37) or relative (e.g.
                       42m for 42 minutes)
```

* 查看容器的元数据
```shell script
命令: docker inspect 容器id

[
    {
        "Id": "f38f514fe275e9ed9bbf6e26db278c93d55445cbfbb5eb7795466f273e354ca6",
        "Created": "2020-08-11T09:50:05.24802069Z",
        "Path": "/bin/sh",
        "Args": [
            "-c",
            "while true;do echo melon;sleep 1;done"
        ],
        "State": {
            "Status": "running",
            "Running": true,
            "Paused": false,
            "Restarting": false,
            "OOMKilled": false,
            "Dead": false,
            "Pid": 29455,
            "ExitCode": 0,
            "Error": "",
            "StartedAt": "2020-08-11T09:50:05.867109911Z",
            "FinishedAt": "0001-01-01T00:00:00Z"
        },
        "Image": "sha256:0d120b6ccaa8c5e149176798b3501d4dd1885f961922497cd0abef155c869566",
        "ResolvConfPath": "/var/lib/docker/containers/f38f514fe275e9ed9bbf6e26db278c93d55445cbfbb5eb7795466f273e354ca6/resolv.conf",
        "HostnamePath": "/var/lib/docker/containers/f38f514fe275e9ed9bbf6e26db278c93d55445cbfbb5eb7795466f273e354ca6/hostname",
        "HostsPath": "/var/lib/docker/containers/f38f514fe275e9ed9bbf6e26db278c93d55445cbfbb5eb7795466f273e354ca6/hosts",
        "LogPath": "/var/lib/docker/containers/f38f514fe275e9ed9bbf6e26db278c93d55445cbfbb5eb7795466f273e354ca6/f38f514fe275e9ed9bbf6e26db278c93d55445cbfbb5eb7795466f273e354ca6-json.log",
        "Name": "/nifty_khorana",
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
            "Capabilities": null,
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
                "LowerDir": "/var/lib/docker/overlay2/f03de815e21e5fadd45a0c0381296ea47849193b41679aea5b543ed485e0644b-init/diff:/var/lib/docker/overlay2/9f675e79003d204247c8d5e99c48dbcc0c2dea96e66fe41b946c51a77fe1df21/diff",
                "MergedDir": "/var/lib/docker/overlay2/f03de815e21e5fadd45a0c0381296ea47849193b41679aea5b543ed485e0644b/merged",
                "UpperDir": "/var/lib/docker/overlay2/f03de815e21e5fadd45a0c0381296ea47849193b41679aea5b543ed485e0644b/diff",
                "WorkDir": "/var/lib/docker/overlay2/f03de815e21e5fadd45a0c0381296ea47849193b41679aea5b543ed485e0644b/work"
            },
            "Name": "overlay2"
        },
        "Mounts": [],
        "Config": {
            "Hostname": "f38f514fe275",
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
                "/bin/sh",
                "-c",
                "while true;do echo melon;sleep 1;done"
            ],
            "Image": "centos",
            "Volumes": null,
            "WorkingDir": "",
            "Entrypoint": null,
            "OnBuild": null,
            "Labels": {
                "org.label-schema.build-date": "20200809",
                "org.label-schema.license": "GPLv2",
                "org.label-schema.name": "CentOS Base Image",
                "org.label-schema.schema-version": "1.0",
                "org.label-schema.vendor": "CentOS"
            }
        },
        "NetworkSettings": {
            "Bridge": "",
            "SandboxID": "d0b177cf946234b771ff730d00f986549b4d7b8fec3d39c07116d081276cece6",
            "HairpinMode": false,
            "LinkLocalIPv6Address": "",
            "LinkLocalIPv6PrefixLen": 0,
            "Ports": {},
            "SandboxKey": "/var/run/docker/netns/d0b177cf9462",
            "SecondaryIPAddresses": null,
            "SecondaryIPv6Addresses": null,
            "EndpointID": "8c03530f0239495f89902c428c435ebfd471205fbc65b1ab1516cb502e1273d5",
            "Gateway": "172.17.0.1",
            "GlobalIPv6Address": "",
            "GlobalIPv6PrefixLen": 0,
            "IPAddress": "172.17.0.2",
            "IPPrefixLen": 16,
            "IPv6Gateway": "",
            "MacAddress": "02:42:ac:11:00:02",
            "Networks": {
                "bridge": {
                    "IPAMConfig": null,
                    "Links": null,
                    "Aliases": null,
                    "NetworkID": "631ff695781c025731bcff8a76c346c3b419bd0481d085baf84b041032451490",
                    "EndpointID": "8c03530f0239495f89902c428c435ebfd471205fbc65b1ab1516cb502e1273d5",
                    "Gateway": "172.17.0.1",
                    "IPAddress": "172.17.0.2",
                    "IPPrefixLen": 16,
                    "IPv6Gateway": "",
                    "GlobalIPv6Address": "",
                    "GlobalIPv6PrefixLen": 0,
                    "MacAddress": "02:42:ac:11:00:02",
                    "DriverOpts": null
                }
            }
        }
    }
]
```

* 进入正在运行的容器（非常重要）
```shell script
命令: docker exec --help

Usage:  docker exec [OPTIONS] CONTAINER COMMAND [ARG...]

Run a command in a running container

Options:
  -d, --detach               Detached mode: run command in the background
      --detach-keys string   Override the key sequence for detaching a container
  -e, --env list             Set environment variables
  -i, --interactive          Keep STDIN open even if not attached
      --privileged           Give extended privileges to the command
  -t, --tty                  Allocate a pseudo-TTY
  -u, --user string          Username or UID (format: <name|uid>[:<group|gid>])
  -w, --workdir string       Working directory inside the container


命令: docker exec -it 交互模式
例: docker exec -it 容器id /bin/bash


命令: docker attach --help
Usage:  docker attach [OPTIONS] CONTAINER

Attach local standard input, output, and error streams to a running container

Options:
      --detach-keys string   Override the key sequence for detaching a container
      --no-stdin             Do not attach STDIN
      --sig-proxy            Proxy all received signals to the process (default true)
```

* 从容器内拷贝文件到主机上
```shell script
命令: docker cp --help
Usage:  docker cp [OPTIONS] CONTAINER:SRC_PATH DEST_PATH|-
        docker cp [OPTIONS] SRC_PATH|- CONTAINER:DEST_PATH

Copy files/folders between a container and the local filesystem

Use '-' as the source to read a tar archive from stdin
and extract it to a directory destination in a container.
Use '-' as the destination to stream a tar archive of a
container source to stdout.

Options:
  -a, --archive       Archive mode (copy all uid/gid information)
  -L, --follow-link   Always follow symbol link in SRC_PATH
```

