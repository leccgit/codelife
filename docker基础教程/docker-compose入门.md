# docker-compose入门

docker-compose：帮助定义和共享容器应用程序的工具，使用yaml文件定义，可以快捷的使用命令启动和停止容器

## 检查是否安装docker-compose

```markdown
docker-compose version
```

## docker-compose 文件写法

1. 在当前app的根目录下，创建一个名为docker-compose.yaml文件。
2. 撰写文件时候，首先需要从定义文件模版开始，大多是时候，优先使用最新支持的版本（查看自己安装的docker-compose支持版本）。
3. docker运行命令如下

```shell
docker run -dp 3000:3000 \
  -w /app -v "$(pwd):/app" \
  --network todo-app \
  -e MYSQL_HOST=mysql \
  -e MYSQL_USER=root \
  -e MYSQL_PASSWORD=secret \
  -e MYSQL_DB=todos \
  node:12-alpine \
  sh -c "yarn install && yarn run dev"
```

## 转换docker-compose文件步骤如下

1. 为容器定义服务入口和指定镜像，可以为服务选择任何名称，该名称会自动成为网络别名

```yaml
version: "3.7"

service:
  app:
    image: node:12-alpine
```

2. 通常，会把`command`放在离`image`较近的地方，注意对于放置的顺序没有要求。

```yaml
version: "3.7"

service:
  app:
    image: node:12-alpine
    command: sh -c "yarn install && yarn run dev"
```

3. 指定端口映射 -p 3000:
   3000，该处使用的为短语法，也有详细的[长语法](https://docs.docker.com/compose/compose-file/compose-file-v3/#long-syntax-1)可以使用

```yaml
version: "3.7"
service:
  app:
    image: node:12-alpine
    command: sh -c "yarn install && yarn run dev"
    ports:
      - 3000:3000
```

4. 使用 `working-dir`来指定工作目录（-w /app）,使用`volumes`来指定工作目录和卷的映射关系`-v "$(pwd):/app"`)，Volumes
   也有[短句](https://docs.docker.com/compose/compose-file/compose-file-v3/#short-syntax-3)和 [长](https://docs.docker.com/compose/compose-file/compose-file-v3/#long-syntax-3)句。
   ps: 使用docker-compose时候，可以使用当前目录的相对路径

```yaml
version: "3.7"
service:
  app:
    image: node:12-alpine
    command: sh -c "yarn install && yarn run dev"
    ports:
      - 3000:3000
    working_dir: /app
    volumes:
      - ./:/app
```

5. 根据需要使用的 `environment`来指定环境变量

```yaml
version: "3.7"
service:
  app:
    image: node:12-alpine
    command: sh -c "yarn install && yarn run dev"
    ports:
      - 3000:3000
    working_dir: /app
    volumes:
      - ./:/app
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: secret
      MYSQL_DB: todos
```

## 定义mysql服务

1. docker运行命令如下

```shell
docker run -d \
  --network todo-app --network-alias mysql \
  -v todo-mysql-data:/var/lib/mysql \
  -e MYSQL_ROOT_PASSWORD=secret \
  -e MYSQL_DATABASE=todos \
  mysql:5.7
```

2. 定义服务

```yaml
version: "3.7"
service:
  app:
  # The app service definition
  mysql:
    image: mysql:5.7
    volumes:
      - todo-mysql-data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD:secret
      MYSQL_DATABASE:todos
volumes:
  todo-mysql-data:
```

## 完整的docker-compose文件如下

```yaml
version: "3.7"

service:
  app:
    image: node:12-alpine
    command: sh -c "yarn install && yarn run dev"
    ports:
      - 3000:3000
    working_dir: /app
    volumes:
      - ./:/app
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: secret
      MYSQL_DB: todos

  mysql:
    image: mysql:5.7
    volumes:
      - todo-mysql-data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD:secret
      MYSQL_DATABASE:todos
volumes:
  todo-mysql-data:
```

1. 启动docker-compose

```shell
docker-compose up
```

2. 删除容器

```shell
docker-compose down
```

ps：运行`docker-compose down` 容器将停止，网络也被删除，但是`compose`中命名的卷不会被直接删除，如果要删除卷，需要显示的指定 --volumes标志