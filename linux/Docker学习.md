# Docker学习

## 1.前戏

你最喜欢的前戏部分;

- 什么是docker?

  ```
  docker将应用程序与程序的依赖，打包在一个文件里面。运行这个文件就会生成一个虚拟容器。
  程序运行在虚拟容器里，如同在真实物理机上运行一样，有了docker，就不用担心环境问题了。
  ```

- 应用场景

  ```
  web应用的自动化打包和发布
  自动化测试和持续集成、发布
  在服务型环境中部署和调整数据库或其他应用
  ```

- docker与虚拟机的区别

  - 物理机

    ![1569064252132](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1569064252132.png)

  - 虚拟机

    ![1569064268685](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1569064268685.png)

  - 容器

    ![1569064287414](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1569064287414.png)

- docker的三大概念镜像,容器,仓库

  ```python
  #镜像
  镜像文件,可以理解为操作系统的centos.iso文件;
  例如：一个镜像可以包含一个完整的CentOS操作系统环境，里面仅安装了Apache或用户需要的其他应用程序。
  镜像可以用来创建Docker容器。
  Docker提供了一个很简单的机制来创建镜像或者更新现有的镜像，用户甚至可以直接从其他人那里下载一个已经做好的镜像来直接使用。
  #容器
  容器实例,基于docker镜像运行出的容器实例,可以理解为微型操作系统
  #仓库
  仓库, 存储镜像文件的地方
  ```

  

## 2.docker与虚拟机的对比

| 特性       | 容器               | 虚拟机     |
| ---------- | ------------------ | ---------- |
| 启动       | 秒级               | 分钟级     |
| 硬盘使用   | 一般为 MB          | 一般为 GB  |
| 性能       | 接近原生           | 弱         |
| 系统支持量 | 单机支持上千个容器 | 一般几十个 |

- **Linux容器不是模拟一个完整的操作系统，而是对进程进行隔离。在正常进程的外面套了一个保护层，对于容器里面进程来说，它接触的资源都是虚拟的，从而实现和底层系统的隔离。**

- **镜像作为标准的交付件，可在开发、测试和生产环境上以容器来运行，最终实现三套环境上的应用以及运行所依赖内容的完全一致。**

![1569064447792](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1569064447792.png)

- 那么你肯定看出了**docker的优势**:

  ```python
  1.更高效的利用系统资源
  2.更快速的启动时间
  3.一致的运行环境
  4.持续交付和部署
  5.更轻松的迁移
  ```

- 工作中的虚拟化加容器

  ![1569064587804](C:\Users\big cattle\AppData\Roaming\Typora\typora-user-images\1569064587804.png)

## 3.docker的安装使用

### 3.1安装docker

```python
1.yum安装,配置yum仓库
-阿里云仓库,清华源仓库,163仓库, 问题是,docker的版本可能很低,有很多漏洞
-选择软件官方提供的yum仓库,版本都是最新的,但是可能下载较慢
-由于网速问题,学习阶段还是使用阿里云的docker
2.rpm 不推荐
3.源码编译安装,很麻烦,如果没有特定需求,还是选择yum正确姿势:
```

- 那么下面就是用正确姿势,安装docker

```python
#1.卸载旧版本
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine

#2.设置存储库
sudo yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2

sudo yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo

#3.安装docker社区版
sudo yum install docker-ce
#4.启动关闭docker
systemctl start docker
// systemctl status/start/stop docker  #看状态/启动/停止
```

**ps:docker最低支持centos7且在64位平台上，内核版本在3.10以上**

### 3.2docker基础命令

- 加速配置

  ```python
  #一条命令加速
  curl -sSL https://get.daocloud.io/daotools/set_mirror.sh | sh -s http://95822026.m.daocloud.io
  ```

- 增, 获取镜像文件 ,centos或者ubuntu 系统等等镜像

  - docker pull  镜像文件名   		#下载docker镜像 

  ```python
  docker  run  hello-world   #运行镜像文件,生成docker容器实例,docker run命令,会自动下载不存在的镜像
  #容器是随时创建,随时删除的,轻量级,每次docker run 都会生成新的容器记录 
  #docker容器进程,如果没有在后台运行的话,就会立即挂掉,  (容器中必须有正在工作的进程)
  
  #运行一个活着的容器进程
  docker run -d centos /bin/sh -c "while true;do echo '你个糟老头子,不听课,坏得很'; sleep 1;done"
  	#运行镜像  
  	-d  后台运行的意思
  	 centos  指的是镜像文件名  
  	/bin/sh  要在这个容器内运行的命令,指定的解释器 shell解释器 
  	-c  指定一段shell代码 
  	
  #进入容器空间内的命令 
  docker exec -it 容器id  /bin/bash  #进入容器空间内 
  	-i 交互式命令操作
  	-t 开启一个新的终端
      exit 退出虚拟容器
  	
  #运行一个ubuntu容器
  docker run -it ubuntu  
  ```

- 删

  - 查看容器运行// systemctl status/start/stop docker  #看状态/启动/停止

  ```python
  docker rmi  镜像名字/镜像id   #删除镜像文件 
  docker rm  容器id/容器进程名字   #删除容器记录
  docker rmi -f  #强制删除镜像文件  
  docker  rm  `docker ps -aq`   #批量删除容器记录,只能删除挂掉的容器记录 
  ```

- 改

  ```python
  #docker容器进程的启停命令
  docker start  容器id 
  docker stop  容器id  
  ```

- 查

  ```python
  docker search  镜像文件名字   #搜索镜像文件
  docker images  #列出当前所有的镜像文件 
  docker ps   #列出当前记录正在运行的容器进程 
  docker ps -a  #列出所有的容器进程,以及挂掉的  
  
  docker logs 容器id  #查看容器内的日志信息 
  docker logs -f  容器id   #检测容器内的日志 
  ```

## 4. 容器镜像

- 容器镜像就是容器进程,基于容器镜像文件运行出的   

  ```python
  docker镜像就是python的 类,基于这个类可以无限的实例化
  docker的容器就是 类的实例化对象 
  ```

### 4.1提交自定制的镜像文件

```python
# 流程:
运行出容器实例  二次修改容器实例  提交容器实例为新的镜像  导出镜像 发给别人导入
```

1. docker run -it centos /bin/bash     #进入一个纯净的centos容器空间内,此时是最小化安装的系统,没有vim 没有py3 

2. 在容器空间内  yum install vim   ,然后退出容器

3. 此时这个容器记录就是携带者 vim的容器记录了(可以理解为携带者程序依赖,或者python3等等)

4. 提交这个容器为新的 镜像文件 

   ```python
   #  docker commit 容器进程id    镜像文件的名字 
   docker commit 419  s21docker-centos-vim 
   ```

5. 导出docker镜像,成为一个压缩文件,可以发送给你的测试,运维同事了

   ```python
   docker save 镜像文件名  > /opt/s21-centos-vim.tar.gz  
   ```

6. 此时可以发送文件,给别人导入了

   ```python
   docker load <  同事给你发的镜像文件
   ```

   

### 4.2容器内运行一个web程序 ,

- 容器内运行一个web程序 ,进行端口映射 

1.  下载一个flask的docker镜像 

   ```python
   docker pull training/webapp
   ```

2. 运行docker镜像

   ```python
   # docker run -d -P  
   	-d  后台运行 
   	-P  随机端口映射   随机的宿主机的端口:容器内的端口(自动指定的,由代码指定)
   	-p  指定端口映射   宿主机的7777:8500
   docker run -d -P training/webapp python app.py  #创建一个容器空间,然后在里面执行 python app.py 命令
   docker run -d -p  6000:5000 training/webapp python app.py  #创建一个容器空间,然后在里面执行 python app.py 命令
   ```

3. 访问这个容器应用 : 服务器ip:宿主机的映射端口 
   
- 123.206.16.61:32768 
  
4. 进入容器空间内,查看代码
   
   - docker exec -it 容器id  /bin/bash

### 4.3 dockerfile

- dockerfile学习,docker的脚本文件,用于构建镜像文件的

- 镜像是容器的基础，每次执行docker run的时候都会指定哪个镜像作为容器运行的基础。我们之前的例子都是使用来自docker hub的镜像，直接使用这些镜像只能满足一定的需求，当镜像无法满足我们的需求时，就得自定制这些镜像。

  ```python
  FROM scratch # 制作base image 基础镜像，尽量使用官方的image作为base image
  FROM centos # 使用base image
  FROM ubuntu:14.04 #带有tag的base image
  
  LABEL version=“1.0” #容器元信息，帮助信息，Metadata，类似于代码注释
  LABEL maintainer=“yc_uuu@163.com"
  
  #对于复杂的RUN命令，避免无用的分层，多条命令用反斜线换行，合成一条命令！
  RUN yum update && yum install -y vim \
      Python-dev #反斜线换行
  RUN /bin/bash -c "source $HOME/.bashrc;echo $HOME”
  
  WORKDIR /root #相当于linux的cd命令，改变目录，尽量使用绝对路径！！！不要用RUN cd
  WORKDIR /test #如果没有就自动创建
  WORKDIR demo #再进入demo文件夹
  RUN pwd     #打印结果应该是/test/demo
  
  ADD and COPY 
  ADD hello /  #把本地文件添加到镜像中，吧本地的hello可执行文件拷贝到镜像的/目录
  ADD test.tar.gz /  #添加到根目录并解压
  
  WORKDIR /root
  ADD hello test/  #进入/root/ 添加hello可执行命令到test目录下，也就是/root/test/hello 一个绝对路径
  COPY hello test/  #等同于上述ADD效果
  
  ADD与COPY
     - 优先使用COPY命令
      -ADD除了COPY功能还有解压功能
  添加远程文件/目录使用curl或wget
  
  ENV #环境变量，尽可能使用ENV增加可维护性
  ENV MYSQL_VERSION 5.6 #设置一个mysql常量
  RUN yum install -y mysql-server=“${MYSQL_VERSION}” 
  
  ------
  
  VOLUME and EXPOSE 
  存储和网络
  
  RUN and CMD and ENTRYPOINT
  RUN：执行命令并创建新的Image Layer
  CMD：设置容器启动后默认执行的命令和参数
  ENTRYPOINT：设置容器启动时运行的命令
  
  Shell格式和Exec格式
  RUN yum install -y vim
  CMD echo ”hello docker”
  ENTRYPOINT echo “hello docker”
  
  Exec格式
  RUN [“apt-get”,”install”,”-y”,”vim”]
  CMD [“/bin/echo”,”hello docker”]
  ENTRYPOINT [“/bin/echo”,”hello docker”]
  
  
  通过shell格式去运行命令，会读取$name指令，而exec格式是仅仅的执行一个命令，而不是shell指令
  cat Dockerfile
      FROM centos
      ENV name Docker
      ENTRYPOINT [“/bin/echo”,”hello $name”]#这个仅仅是执行echo命令，读取不了shell变量
      ENTRYPOINT  [“/bin/bash”,”-c”,”echo hello $name"]
  
  CMD
  容器启动时默认执行的命令
  如果docker run指定了其他命令(docker run -it [image] /bin/bash )，CMD命令被忽略
  如果定义多个CMD，只有最后一个执行
  
  ENTRYPOINT
  让容器以应用程序或服务形式运行
  不会被忽略，一定会执行
  最佳实践：写一个shell脚本作为entrypoint
  COPY docker-entrypoint.sh /usr/local/bin
  ENTRYPOINT [“docker-entrypoint.sh]
  EXPOSE 27017
  CMD [“mongod”]
  
  [root@master home]# more Dockerfile
  FROm centos
  ENV name Docker
  #CMD ["/bin/bash","-c","echo hello $name"]
  ENTRYPOINT ["/bin/bash","-c","echo hello $name”]
  ```

  

- ##### dockerfile构建一个flask web app

1. 编写flask代码文件 

   ```python
   s21flask.py 
   	#coding:utf8
   	from flask import Flask
   	app=Flask(__name__)
   	@app.route('/')
   	def hello():
   		return "hello docker"
   	if __name__=="__main__":
   		app.run(host='0.0.0.0',port=8080)
   ```

2. 准备Dockerfile (名字必须叫做 Dockerfile)

   - touch  Dockerfile  ,写入如下内容

   ```python
   [root@VM_32_137_centos s21docker]# cat Dockerfile 
   	FROM centos
   	COPY CentOS-Base.repo /etc/yum.repos.d/
   	COPY epel.repo /etc/yum.repos.d/
   	RUN yum clean all
   	RUN yum install python-setuptools -y
   	RUN easy_install flask
   	COPY s21flask.py /opt/
   	WORKDIR /opt
   	EXPOSE 8080
   	CMD ["python","s21flask.py"]
   ```

   - 在Dockerfile同级目录,准备好其他环境文件,代码文件

     ```python
     [root@VM_32_137_centos s21docker]# ls
     CentOS-Base.repo  Dockerfile  epel.repo  s21flask.py
     ```

3. 构建docker镜像

   ```python
   docker build -t yuchao163/s21-flask-docker .
   
   	docker build  编译Dockerfile  
   	-t  给镜像加上名字  ,镜像名字,以仓库地址开头,则可以推送到仓库中管理
   	.    找到当前的Dockefile文件 
   ```

4. 构建完毕之后,查看镜像文件

   ```python
   docker images  
   ```

5. 运行这个flask镜像文件,生成容器实例,代码就跑在容器中了

   ```python
   docker run -d  -p  5000:8080  指定你要运行的镜像id/名字
   ```

### 4.6 仓库管理

- 推送本地镜像到dockerhub 共有仓库(私密代码不安全)

  ```python
  1.登录docker账户
  docker login
  2.修改docker镜像文件名字,以docker hub账号开头 
  docker tag docker.io/hello-world   yuchao163/s21-hello
  3.推送镜像到dockerhub仓库中,(注意这个是公共仓库)
  docker push  yuchao163/s21-hello 
  ```

- #私有docker仓库搭建(保证镜像安全)

  ```python
  
  修改docker的配置文件,  
  cat /etc/docker/daemon.json  ,内容如下
  
  	{                                                                                                      
      "registry-mirrors": ["http://f1361db2.m.daocloud.io"],
       "insecure-registries": [
      "192.168.182.130:5000"
    ]
  }
  
  
  3.修改docker的启动文件,加载第二步,修改的配置文件
  
  vim /lib/systemd/system/docker.service  #找到如下的[Service]  代码块,添加参数
  
  [Service]
  
  EnvironmentFile= -/etc/docker/daemon.json  
  	
  4.修改了docker配置文件，重新加载docker
  systemctl daemon-reload
  	
  5.重启docker服务 
  
  systemctl restart docker
  
  6.由于重启了docker,需要重新运行私有仓库的容器进程
  
  docker run --privileged=true -d -p 5000:5000 -v /opt/data/registry:/var/lib/registry registry
  
  
  --privileged=true  docker容器的安全机制：设置特权级运行的容器
  
   1042  docker pull 192.168.182.130:5000/s21-666-hello
   1043  docker run 192.168.182.130:5000/s21-666-hello
  ```

  