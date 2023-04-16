- 卸载原有的MariaDB

  ```sh
  # 查看系统自带的MariaDB
  [root@mysql_master ~]# rpm -qa |grep mariadb
   mariadb-libs-5.5.56-2.el7.x86_64
   
  # 卸载系统自带的MariaDB
  [root@mysql_master ~]# rpm -e --nodeps mariadb-libs-5.5.56-2.el7.x86_64
  
  # 删除 /etc/my.cnf配置文件
  [root@mysql_master ~]# rm -rf /etc/my.cnf
  ```

- 关闭防火墙

  ```sh
  #查看防火墙状态
  firewall-cmd --state
  
  #关闭防火墙
  systemctl stop firewalld.service
  
  # 禁止防火墙开机自动启动
  systemctl disable firewalld.service
  ```

  

- 检查

  ```sh
  # 执行下面的命令 没有返回值说明 MySQL不存在
  rpm -qa |grep mysql
  ```

- 创建msyql用户

  ```sh
  # 创建MySQL用户组和用户 并在/home文件加下 创建mysql用户主目录
  adduser mysql
  
  # 修改MySQL用户登录密码 按照提示输入两次密码即可
  passwd mysql
  ```

- yum 先决条件

  ```sh
  # 检查
  rpm -qa | grep ncurses
  rpm -qa | grep libaio
  # 没有就安装
  yum install libaio
  yum  install numactl
  ```

- 下载安装包

  - https://mirrors.cloud.tencent.com/mysql/downloads/MySQL-5.7/mysql-5.7.34-1.el7.x86_64.rpm-bundle.tar

- 解压并安装

  ```sh
  # 解压
  tar -xvf mysql-5.7.22-1.el7.x86_64.rpm-bundle.tar
  # 安装（按顺序进行）
  rpm -ivh mysql-community-common-5.7.22-1.el7.x86_64.rpm
  rpm -ivh mysql-community-libs-5.7.22-1.el7.x86_64.rpm
  rpm -ivh mysql-community-client-5.7.22-1.el7.x86_64.rpm
  rpm -ivh mysql-community-server-5.7.22-1.el7.x86_64.rpm
  ```

- 数据库初始化

  ```sh
  mysqld --initialize  #初始化后会在/var/log/mysqld.log生成随机密码
  ```

- 修改mysql数据库目录的所属用户及其所属组, 并启动

  ```sh
  chown mysql:mysql /var/lib/mysql -R
  systemctl start mysqld.service
  systemctl status mysqld.service
  ```

- 查看初始密码

  ```sh
  grep 'password' /var/log/mysqld.log
  ```

- 登录mysql,并修改默认密码

  ```sh
  mysql -uroot -p'-4iq<tyjVpLb' # 登录
  
  # set password=password('msyql'); # 设置密码
  set password for 'root'@'localhost'=password('msyql'); # 设置密码
  #ALTER USER 'root'@'localhost' IDENTIFIED BY 'msyql'; # 设置密码
  # update user set host='%',authentication_string=password('mysql') where user='root'; # 设置密码
  flush privileges; # 刷新权限
  
  ```

  - 配置免密登录-可以忽略，一般用来修改密码

    ```sh
    vim /etc/my.cnf
    
    [mysqld]
    skip-grant-tables
    
    systemctl restart mysqld.service
    mysql -u root -p
    use mysql; 
    update user set host='%',authentication_string=password('mysql') where user='root';
    flush privileges;
    ```

    

- 配置远程访问权限

  ```sh
  GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'mysql' WITH GRANT OPTION;
  flush privileges;
  ```

  

- 创建用户和数据库并赋权

  ```sh
  create user 'test'@'%' identified by 'test';
  create database test_db;
  grant all on test_db.* to 'test'@'%';
  ```



