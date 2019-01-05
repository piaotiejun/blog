---
title: Hello World
---
Welcome to [Hexo](https://hexo.io/)! This is your very first post. Check [documentation](https://hexo.io/docs/) for more info. If you get any problems when using Hexo, you can find the answer in [troubleshooting](https://hexo.io/docs/troubleshooting.html) or you can ask me on [GitHub](https://github.com/hexojs/hexo/issues).

## Quick Start

### Create a new post

``` bash
$ hexo new "文章名称"
```

More info: [Writing](https://hexo.io/docs/writing.html)

### Run server

``` bash
$ hexo server
$ hexo s
```

More info: [Server](https://hexo.io/docs/server.html)

### Generate static files

``` bash
$ hexo generate
$ hexo g
```

More info: [Generating](https://hexo.io/docs/generating.html)

### Deploy to remote sites

``` bash
$ hexo deploy
```

More info: [Deployment](https://hexo.io/docs/deployment.html)


### sqlserver
sudo docker run -e 'ACCEPT_EULA=Y' -e 'MSSQL_SA_PASSWORD=Ruoweiedu123!' -p 1433:1433 \
-v /data/sqlserver/db1:/var/opt/mssql \
--name sqlserver1 -d microsoft/mssql-server-linux:2017-latest

sa
Ruoweiedu123!

CREATE DATABASE xiaofang COLLATE Chinese_PRC_CI_AS;

### nginx
docker run \
-p 80:80 \
--name nginx1 \
-v /data/nginx/nginx1/www:/www \
-v /data/nginx/nginx1/conf/nginx.conf:/etc/nginx/nginx.conf \
-v /data/nginx/nginx1/logs:/wwwlogs  \
-d nginx  


### java
1.打包war
./gradlew bootRepackage -Pdev

2.创建docker镜像
/Users/potiejun/workspace/docker/xiaofang
docker build -t piaotiejun/xiaofang:v1 .
docker push piaotiejun/xiaofang:v1

3.服务器pull
docker pull piaotiejun/xiaofang:v1

4.服务器运行
docker run -d \
-p 80:80 -p 81:81 \
--name xiaofang \
-v /data/projects/xiaofang/log:/log \
-v /data/projects/xiaofang/upload:/upload \
-v /data/projects/xiaofang/static:/static \
piaotiejun/xiaofang:v1


### oracle
https://hub.docker.com/r/sath89/oracle-12c

docker run -d -p 8080:8080 -p 1521:1521 --name oracle1 sath89/oracle-12c

http://192.168.1.160:8080/em/
sys oracle

http://192.168.1.160:8080/apex/apex


### 禅道
https://hub.docker.com/r/idoop/zentao

docker run -d -p 82:80 -p 3307:3306 \
        -e USER="root" -e PASSWD="password" \
        -e BIND_ADDRESS="false" \
        -e SMTP_HOST="163.177.90.125 smtp.exmail.qq.com" \
        -v /data/zentao/zbox1/:/opt/zbox/ \
        --name zentao-server \
        idoop/zentao:latest







