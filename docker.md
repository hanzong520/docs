# Docker

## 官网

https://www.docker.com/

## 介绍一波

Docker是google公司用go语言开发出来的，没有什么技术含量，用linux底层的东西做出来了，但是想法好，能理解Linux的本质，把精华提取出来，但是Docker之父已经走了，失去了灵魂，更新进度放慢了，想当年，每天能更新两次。

> 对比传统虚拟机：相对于虚拟机，少了一层虚拟系统，不用装虚拟机，可以直接在主机上（宿主机）寄生，相当于在主机上装了一个应用

> 高效利用系统资源：因为不需要硬件分配，节省了资源，可以运行更多的应用

> 更快的启动时间：由于直接运行在主机上（宿主机），无需启动完整的操作系统，所以可以秒级，甚至做到毫秒级启动

> 一致的运行环境：原来开发、测试、线上环境不一致，导致程序员甩锅（这段代码在我机器上运行没问题啊），现在Docker做到了一致

> 持续交付和部署：真正实现一次编译，到处运行（在一个Docker可以运行，在任何Docker都可以），可以实现CI/CD（持续集成/持续部署）

> 更轻松的交付和迁移：克隆打包迁移

## Docker架构

> 架构：CS架构

> daemon 守护进程，程序崩溃之后自动重启
>
> REST API 提供REST的API
>
> Docker CLI 可以通过命令行通过REST风格的形式访问服务器

``` shell
docker run -p 8080:8080 tomcat
```

容器是镜像的实例，就像实体类和实体类对象的关系，容器可以随意修改，即使坏了之后也可以通过镜像重新实例化一个容器 

Registry （仓库）

Images（镜像）

Containers（容器）

docker pull 命令执行过程，从仓库下载镜像到本地镜像

docker run 本地镜像进行实例化，生成容器



## Docker安装

1、使用 `root` 权限登录 Centos。确保 yum 包更新到最新。

```
$ sudo yum update
```

2、卸载旧版本(如果安装过旧版本的话)

```
$ sudo yum remove docker  docker-common docker-selinux docker-engine
```

4、安装需要的软件包， yum-util 提供yum-config-manager功能，另外两个是devicemapper驱动依赖的

```
  $ sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

5、设置yum源

```
$ sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

6、可以查看所有仓库中所有docker版本，并选择特定版本安装

```
$ yum list docker-ce --showduplicates | sort -r
```

7、安装docker

```
$ sudo yum install docker-ce  #由于repo中默认只开启stable仓库，故这里安装的是最新稳定版17.12.0
$ sudo yum install <FQPN>  # 例如：sudo yum install docker-ce-17.12.0.ce
```

8、启动并加入开机启动

```
$ sudo systemctl start docker
$ sudo systemctl enable docker
```

9、验证安装是否成功(有client和service两部分表示docker安装启动都成功了)

```
$ docker version
```

10、配置Docker镜像加速

* 阿里云登录
* 搜索容器镜像服务
* 镜像加速器
* 形成专属的镜像加速地址  https://uwx3m98o.mirror.aliyuncs.com

2、配置

运行命令（注意修改镜像加速地址）：

```
mkdir -p /etc/docker
tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://uwx3m98o.mirror.aliyuncs.com"]
}
EOF
systemctl daemon-reload
systemctl restart docker
```

3、查看是否启用

``` 
$ docker info
```

## Docker 命令

下载镜像

``` shell
$ docker pull tomcat
```

查看镜像

``` shell
$ docker images
```

删除镜像

``` shell
$ docker rmi images
```



启动程序

> 第一个8080是宿主机端口号，第二个是容器的端口号

```shell
$ docker run -p 8080:8080 tomcat
```

守护运行

``` shell
$ docker run -p 8080:8080 -d tomcat
```



查看正在运行的程序

```shell
$ docker ps 
						 -a 所有的，包括未启动的
```

停止程序

```shell
$ docker stop <容器ID>
```





修改docker中tomcat 的配置

``` shell
使用命令: docker exec -it 运行的tomcat容器ID /bin/bash 进入到tomcat的目录

docker exec -it 容器ID /bin/bash
进入到tomcat目录，

#查看目录
ls -l

```

退出docker

``` powershell
exit
```



指定工作目录

相当于CD，但是会记忆工作目录，下次进入docker会自动跳转到WORKDIR目录下

``` shell
WORDDIR 
```







## 定制镜像Dockerfile

> 一堆安装部署命令的集合



``` shell
[root@iZ2zebc0dctm6mo0zm3v85Z myshop]# ls
Dockerfile  index.jsp

[root@iZ2zebc0dctm6mo0zm3v85Z myshop]# echo hello word index.jsp
hello word index.jsp
[root@iZ2zebc0dctm6mo0zm3v85Z myshop]# ls
Dockerfile
[root@iZ2zebc0dctm6mo0zm3v85Z myshop]# vi index.jsp
[root@iZ2zebc0dctm6mo0zm3v85Z myshop]# ls
Dockerfile  index.jsp
[root@iZ2zebc0dctm6mo0zm3v85Z myshop]# vi Dockerfile 

# Dockerfile中的内容

FROM tomcat:latest
COPY index.jsp /usr/local/tomcat/webapps/ROOT

# 构建
docker build -t myshop .

docker images
docker run -p 9080:9080 --name myshop -d myshop
```

## Docker Hub 帐号

ID: orange2020

邮箱：18661960960@163.com

密码：orange2020.

## Docker Compose

> 简化docker的启动停止流程，编排docker启动服务与服务之间的关系

## docker-compose安装

```shell
curl -L https://get.daocloud.io/docker/compose/releases/download/1.25.1/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose

# 验证是否安装成功
sh-3.2# ./docker-compose version
docker-compose version 1.25.1, build a82fef07
docker-py version: 4.1.0
CPython version: 3.7.4
OpenSSL version: OpenSSL 1.1.1c  28 May 2019
sh-3.2#
```

## docker-compose 使用

> 编写vi docker-compose.yaml文件

``` shell
root@iZ2zebc0dctm6mo0zm3v85Z tomcat]# vi docker-compose.yaml
```

> 文件内容如下：

``` shell
version: '3.1'
service:
  tomcat: 
    restart:always
    image: tomcat
    container_name: tomcat
    ports:
       - 9090:8080
```

``` shell

[root@iZ2zebc0dctm6mo0zm3v85Z tomcat]# vi ./docker-compose.yaml 

# 启动tomcat-使用docker-compose
[root@iZ2zebc0dctm6mo0zm3v85Z tomcat]# docker-compose up
Recreating tomcat ... done
Attaching to tomcat
tomcat    | NOTE: Picked up JDK_JAVA_OPTIONS:  --add-opens=java.base/java.lang=ALL-UNNAMED --add-opens=java.base/java.io=ALL-UNNAMED --add-opens=java.rmi/sun.rmi.transport=ALL-UNNAMED
```

> 下线

``` shell
docker-compose down
```

