## 部署Nginx
1. 搜索Nginx镜像
```shell script  
docker search nginx
```
2. 下载Nginx镜像
```shell script
docker pull nginx
```
3. 启动Nginx容器
```shell script
docker run -d --name nginx01 -p 3344:80 nginx
```
4. 测试
```shell script
curl localhost:3344

<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
```

5. 进入容器
```shell script
docker exec -it nignx01 /bin/bash
```


## 安装Tomcat
1. 搜索Tomcat
```shell script
docker search tomcat
```
2. 下载Tomcat
```shell script
docker pull tomcat
```
3. 启动Tomcat
```shell script
docker run -d --name tomcat01 -p 3344:8080 tomcat
```
4. 进入容器
```shell script
docker exec -it tomcat01 /bin/bash
```
