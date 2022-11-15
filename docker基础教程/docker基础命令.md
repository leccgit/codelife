# docker基础文档
> 基础命令
ps命令
```dockerfile
docket ps       # 列出所有正在运行的容器
docker ps -a    # 列出所有的容器
docker ps -l    # 列出最后一次运行的容器,不管是否正在运行
docker ps -n x  # 显示最后n个容器,不管容器是否运行还是停止
docker ps -a -q # 显示所有容器的id(-p命令)
```
> docker安装ubuntu
```markdown
docker run -it ubuntu /bin/bash
    -i：保证标准输入 stdin开启
    -t：分配tty终端
命令解释：基于ubuntu构建容器(没有该容器，docker会向dockerhub下载)最后使用/bin/bash命令,运行容器内的shell
备注: 默认安装的ubuntu没有自带vim，需要额外安装 apt-get update && apt-get install vim 
```
> 容器命名
```markdown
docker run -it --name my_ubuntu ubuntu /bin/bash
my_ubuntu：将构建的ubuntu容器命名为my_ubuntu，注意docker中容器的名字唯一，不然需要删除同名的容器，才能继续构建
ps：运行中的容器需要stop之后在进行删除
```
> 容器操作
重启停止的容器
```markdown
sudo docker start my_ubuntu
or 
sudo docker restart my_ubuntu
```

> 附着到容器上
```markdown
sudo docker attach xxxx
ps：退出容器shell时,容器也会随之停止运行
```

> 创建守护式容器（守护进程，常用于生产环境）
```markdown
sudo docker run -d --name my_ubuntu ubuntu /bin/sh -c "while true; do echo hello world; sleep 1;done"
命令解释：
    -d：docker会将容器放置于后台运行
```

> 容器日志查看
```markdown
sudo docker logs my_ubuntu 
sudo docker logs my_ubuntu --tail -f 
```

> 在容器内部运行进程
```markdown
docker exec -it my_ubuntu /bin/bash
注意：在没有bash的环境，也可以启动其他的终端 例如： /sh
```

> 设置容器自动重启
```markdown
docker run -d --restart=always --name my_ubuntu  ubuntu /bin/sh -c "while true; do echo hello world; sleep 1;done"
命令解释：
    --restart=alwags：无论容器退出码是多少,docker都会自动重启容器
    --restart=on-failure:5：只有容器退出码为非0值的时候,容器才会进行重启,当前设置最大重启次数为5次
```

> 深入容器
```markdown
docker inspect my_ubuntu
命令解释：
    inspect：检查容器参数配置，返回配置信息、名称、命令、网络等有效命令
    备注：docker inspect支持同时指定多个容器。所有的容器，默认保存在 /var/lib/docker/containers目录下
```

> 删除容器
```markdown
docker rm my_ubuntu
备注：运行中的容器无法被删除，需要使用docker stop 或者 docker kill命令将对应的容器stop，才能继续执行删除命令
```

> docker file