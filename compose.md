## Docker Compose 
Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application’s services. Then, with a single command, you create and start all the services from your configuration.

### [官方文档](https://docs.docker.com/compose/)

### 使用compose三步骤：
1. Define your app’s environment with a "Dockerfile" so it can be reproduced anywhere.
2. Define the services that make up your app in "docker-compose.yml" so they can be run together in an isolated environment.
3. Run docker-compose up and Compose starts and runs your entire app.


### 作用：批量容器编排

Compose是Docker官方的一个开源项目，需要下载。[安装](https://docs.docker.com/compose/install/)  
[centos7.6](https://juejin.im/post/6844903965381885966)

例：
```yaml
version: '2.0'
services:
  web:
    build: .
    ports:
    - "5000:5000"
    volumes:
    - .:/code
    - logvolume01:/var/log
    links:
    - redis
  redis:
    image: redis
volumes:
  logvolume01: {}
```

### 重要概念：
1. 服务services：容器，应用（web，redis，mysql）
2. 项目project：一组关联的容器（我们最终要生成的是一个项目）

### [Getting started](https://docs.docker.com/compose/gettingstarted/)
Step 1: Setup
```shell script
$ mkdir composetest
$ cd composetest
```

```shell script
# create app.py

import time

import redis
from flask import Flask

app = Flask(__name__)
cache = redis.Redis(host='redis', port=6379)


def get_hit_count():
    retries = 5
    while True:
        try:
            return cache.incr('hits')
        except redis.exceptions.ConnectionError as exc:
            if retries == 0:
                raise exc
            retries -= 1
            time.sleep(0.5)


@app.route('/')
def hello():
    count = get_hit_count()
    return 'Hello World! I have been seen {} times.\n'.format(count)
```

```shell script
# create requirements.txt

flask
redis
```

Step 2: Create a Dockerfile
```shell script
FROM python:3.7-alpine
WORKDIR /code
ENV FLASK_APP app.py
ENV FLASK_RUN_HOST 0.0.0.0
RUN apk add --no-cache gcc musl-dev linux-headers
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt
EXPOSE 5000
COPY . .
CMD ["flask", "run"]
```

Step 3: Define services in a Compose file
```shell script
version: '3'
services:
  web:
    build: .
    ports:
      - "5000:5000"
  redis:
    image: "redis:alpine"
```
Step 4: Build and run your app with Componse 
```shell script
$ docker-compose up
Creating network "composetest_default" with the default driver
Creating composetest_web_1 ...
Creating composetest_redis_1 ...
Creating composetest_web_1
Creating composetest_redis_1 ... done
Attaching to composetest_web_1, composetest_redis_1
web_1    |  * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
redis_1  | 1:C 17 Aug 22:11:10.480 # oO0OoO0OoO0Oo Redis is starting oO0OoO0OoO0Oo
redis_1  | 1:C 17 Aug 22:11:10.480 # Redis version=4.0.1, bits=64, commit=00000000, modified=0, pid=1, just started
redis_1  | 1:C 17 Aug 22:11:10.480 # Warning: no config file specified, using the default config. In order to specify a config file use redis-server /path/to/redis.conf
web_1    |  * Restarting with stat
redis_1  | 1:M 17 Aug 22:11:10.483 * Running mode=standalone, port=6379.
redis_1  | 1:M 17 Aug 22:11:10.483 # WARNING: The TCP backlog setting of 511 cannot be enforced because /proc/sys/net/core/somaxconn is set to the lower value of 128.
web_1    |  * Debugger is active!
redis_1  | 1:M 17 Aug 22:11:10.483 # Server initialized
redis_1  | 1:M 17 Aug 22:11:10.483 # WARNING you have Transparent Huge Pages (THP) support enabled in your kernel. This will create latency and memory usage issues with Redis. To fix this issue run the command 'echo never > /sys/kernel/mm/transparent_hugepage/enabled' as root, and add it to your /etc/rc.local in order to retain the setting after a reboot. Redis must be restarted after THP is disabled.
web_1    |  * Debugger PIN: 330-787-903
redis_1  | 1:M 17 Aug 22:11:10.483 * Ready to accept connections
```

步骤:
1. 创建网络
2. 执行Docker-compose yaml 
3. 启动服务

规则：
1. componse里的镜像自动构建下载，
2. 默认服务名: 项目名_服务名_num
3. 网络规则：项目名_default  bridge local （项目中的内容都在同一个网络中）


Step 5: Edit the compose file to add a bind mount

Step 6: Re-build and run the app with Compose

### [yaml 规则](https://docs.docker.com/compose/compose-file/)
```shell script
# 三层!

version: '' # 版本
services:   # 服务
    服务一:
    服务二:

其他配置: # 网络，卷， 全局规则
```