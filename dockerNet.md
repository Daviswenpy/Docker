## docker 网络 docker0

```shell script
[root@Melon ~]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:16:3e:0a:5c:a6 brd ff:ff:ff:ff:ff:ff
    inet 172.19.108.56/20 brd 172.19.111.255 scope global dynamic eth0
       valid_lft 315167864sec preferred_lft 315167864sec
3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default
    link/ether 02:42:11:cd:6e:9c brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
```
1. lo: 本机循环地址 127.0.0.1/8
2. eth0: 阿里云内网地址
3. docker0: docker0地址

### 问题：docker如何处理容器网络访问?

```shell script
[root@Melon ~]# docker exec -it tomcat01 ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
46: eth0@if47: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever
```
docker 分配容器一个ip地址：eth0@if47

```shell script
[root@Melon ~]# ping 172.17.0.2
PING 172.17.0.2 (172.17.0.2) 56(84) bytes of data.
64 bytes from 172.17.0.2: icmp_seq=1 ttl=64 time=0.085 ms
64 bytes from 172.17.0.2: icmp_seq=2 ttl=64 time=0.063 ms
64 bytes from 172.17.0.2: icmp_seq=3 ttl=64 time=0.060 ms
64 bytes from 172.17.0.2: icmp_seq=4 ttl=64 time=0.061 ms
```
linux 能够ping通

```shell script
[root@Melon ~]# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 00:16:3e:0a:5c:a6 brd ff:ff:ff:ff:ff:ff
    inet 172.19.108.56/20 brd 172.19.111.255 scope global dynamic eth0
       valid_lft 315167142sec preferred_lft 315167142sec
3: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether 02:42:11:cd:6e:9c brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
47: veth6d39c66@if46: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default
```

### veth-pair 技术

### 容器互联

### 自定义网络
```shell script
# 查看所有docker网络

[root@Melon ~]# docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
6f9fb6a4332c        bridge              bridge              local
bb547e623152        host                host                local
707076f78ab1        none                null                local
```
网络模式：
1. bridge：桥接 docker（默认）
2. host：和宿主主机共享网络
3. none：不配置网络

```shell script
[root@Melon ~]# docker network --help

Usage:  docker network COMMAND

Manage networks

Commands:
  connect     Connect a container to a network
  create      Create a network
  disconnect  Disconnect a container from a network
  inspect     Display detailed information on one or more networks
  ls          List networks
  prune       Remove all unused networks
  rm          Remove one or more networks

```

```shell script
Usage:  docker network create [OPTIONS] NETWORK

Create a network

Options:
      --attachable           Enable manual container attachment
      --aux-address map      Auxiliary IPv4 or IPv6 addresses used by Network driver (default map[])
      --config-from string   The network from which copying the configuration
      --config-only          Create a configuration only network
  -d, --driver string        Driver to manage the Network (default "bridge")
      --gateway strings      IPv4 or IPv6 Gateway for the master subnet
      --ingress              Create swarm routing-mesh network
      --internal             Restrict external access to the network
      --ip-range strings     Allocate container ip from a sub-range
      --ipam-driver string   IP Address Management Driver (default "default")
      --ipam-opt map         Set IPAM driver specific options (default map[])
      --ipv6                 Enable IPv6 networking
      --label list           Set metadata on a network
  -o, --opt map              Set driver specific options (default map[])
      --scope string         Control the network's scope
      --subnet strings       Subnet in CIDR format that represents a network segment
```

```shell script
[root@Melon ~]# docker network create --driver bridge --subnet 192.168.0.0/16 --gateway 192.168.0.1 mynet
9ac6fef2aa000164f0bf374eb203b5c44883daf5f89d0aa3263e1359d5e7a1b3
[root@Melon ~]# docker network ls
NETWORK ID          NAME                DRIVER              SCOPE
6f9fb6a4332c        bridge              bridge              local
bb547e623152        host                host                local
9ac6fef2aa00        mynet               bridge              local
707076f78ab1        none                null                local
```

```shell script
[root@Melon ~]# docker network inspect mynet
[
    {
        "Name": "mynet",
        "Id": "9ac6fef2aa000164f0bf374eb203b5c44883daf5f89d0aa3263e1359d5e7a1b3",
        "Created": "2020-08-14T16:12:00.281883465+08:00",
        "Scope": "local",
        "Driver": "bridge",
        "EnableIPv6": false,
        "IPAM": {
            "Driver": "default",
            "Options": {},
            "Config": [
                {
                    "Subnet": "192.168.0.0/16",
                    "Gateway": "192.168.0.1"
                }
            ]
        },
        "Internal": false,
        "Attachable": false,
        "Ingress": false,
        "ConfigFrom": {
            "Network": ""
        },
        "ConfigOnly": false,
        "Containers": {},
        "Options": {},
        "Labels": {}
    }
]
```

使用自定义网络：
```shell script
[root@Melon ~]# docker run -d -P --name tomcat01 --net mynet tomcat
1cb24ba69544f5d8df2a5fa90a342a476ba9f314332968d4c6cfd7a0e597aedf
[root@Melon ~]# docker run -d -P --name tomcat02 --net mynet tomcat
f129ce92bc4227dc96d9c454768a9017cbf2fffba138a02a77f99481c0dff6cd

[root@Melon ~]# docker exec -it tomcat01 ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
49: eth0@if50: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether 02:42:c0:a8:00:02 brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.168.0.2/16 brd 192.168.255.255 scope global eth0
       valid_lft forever preferred_lft forever

[root@Melon ~]# docker exec -it tomcat01 ping tomcat02
PING tomcat02 (192.168.0.3) 56(84) bytes of data.
64 bytes from tomcat02.mynet (192.168.0.3): icmp_seq=1 ttl=64 time=0.115 ms
64 bytes from tomcat02.mynet (192.168.0.3): icmp_seq=2 ttl=64 time=0.073 ms
^C
--- tomcat02 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 2ms
rtt min/avg/max/mdev = 0.073/0.094/0.115/0.021 ms
```

好处：  
对不同的集群使用不同的网络，各网络之间隔离

### 网络联通 docker network connect
```shell script
[root@Melon ~]# docker network connect --help

Usage:  docker network connect [OPTIONS] NETWORK CONTAINER

Connect a container to a network

Options:
      --alias strings           Add network-scoped alias for the container
      --driver-opt strings      driver options for the network
      --ip string               IPv4 address (e.g., 172.30.100.104)
      --ip6 string              IPv6 address (e.g., 2001:db8::33)
      --link list               Add link to another container
      --link-local-ip strings   Add a link-local address for the container
```

打通容器-->网络
```shell script
# 一个容器两个ip

[root@Melon ~]# docker network connect mynet tomcat03
[root@Melon ~]# docker exec -it tomcat03 ping tomcat01

```

### [Docker 四种网络模式](https://juejin.im/post/6844903756920782855)
