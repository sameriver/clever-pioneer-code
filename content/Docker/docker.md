# 简介
## 特性
在不依赖**任何语言、框架或者包装系统**，使用**沙盒机制**，让开发者打包应用以及依赖包到可抑制的容器中，然后发布到任何流行的Linux机器上，也可实现虚拟化
## 官网
[docker官网][https://www.docker.com]
[docker中文库][https://www.docker.org.cn/]
## 与虚拟机技术的区别
- 相同点：
	- 都是虚拟化技术
- 不同点：
	- docker启动速度快
	- 容器间共用一个系统内核
## 镜像/容器
**镜像**：存储在**Docker Hub**上的资源
**容器**：安装出来的软件 用于实际使用
## 实际部署
### Linux安装docker
[docker安装命令][get.docker.com]
执行1&4步命令即可
### Window安装docker

## 常用命令
### docker pull
> 拉取镜像

- --platform= 架构（可选:默认选择适合）
- repository
	- registry注册表/地址：docker.io
	- namespace命名空间：library
	- 项目名：nginx
- 标签（版本号）：nginx:latest
### docker image 
> 展示所有已下载镜像

### docker run [-d] xxx
>运行某一镜像 创建容器

- -d：用于分离日志
- -p：设置端口映射 （例：80：80）宿主机80端口映射到容器80端口
- -v
	- **绑定挂载**：绑定容器和宿主机目录（挂载卷）删除容器时，数据在宿主机中留存 （例：宿主机路径：容器路径）
	- **命名卷挂载**：创建存储空间（命名卷）（例：卷的名字：容器内目录）
- -e:传递环境变量(docker hub上查找化境变量)
	- -e XXX = value
- --name：给容器命名（保证在宿主机上是唯一的）
- -it:控制台进入容器进行交互
-  --rm: 容器停止时将自动删除
- --restart always/unless-stopped 停止时自动重启/手动停止时不会重启
- -- networke (子网名称) ：加入子网中运行
-  --networke host：使用Host模式进行 占用宿主机的Ip与端口
### docker create
> 仅创建 不立即启动
> 与docker run命令
### docker volumn
#### docker volume create （名称）
>创建命名卷

####  docker volume inspect (名称)
> 展示命名卷信息

#### docker volume rm  （名称）
> 删除命名卷

#### docker volumn prune -a
> 删除所有未在使用的命名卷

#### dockers volume list
> 展示所有命名卷
### docker rm -f（Id）
> 删除指定容器

- -f：强制删除
### docker rmi  -f  (Id)
> 删除指定镜像
#### docker ps
>展示process status 即容器信息
- 默认展示运行中容器
- a:展示所有容器
### docker start （Id/名称）
> 启动容器（保存历史配置信息）
### docker stop （Id/名称）
>停止容器运行
### docker inspect （Id）
> 展示容器具体信息
### docker logs （Id/名称）
> 查看容器日志
- -f :滚动查看日志
### docker exec （Id/名称）（linux命令）
>容器内执行linux命令

### docker exec -it （Id） /bin/sh
> 进入容器中获得交互界面

### docker build -t (名称):(版本号) .
- 版本号可省略不写
- . 表示在当前目录下构建
### docker login
> 登录docker 进入网站输入验证码即可
### docker push （namespace/用户名）/（名称）
> 推送镜像


### docker network create （子网名称）
> 创建子网
### docker network list
>展示所有子网信息
### docker network rm (子网Id)
>删除子网
### docker compose
>**使用yml文件**将多个docker容器联系
>理解为多个docker run命令 按照特定格式列到一个文件
>同一个compose文件自动加入同一个子网
>**depends_on**将调整运行顺序
>![[Pasted image 20250803114212.png]]
####  vi docker-compose.yaml 
> 创建dockercompose文件
#### docker compose up -d  
>运行compose容器文件
- f：指定compose文件路径

#### docker compose down 
> 停止并删除容器

#### docker compose stop
> 停止但不删除容器

#### docker compose start
> 启动容器


## Dockerfile
```
FROM (基础镜像)
//基础镜像
WORKDIR （工作目录）
//指定镜像目录
COPY ..
//将代码文件靠被动工作目录 **.**:当前目录 **.**:镜像内当前工作目录
RUN PIP INSTALL xxx 
//安装依赖
EXPOSE 8080
//服务端口
CMD ["python3","main.py"]
ENTRYPOINT ["python3","main.py"] 
//容器运行时的默认启动命令
//ENTRYPOINT 优先级更高 不容易被覆盖
```

## 原理
### Cgroups
### Namespace
### Docker 网络
> 默认Bridge桥接模式
- 每个容器分配内部ip地址
- 同一子网
	- 容器可以互相通信
	- 使用名称互相访问 不需要使用内部ip地址
	- 子网内部存在 DNS机制
	- ![[Pasted image 20250803113048.png]]
- Host模式
	- Docker容器直接共享宿主机的网络
	- 容器使用宿主机的Ip地址 无需-p参数进行端口映射
	- 直接运行在宿主机的端口
- none模式
	- 不联网
## K8S