# docker-series

整理自己开发过程中经常用到 docker 相关操作

<!-- TOC -->

- [node.js](#node.js)
- [vue.js](#vue)

<!-- /TOC -->

## node.js

> 因为本人使用的是 egg.js 框架，并且打算一直使用下去，所以 docker 操作就以 egg.js 为主。

+ 第一步，修改 `package.json` 的启动指令 

**egg.js官方文档：**`--daemon` 是否允许在**后台模式**，无需 `nohup`。若使用 `Docker` 建议直接**前台运行**。

```json
"scripts": {
    "docker": "egg-scripts start --title=egg-server-padipata"
}
```

+ 第二步，创建 `.dockerignore` 文件过滤

```.dockerignore
node_modules/
.git/
.idea/
```

+ 第三步，创建 `Dockerfile`

```Dockerfile
FROM node:9.4.0-alpine

RUN mkdir -p /usr/src/app

WORKDIR /usr/src/app

COPY package.json /usr/src/app/

RUN npm i --registry=https://registry.npm.taobao.org

COPY . /usr/src/app

EXPOSE 7001

CMD npm run docker
```

+ 第四部，发布项目

```shell
# 打包镜像
docker build -t yipage/padipata .

# 运行镜像
docker run -d -p 7001:7001 yipage/padipata

# 查看运行日志
docker ps
docker logs {CONTAINER ID}

# 停止服务
docker ps
docker stop {CONTAINER ID}

# 删除镜像
docker ps -a
docker rm {CONTAINER ID}
docker rmi {IMAGE ID}
```





