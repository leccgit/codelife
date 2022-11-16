# dockerfile优化写法

## 镜像检查

```markdown
seq1：docker scan --login seq2： docker scan <image-name>
```

## 查看镜像的构建历史

```markdown
docker image history <image-name>
```

## dockerfile分析

> ps: 当docker中的构建层，发生变化时候，所有基于该层的构建都需要重新构建操作
> 每个docker-file指令都会成为镜像的一个新层，当我们对镜像进行更改时，需要重新安装yarn依赖

```dockerfile
# syntax=docker/dockerfile:1
FROM node:12-alpine
WORKDIR /app
COPY . .
RUN yarn install --production
CMD ["node", "src/index.js"]
```

对此，明显node环境不会轻易发生改变，主要的变动集中在用户代码处，对此可以的优化写法如下，由于用户代码处于构建的上层，在该处用户的代码发生了变动，也不需要重新安装yarn

```dockerfile
# syntax=docker/dockerfile:1
FROM node:12-alpine
WORKDIR /app
COPY package.json yarn.lock ./
RUN yarn install --production
COPY . .
CMD ["node", "src/index.js"]
```