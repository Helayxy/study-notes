### centos7下安装RabbitMQ

**由于RabbitMQ是用Erlang编写的，所以需要先安装Erlang语言的运行环境**

##### 一、安装Erlang

###### 1.在线安装（需要Linux虚拟机能连接外网）

```shell
yum install esl-erlang_17.3-1~centos~6_amd64.rpm

yum install esl-erlang-compat-R14B-1.el6.noarch.rpm
```

###### 2.离线安装

（1）环境准备（需要以下四个安装包）

```shell
esl-erlang_17.3-1~centos~6_amd64.rpm

esl-erlang-17.3-1.x86_64.rpm

esl-erlang-compat-R14B-1.el6.noarch.rpm

rabbitmq-server-3.4.1-1.noarch.rpm
```

（2）开始安装

依次执行以下命令开始安装：

```shell
rpm -ivh esl-erlang-17.3-1.x86_64.rpm --force --nodeps

rpm -ivh esl-erlang_17.3-1~centos~6_amd64.rpm --force --nodeps

rpm -ivh esl-erlang-compat-R14B-1.el6.noarch.rpm --force --nodeps
```

说明：

i表示安装，v表示显示安装过程，h表示显示进度；

--force：表示强制安装；

--nodeps：表示忽略依赖冲突；

##### 二、安装安装RabbitMQ

###### 1.使用下面所示命令进行安装

```shell
rpm -ivh rabbitmq-server-3.4.1-1.noarch.rpm --force --nodeps
```

###### 2.设置配置文件

```shell
cp /usr/share/doc/rabbitmq-server-3.4.1/rabbitmq.config.example /etc/rabbitmq/rabbitmq.config
```

###### 3.修改配置文件，开启用户远程访问

```shell
vi /etc/rabbitmq/rabbitmq.config
```

```config
%% Uncomment the following line if you want to allow access to the
   %% guest user from anywhere on the network.
   {loopback_users, []}
```

将 {loopback_users, []}前面的%%和末尾的，去掉，保存退出

###### 4.启动、停止RabbitMQ

```shell
service rabbitmq-server start	#启动

service rabbitmq-server stop	#关闭

service rabbitmq-server restart	#重启
```

###### 5.开启web界面管理工具

```shell
rabbitmq-plugins enable rabbitmq_management	#开启web界面管理工具

service rabbitmq-server restart	#重启
```

###### 6.设置开机启动

```shell
chkconfig rabbitmq-server on
```

###### 7.防火墙开放15672端口

```shell
/sbin/iptables -I INPUT -p tcp --dport 15672 -j ACCEPT

/etc/rc.d/init.d/iptables save
```

###### 8.或者关闭防火墙

```shell
systemctl stop firewalld.service
```

注意：第7步和第8步选择一个即可，关闭防火墙就不需要开放端口了

