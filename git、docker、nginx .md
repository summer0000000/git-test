## Docker 

### 1.熟悉docker

删除dokcer

```
sudo yum remove docker \
docker-client \
docker-client-latest \
docker-common \
docker-latest \
docker-latest-logrotate \
docker-logrotate \
docker-engine  卸载 Docker 引擎
----------------------------------
sudo rm -rf /var/lib/docker  .删除 Docker 数据目录
---------------------------------
yum list installed | grep docker 查看是否有漏掉的docker依赖

```

docker images -- docker镜像

tee /etc/docker/daemon.json <<-'EOF' {  "registry-mirrors": ["https://jhcckxcy.mirror.aliyuncs.com"] } EOF  --docker 下载资源镜像源

> 利用docker快速安装mysql
>
> ```
> docker run -d \       
>   --name mysql \
>   -p 3306:3306 \
>   -e TZ=Asia/Shanghai \
>   -e MYSQL_ROOT_PASSWORD=123 \
>   mysql
> ```
>
> + -d：让容器在后台运行 
> + --name 给容器命名
> + -e：环境变量
> + -p：宿主机端口映射到容器内的端口
>
> 镜像名称结构：
>
> Respositorty（镜像名） ： TAG（版本）

### 2.docker基础

#### 2.1常见命令

+ docker search 命令来查找官方仓库中的镜像

+ docker build : 就是构建一个docker镜像

  + docker build -t (镜像名字)：(镜像版本)  (Dockerfile目录）

+ docker pull  拉取镜像

+ docker push  推送镜像到镜像仓库

+ docker images 查看镜像

+ docker rmi 删除镜像

+ docker run  创建并运行容器

+ docker stop 停止容器

+ docker start 启动容器

+ docker restart 重新启动容器

+ docker rm 删除容器  条件：容器处于停止状态可以删除

  + docker rm （容器名字） -f  强制删除容器

+ docker ps 查看运行中容器

  + docker ps --format "table {{.ID}}\t{{.Image}}\t{{.Ports}}\t{{.Status}}\t{{.Names}}" 规定   设置命名别名在liunx中的~/.bashrc 中添加 alias dps=‘docker ps --format "table {{.ID}}\t{{.Image}}\t{{.Ports}}\t{{.Status}}\t{{.Names}}" ’ 后保存 source ~/.bashrc 生效
  + docker ps -a 查看所有容器

+ docker logs 查看日志

  + docker logs -f 持续查看日志

+ docker exec 进入容器

  + docker exec -it （容器名） bash   以bash方式进入容器进行执行

+ docker save 保存镜像到本地压缩文件

  + docker sava -o （镜像名）

+ docker load 加载本地压缩文件到镜像

  + docker load -i (文件名)

+ docker inspect xxx 查看容器详情

  + ###### Mount 挂在数据

#### 2.2数据卷

数据卷（volume）是一个虚拟目录，是容器内目录与宿主机目录之间映射的桥梁。

> + 在执行dockerrun命令时，使用-**v数据卷：容器内目录**可以完成数据卷挂载
>
> + 当创建容器时，如果挂裁了数据卷且数据卷不存在，会自动创建数据卷
>
> + -v 容器内路径 #匿名挂载
>
>   -v 卷名:容器内路径 #具名挂载
>
>   -v /宿主机路径:容器内路径 #指定路径挂载

+ docker volume create 创建数据卷
+ docker volume ls 查看说有数据卷
+ docker volume rm 删除数据卷
  + docker rm -v  删除容器之时删除相关的卷
+ docker volume inspect xxx  查看某一个数据卷
+ docker volume prune 清除数据卷

挂载本地目录或文件

数据卷的目录结构较深，如果我们去操作数据卷目录会不太方便。在很多情况下，我们会直接将容器目录与宿主机指定目录挂载。挂载语法与数据卷类似：

```
# 挂载本地目录 -v 本地目录:容器内目录 # 挂载本地文件 -v 本地文件:容器内文件
```

**注意**：本地目录或文件必须以 `/` 或 `./`开头，如果直接以名字开头，会被识别为数据卷名而非本地目录名。

案例：

```
docker run -d \
  --name mysql \
  -p 3306:3306 \
  -e TZ=Asia/Shanghai \
  -e MYSQL_ROOT_PASSWORD=123 \
  -v ./mysql/data:/var/lib/mysql \
  -v ./mysql/conf:/etc/mysql/conf.d \
  -v ./mysql/init:/docker-entrypoint-initdb.d \
  mysql
```

#### 2.3自定义镜像

**Dockerfile**：

由于制作镜像的过程中，需要逐层处理和打包，比较复杂，所以Docker就提供了自动打包镜像的功能。我们只需要将打包的过程，每一层要做的事情用固定的语法写下来，交给Docker去执行即可。

而这种记录镜像结构的文件就称为**Dockerfile**，其对应的语法可以参考官方文档

其中的语法比较多，比较常用的有：

| **指令**       | **说明**                                     | **示例**                     |
| :------------- | :------------------------------------------- | :--------------------------- |
| **FROM**       | 指定基础镜像                                 | `FROM centos:6`              |
| **ENV**        | 设置环境变量，可在后面指令使用               | `ENV key value`              |
| **COPY**       | 拷贝本地文件到镜像的指定目录                 | `COPY ./xx.jar /tmp/app.jar` |
| **RUN**        | 执行Linux的shell命令，一般是安装过程的命令   | `RUN yum install gcc`        |
| **EXPOSE**     | 指定容器运行时监听的端口，是给镜像使用者看的 | EXPOSE 8080                  |
| **ENTRYPOINT** | 镜像中应用的启动命令，容器运行时调用         | ENTRYPOINT java -jar xx.jar  |

例如，要基于Ubuntu镜像来构建一个Java应用，其Dockerfile内容如下：

```
# 指定基础镜像
FROM ubuntu:16.04
# 配置环境变量，JDK的安装目录、容器内时区
ENV JAVA_DIR=/usr/local
ENV TZ=Asia/Shanghai
# 拷贝jdk和java项目的包
COPY ./jdk8.tar.gz $JAVA_DIR/
COPY ./docker-demo.jar /tmp/app.jar
# 设定时区
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
# 安装JDK
RUN cd $JAVA_DIR \
&& tar -xf ./jdk8.tar.gz \
&& mv ./jdk1.8.0_144 ./java8
# 配置环境变量
ENV JAVA_HOME=$JAVA_DIR/java8
ENV PATH=$PATH:$JAVA_HOME/bin
# 指定项目监听的端口
EXPOSE 8080
# 入口，java项目的启动命令
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

就有人提供了基础的系统加JDK环境，我们在此基础上制作java镜像，就可以省去JDK的配置了：

```
# 基础镜像
FROM openjdk:11.0-jre-buster
# 设定时区
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
# 拷贝jar包
COPY docker-demo.jar /app.jar
# 入口
ENTRYPOINT ["java", "-jar", "/app.jar"]
```

#### 2.4网路

+ 默认情况下，所有容器都是以bridge方式连接到Docker的一个虚拟网桥上，所有可以相互访问，但是网络可能波动被占用，导致出现在不同网关上，不能互相访问。

加入自定义网络的容器才可以通过容器名互相访问，Docker的网络操作命令如下：

+ docker network create （网络名）：创建一个网路
+ docker network ls ：查看说有网络
+ docker network rm ： 删除指定网络
+ docker network prune：清除未使用的网络
+ docker network connent （网络名）（容器名）：使指定容器连接加入某网络
  + docker run -d --name dd -p 8080:8080 --network heima docker-demo 直接在创建容器时添加网络
+ docker network disconnect （网络名）（容器名）：使指定容器连接离开某网络
+ docker network inspect : 查看网络详细信息

### 3.部署应用

+ 配置环境
  + 创建一个网路容器，将mysql和jar包（包含Dockerfile）和jdk配置好镜像。 jar包需要自定义镜像 

+ 部署后端
  + 通过docker运行jdk，mysql、jar的镜像。
  + **注：后端和mysql必须在一个网络上面才可以访问**
+ 部署前端 （重点：nginx.conf配置文件）
  + 配置好nginx.conf文件
  + 运行nginx镜像

```
mysql的docker运行案例：
docker run -d \
  --name mysql \
  -p 3306:3306 \
  -e TZ=Asia/Shanghai \
  -e MYSQL_ROOT_PASSWORD=123 \
  -v ./mysql/data:/var/lib/mysql \ -
  -v ./mysql/conf:/etc/mysql/conf.d \
  -v ./mysql/init:/docker-entrypoint-initdb.d \
  -network hmall \
  mysql
  
nginx的docker运行案例：
docker run -d \
  --name nginx \
  -p 18080:18080 \  # 是nginx中代理端口号
  -p 18081:18081 \  # 是nginx中代理端口号
  -v /root/nginx/html:/usr/share/nginx/html \ # 是nginx的html页面显示地址
  -v /root/nginx/nginx.conf:/etc/nginx/nginx.conf \ #是nginx的conf配置路径
  --network hmall \ # 是保证在同一网络,具体看conf中设置的,可有可无。
  nginx
```

### 4.DockerCompose

Docker Compose就可以帮助我们实现**多个相互关联的Docker容器的快速部署**。它允许用户通过一个单独的 docker-compose.yml 模板文件（YAML 格式）来定义一组相关联的应用容器。

#### 4.1 基本语法

docker-compose.yml文件的基本语法可以参考官方文档：https://docs.docker.com/compose/compose-file/compose-file-v3/

| **docker run 参数** | **docker compose 指令** | **说明**   |
| :------------------ | :---------------------- | :--------- |
| --name              | container_name          | 容器名称   |
| -p                  | ports                   | 端口映射   |
| -e                  | environment             | 环境变量   |
| -v                  | volumes                 | 数据卷配置 |
| --network           | networks                | 网络       |

> 对比
>
> ```
> docker run -d \
>   --name mysql \
>   -p 3306:3306 \
>   -e TZ=Asia/Shanghai \
>   -e MYSQL_ROOT_PASSWORD=123 \
>   -v ./mysql/data:/var/lib/mysql \
>   -v ./mysql/conf:/etc/mysql/conf.d \
>   -v ./mysql/init:/docker-entrypoint-initdb.d \
>   --network hmall
>   mysql
> ```
>
> <hr/>
>
> ```
> version: "3.8"  # 项目
> 
> services: # 服务
>   mysql: # 服务A
>     image: mysql
>     container_name: mysql
>     ports:
>       - "3306:3306"
>     environment:
>       TZ: Asia/Shanghai
>       MYSQL_ROOT_PASSWORD: 123
>     volumes:
>       - "./mysql/conf:/etc/mysql/conf.d"
>       - "./mysql/data:/var/lib/mysql"
>     networks:
>       - new
> networks:
>   new:
>     name: hmall
> ```



> 项目部署dockerCompose
>
> ```
> services:
>   mysql:
>     image: mysql # 镜像
>     container_name: mysql # 名字
>     ports: # 端口
>       - "3306:3306"
>     environment: # 环境
>       TZ: Asia/Shanghai
>       MYSQL_ROOT_PASSWORD: 123
>     volumes: # 挂载
>       - "./mysql/conf:/etc/mysql/conf.d"
>       - "./mysql/data:/var/lib/mysql"
>       - "./mysql/init:/docker-entrypoint-initdb.d"
>     networks: # 网路
>       - hm-net
>   demo: 
>     build: #构建-> 在当前地址构建
>       context: .
>       dockerfile: Dockerfile # 指定文件
>     container_name: demos
>     ports:
>       - "8080:8080"
>     networks:
>       - hm-net
>     depends_on: # 依赖
>       - mysql
>   nginx:
>     image: nginx
>     container_name: nginx
>     ports:
>       - "18080:18080"
>       - "18081:18081"
>     volumes:
>       - "./nginx/nginx.conf:/etc/nginx/nginx.conf"
>       - "./nginx/html:/usr/share/nginx/html"
>     depends_on:
>       - demo
>     networks:
>       - hm-net
> networks: # 创建网络
>   hm-net:
>     name: hmall
> ```

docker compos的命名格式

+ docker compose [OPTIONS] [COMMAND] 
+ docker conpose ps 查看镜像
+ docker conpose down 移除

其中，OPTIONS和COMMAND都是可选参数，比较常见的有：

| **类型** | **参数或指令** | **说明**                                                     |
| :------- | :------------- | :----------------------------------------------------------- |
| Options  | -f             | 指定compose文件的路径和名称                                  |
|          | -p             | 指定project名称。project就是当前compose文件中设置的多个service的集合，是逻辑概念 |
| Commands | up             | 创建并启动所有service容器                                    |
|          | down           | 停止并移除所有容器、网络                                     |
|          | ps             | 列出所有启动的容器                                           |
|          | logs           | 查看指定容器的日志                                           |
|          | stop           | 停止容器                                                     |
|          | start          | 启动容器                                                     |
|          | restart        | 重启容器                                                     |
|          | top            | 查看运行的进程                                               |
|          | exec           | 在指定的运行中容器中执行命令                                 |



<hr/>
<Hr/>





## git 

生成密钥 ssh-keygen -t rsa -b xxx   

```
# 示例代码：生成SSH密钥对
ssh-keygen -t rsa -b 4096 -f ~/.ssh/my_key
# 生成的文件
id_rsa文件是私钥，要保存好，放在本地，私钥可以生产公钥，反之不行。
id_rsa.pub文件是公钥，可以用于发送到其他服务器，或者git上
```

端口22不能使用

```
命令测试 SSH 连接
ssh -T git@github.com
修改端口
ssh -T -p 443 git@ssh.github.com
```

工作区   —  暂存区 — 本地仓库 

### 1.git 基本命令

- **git init**：初始化一个git仓库  
- git init xxx： 指定初始化一个git仓库
- **git clone** <url>  [directory]： 拉取一个git仓库   url为git仓库地址，directory为本地目录
  - git clone -b <分支名称> 拉取分支

- **git config**：配置信息
- **git add**：添加文件到缓存命令
  - git add .  将当前目录下的文件添加到暂停区
- **git status**：查看文件的状态命令

- **git diff**：查看更新的详细信息命令
  - git diff --cached/staged 比较暂停区和本地仓库数据
- **git commit**：提交命令
  - git commit -m "日志" 
  - git commit -am  "日志"
- **git reset HEAD**：取消缓存命令
  - git reset --soft 保留工作区和缓冲区的文件
  - git reset --hard 不保留工作区和缓冲区的文件
  - git reset --mixed 保留工作区文件，不保留缓冲区的文件
- **git rm**：删除命令
  - git rm --cached <file>  文件从暂存区域移除 工作区中存在
  - git rm -r * 递归删除
- **git mv**：移动或重命名命令
- **git ls-files** ：查看暂停区中的文件

### 2.git分支管理

+ **git branch**：查看分支命令
+ **git branch (branchname)**：创建分支命令
+ **git checkout / [推荐]switch (branchname)**：切换分支命令
+ **git merge/rebase**xxx：合并分支命令  -- 将任意分支合并到到当前分支
  + git merge xxx ：不会破坏原分支的提交历史。
  + git rebase xxx ：会破会原分支。
+ **git branch -d (branchname)**：删除合并分支命令
+ **git branch -D (branchname)**：未合并分支命令
+ **git branch --set-upstream-to=`remote_name` `local_name`**  分支关联 便于拉取文件  
  + `remote_name` ： 远程分支名 // 这里是你创建的分支需要和已有的那个分支进行关联的名称
    `local_name` ： 本地分支 // 你当前创建的本地分支名称
+ git merge --abort 中止合并

### 3.git查看提交历史

+ git log 查看
  + **––oneline** ：查看历史记录的简洁版本
  + **–reverse** ：逆向显示所有日志
  + **––author** ：查找指定用户的提交日志
  + **–since、–before、 --until、–after**： 指定筛选日期
  + **–no-merges** ：选项以隐藏合并提交
+ **git reflog**
+ **git log --oneline --graph --decorate --all** 查看出现的分支  
  + 用alias graph=”git log --oneline --graph --decorate --all“ 创建快捷命令

### 4.Git 远程仓库

+ **git remote add**  [alias] [url]：添加远程仓库  -参数[alias]为别名， [url]为远程仓库的地址
+ **git remote**  /-v：查看当前的远程仓库
+ **git fetch**、**git pull**：提取远程仓仓库
  + git pull <远程仓库名>  <远程分支名> : <本地分支名>
+ **git push** [alias] [branch]：推送到远程仓库
+ **git remote rm**：删除远程仓库

### 5.忽略文件 .gitignore

+ 空行或者以#开头的行会被Git忽略。一般空行用于可读性的分隔，#一般用作注释
+ 使用标准的Blob模式匹配，例如：
  + 星号*通配任意个字符
  + 问号？匹配单个字符
  + 中括号[]表示匹配列表中的单个字符，比如：[abc]表示a/b/c
+ 两个星号**表示匹配任意的中间目录
+ 中括号可以使用短中线连接，比如：[0-9]表示任意一位数字，[a-z]表示任意一位小写字母
+ 感叹号！表示取反

<hr/>
<hr/>



## Nginx 

反向代理、权重轮询、iphash

### 1.nginx命令

配置文件：~/nginx/conf/nginx.conf

数据：~/nginx/html

日志文件：~/nginx/log

+ nginx -s stop 停止nginx
  + nginx -s quit 完整有序的停止nginx
  + taskkill /f /t /im nginx.exe 杀死进程的

+ nginx -s reload  重新加载配置文件

**在liunx下的常用命令**

1. `./nginx  启动`
2. `./nginx -s stop  停止`
3. `./nginx -s quit  安全退出`
4. `./nginx -s reload  重新加载配置文件`
5. `ps aux|grep nginx  查看nginx进程`

注意：如何连接不上，检查阿里云安全组是否开放端口，或者服务器防火墙是否开放端口！

```
# 开启
service firewalld start
# 重启
service firewalld restart
# 关闭
service firewalld stop
# 查看防火墙规则
firewall-cmd --list-all
# 查询端口是否开放
firewall-cmd --query-port=8080/tcp
# 开放80端口
firewall-cmd --permanent --add-port=80/tcp
# 移除端口
firewall-cmd --permanent --remove-port=8080/tcp
#重启防火墙(修改配置后要重启防火墙)
firewall-cmd --reload
# 参数解释
1、firwall-cmd：是Linux提供的操作firewall的一个工具；
2、--permanent：表示设置为持久；
3、--add-port：标识添加的端口；
```

### 2.nginx配置解析

文件地址：~/nginx/conf/nginx.conf

> 配置文件分为：**全局块** 、**events块**、**http块**
>
> ```
> # 全局块
> worker_processes  1;
> #events块
> events {
>     worker_connections  1024;
> }
> #http块
> http {
>     server {
>         listen       1880;
>         location / {
>             root   html; ->配置文件同一列html文件，一般为静态地址：/usr/share/nignx/html
>             index  index.html index.htm;
>         }
>         error_page   500 502 503 504  /50x.html;
>         location = /50x.html {
>             root   html;
>         }  
>     }
> }
> ```

### 3.反向代理配置

proxy_pass 代理

> 简单配置
>
> ```conf
> server {
>     listen       80;
>     server_name  www.zhengqing520.com;# 服务器地址或绑定域名
>     location / { # 访问80端口后的所有路径都转发到 proxy_pass 配置的ip中
>         root   /usr/share/nginx/html;
>         index  index.html index.htm;
>    		proxy_pass http://192.168.40.130:80; # 配置反向代理的ip地址和端口号 【注：url地址需加上http:// 或 https://】
>     }
> }
> ```
>
> 复杂配置
>
> ```
> server {
>     listen       80;
>     server_name  www.zhengqing520.com;# 服务器地址或绑定域名
>     location ^~ /api {  # ^~/api 表示匹配前缀为api的请求
>         proxy_pass  http://www.zhengqing520.com:9528/api/;  # 注：proxy_pass的结尾有/， -> 效果：会在请求时将/api/*后面的路径直接拼接到后面
>         # proxy_set_header作用：设置发送到后端服务器(上面proxy_pass)的请求头值  
>             # 【当Host设置为 $http_host 时，则不改变请求头的值;
>             #   当Host设置为 $proxy_host 时，则会重新设置请求头中的Host信息;
>             #   当为$host变量时，它的值在请求包含Host请求头时为Host字段的值，在请求未携带Host请求头时为虚拟主机的主域名;
>             #   当为$host:$proxy_port时，即携带端口发送 ex: $host:8080 】
>         proxy_set_header Host $host; 
>         proxy_set_header X-Real-IP $remote_addr; # 在web服务器端获得用户的真实ip 需配置条件①    【 $remote_addr值 = 用户ip 】
>         proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;# 在web服务器端获得用户的真实ip 需配置条件②
>         proxy_set_header REMOTE-HOST $remote_addr;
>         # proxy_set_header X-Forwarded-For $http_x_forwarded_for; # $http_x_forwarded_for变量 = X-Forwarded-For变量
>     }
> 
>     location ^~ /blog/ { # ^~/blog/ 表示匹配前缀为blog/后的请求
>         proxy_pass  http://zhengqingya.gitee.io/blog/; 
>         proxy_set_header Host $proxy_host; # 改变请求头值 -> 转发到码云才会成功
>         proxy_set_header  X-Real-IP  $remote_addr;
>         proxy_set_header  X-Forwarded-For $proxy_add_x_forwarded_for;
>         proxy_set_header X-NginX-Proxy true;
>     }
> }
> ```

### 4.权重轮询

```
worker_processes  1;
events {
    worker_connections  1024;
}
http {
	upstream demo{
		server 192.168.40.130:81 weight=1;
		server 192.168.40.130:82 weight=4;
	}
    server {
        listen       80;
        location / {
            root   html;
            index  index.html index.htm;
			proxy_pass http://demo;
        }
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

    }

}
```

+ 其中 upstream demo中的weight设置的值为权重

+ 值越高访问次数越多，值相同访问次数一样。

### 5.iphash

添加关键字：**ip_hash**（iphash） 和  **hash $requesturi**（urlhash）

> ```
> upstream demo{
> 		ip_hash;
> 		server 192.168.40.130:81 weight=1 max_fails=3 fail_timeout=30;
> 		server 192.168.40.130:82 weight=4;
> 	}
> ------------------------------------------------
> upstream demo{
> 		hash $requesturi;
> 		server 192.168.40.130:81 weight=1;
> 		server 192.168.40.130:82 weight=4;
> 	}
> ```
>
> 其中：
>
> max_fails=3代表这表示如果在  `fail_timeout` 时间内，连续 3 次对 `backend1.example.com` 的请求失败，那么 Nginx 将不再将请求发送给该服务器。
>
> 如果某个服务器在 `fail_timeout` 时间内达到了 `max_fails` 限制，Nginx 会在 `fail_timeout` 时间内不再向该服务器发送请求。





