# docker常用应用

### mysql

```shell
docker run -dp 3306:3306 --restart=always --name mircr_mysql -e MYSQL_ROOT_PASSWORD=123456 mysql:5.7
```

### postgres

```shell
docker run -dp 5432:5432 --restart=always --name micro_postgres  -e POSTGRES_PASSWORD=123456 postgres
```

### redis

```shell
docker run -dp 6379:6379 --restart=always --name micro_redis redis
```

### mongodb

```shell
docker run -dp 27017:27017 --name micro_mongo -v ~/docker_app/micro_mongo:/mongodb  mongo --auth
# 添加mongo的用户和密码, 建议加上，不然被比特币勒索...
db.createUser({ user: "root", pwd: "123456", roles: [{ role: "userAdminAnyDatabase", db: "admin" }] })
#Read：允许用户读取指定数据库
#readWrite：允许用户读写指定数据库
#dbAdmin：允许用户在指定数据库中执行管理函数，如索引创建、删除，查看统计或访问system.profile
#userAdmin：允许用户向system.users集合写入，可以找指定数据库里创建、删除和管理用户
#clusterAdmin：只在admin数据库中可用，赋予用户所有分片和复制集相关函数的管理权限。
#readAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读权限
#readWriteAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的读写权限
#userAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的userAdmin权限
#dbAdminAnyDatabase：只在admin数据库中可用，赋予用户所有数据库的dbAdmin权限。
#root：只在admin数据库中可用。超级账号，超级权限

```

### rabbitmq

```shell
docker run --restart=always -d -p 15672:15672 -p 5672:5672 --hostname micro_rabbitmq --name micro_rabbitmq -e
RABBITMQ_DEFAULT_USER=root -e RABBITMQ_DEFAULT_PASS=123456 rabbitmq:3.7.16-management
#15672 ：表示 RabbitMQ 控制台端口号，可以在浏览器中通过控制台来执行 RabbitMQ 的相关操作。
#5672: 表示 RabbitMQ 所监听的 TCP 端口号，应用程序可通过该端口与 RabbitMQ 建立 TCP连接，完成后续的异步消息通信 
#RABBITMQ_DEFAULT_USER：用于设置登陆控制台的用户名
#RABBITMQ_DEFAULT_PASS：用于设置登陆控制台的密码
#容器启动成功后，可以在浏览器输入地址：http://ip:15672/访问控制台
```

### docker时区设置

```shell
#时区启动命令： -v /etc/timezone:/etc/timezone:ro -v /etc/localtime:/etc/localtime:ro 
#例子：
docker run --restart=always -d -p 8080:3000 --name wiki --restart unless-stopped -e "DB_TYPE=postgres" -e "
DB_HOST=127.0.0.1" -e "DB_PORT=5432" -e "DB_USER=root" -e "DB_PASS=123456" -e "DB_NAME=wiki" -v
/etc/timezone:/etc/timezone:ro -v /etc/localtime:/etc/localtime:ro ghcr.io/requarks/wiki:2
```
