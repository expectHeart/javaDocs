# windows安装mysql

1.去下载

![官网下载mysql](MySQL下载和安装img\官网下载mysql.gif)





2.安装过程

![安装过程mysql](MySQL下载和安装img\安装过程mysql.gif)

3.配置环境变量

![配置环境变量](MySQL下载和安装img\配置环境变量.gif)

# linux安装mysql

## 一、下载安装

### 1.yum安装方式（相对简单）

##### 1．配置Mysql 8.0安装源

```bash
rpm -Uvh https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm
```

##### 2．安装Mysql 8.0

```bash
yum --enablerepo=mysql80-community install mysql-community-server
```

##### 3．启动mysql服务

```bash
service mysqld start
```

##### 4．查看mysql服务运行状态

```bash
service mysqld status
设置开机自启动
# chkconfig mysqld on  或者  systemctl enable mysqld
```

##### 5．查看root临时密码

安装完mysql之后，会生成一个临时的密码让root用户登录

```bash
grep "A temporary password" /var/log/mysqld.log
```

## 2.安装包安装方式（相对复杂）

##### 1.去官网下载（linux通用版）

<img src="MySQL下载和安装img\下载mysqlLinux包.png" alt="image-20210715142013957" style="zoom:50%;" /> 



##### 2.解压

```bash
tar -xvf mysql-8.0.24-linux-glibc2.12-i686.tar.xz 
```

##### 3.移动并且改名(注意：软件安装一般放在usr/local/目录下)

```bash
mv mysql-8.0.24-linux-glibc2.12-i686 /usr/local/mysql
```

##### 4.创建mysql用户组、mysql用户

```bash
#创建用户组
groupadd mysql
#创建 用户并且指定用户组（[-r表示是新用户]  [-g 用户组]）
useradd -r -g mysql mysql
```

##### 5.在/usr/local/mysql目录下创建两个目录

(data:mysql数据库文件；tmp:mysql连接socket)

```bash
mkdir data
mkdir tmp
```

##### 6.初始化数据库

```bash
bin/mysqld --initialize --user=mysql --datadir=/usr/local/mysql/data --basedir=/usr/local/mysql
```

可能出现的问题

mysql官网给的提示

![image-20210715152321024](C:\Users\32176\Desktop\expect项目总结\mysql\1.下载安装\MySQL下载和安装img\mysql给的提示.png)

```bash
yum install libaio
```

```bash
yum install glibc.i686
yum install libgcc.i686
```

```bash
#这个都下载一下
yum -y install numactl
yum install libnuma
yum install ld-linux.so.2
yum install libaio.so.1
yum install libnuma.so.1
yum install libstdc++.so.6
yum install libncurses.so.5
yum install libtinfo.so.5io
yum clean all
yum makecacheast
```



缺少libstdc++.so.6

1.找出下载源

```bash
yum whatprovides libstdc++.so.6
```

![image-20210715152122948](MySQL下载和安装img\安装过程的问题6.png) 

2.下载

```bash
yum -y install libstdc++-4.8.5-44.el7.i686 : GNU Standard C++ Library
```

##### 6.再执行初始化数据库

```bash
bin/mysqld --initialize --user=mysql --datadir=/usr/local/mysql/data --basedir=/usr/local/mysql
```



![image-20210715153004258](MySQL下载和安装img\linux初始化数据库后.png)

密码csi<(pa;F2tm

##### 7.将mysql及其下所有的目录所有者和组均设为mysql

```bash
 chown  -R mysql:mysql /usr/local/mysql/
```



修改/etc/my.cnf配置文件

```bash
[mysqld]
datadir=/usr/local/mysql/data
socket=/usr/local/mysql/tmp/mysql.sock
user=mysql
# Disabling symbolic-links is recommended to prevent assorted security risks
symbolic-links=0
# Settings user and group are ignored when systemd is used.
# If you need to run mysqld under a different user or group,
# customize your systemd unit file for mariadb according to the
# instructions in http://fedoraproject.org/wiki/Systemd

[mysqld_safe]
log-error=/var/log/mysql/mydqld.log
pid-file=/var/run/mysql/mydqld.pid

[client]
socket=/usr/local/mysql/tmp/mysql.scok

#
# include all files from the config directory
#
!includedir /etc/my.cnf.d

```

##### 8.软后放弃了

## 二、设置远程连接

```bash
#登录mysql
mysql -u root –p
#切换数据库 到use
mysql>use mysql;
# 修改user表
mysql>update user set host = '%' where user = 'root';
# 查看修改结果
mysql>select host, user from user;
# 刷新配置
mysql>FLUSH PRIVILEGES;
```



## 三、卸载

查看安装的MySQL软件

```bash
rpm -qa|grep mysql
```

一个一个卸载即可

```bash
rpm -e MySQL-server-5.6.17-1.el6.i686
rpm -e MySQL-client-5.6.17-1.el6.i686
```

删除mysql服务

```bash
chkconfig --list | grep -i mysql
chkconfig --del mysql
```

清空相关目录

```bash
whereis mysql 或者 find / -name mysql
mysql: /usr/lib/mysql /usr/share/mysql
#清空相关mysql的所有目录以及文件
rm -rf /usr/lib/mysql
rm -rf /usr/share/mysql
rm -rf /usr/my.cnf
```



[cysqld]
datadir-/usr/local/mysql/data
socket=/usr/local/mysql/tmp/mysql. sock
user=mysql
Disabling symbolic-links is recomended to prevent assorted security risks
symbolic-links=O
[mysqld safe]
Log- error=/var/log/mysqld. Log
pid-file=/var/run/mysqld/mysqld. pid
的改后的智时就 .
[client]
s0c ketm /us r/1ocal /mysql /tmp/mysql sock