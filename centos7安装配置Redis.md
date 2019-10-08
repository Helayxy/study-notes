### centos7安装配置Redis

#### 一、安装

- [官网](https://redis.io/download)下载安装包：redis-4.0.9.tar.gz
- 上传至安装目录下：/usr/local/leyou
- 修改权限：

```shell
chmod 755 redis-4.0.9.tar.gz
```

- 重命名：

```shell
mv redis-4.0.9.tar.gz redis
```

- 解压：

```shell
tar -xvf redis-4.0.9.tar.gz
```

- 编译安装：

```shell
cd redis # 进入redis目录
make && make install # redis是用C语言编写的，所以需要编译、安装
```

#### 二、配置

- 修改安装目录下的redis.conf文件：

```shell
vi redis.conf
```

- 修改以下配置：

```shell
#bind 127.0.0.1 # 将这行代码注释，监听所有的ip地址，外网可以访问
protected-mode no # 把yes改成no，允许外网访问
daemonize yes # 把no改成yes，后台运行
```

#### 三、启动或停止

**redis提供了服务端命令和客户端命令：**

- redis-server 服务端命令，可以包含以下参数：
  start 启动
  stop 停止

- redis-cli 客户端控制台，包含参数：
  -h xxx 指定服务端地址，默认值是127.0.0.1
  -p xxx 指定服务端端口，默认值是6379

#### 四、设置开机自启

- 输入命令，新建文件：

```shell
vi /etc/init.d/redis
```

- 输入下面内容：

```shell
#!/bin/sh
# chkconfig:   2345 90 10
# description:  Redis is a persistent key-value database
PATH=/usr/local/bin:/sbin:/usr/bin:/bin

REDISPORT=6379
EXEC=/usr/local/bin/redis-server
REDIS_CLI=/usr/local/bin/redis-cli

PIDFILE=/var/run/redis.pid

CONF="/usr/local/leyou/redis/redis.conf"

case "$1" in  
    start)  
        if [ -f $PIDFILE ]  
        then  
                echo "$PIDFILE exists, process is already running or crashed"  
        else  
                echo "Starting Redis server..."  
                $EXEC $CONF  
        fi  
        if [ "$?"="0" ]   
        then  
              echo "Redis is running..."  
        fi  
        ;;  
    stop)  
        if [ ! -f $PIDFILE ]  
        then  
                echo "$PIDFILE does not exist, process is not running"  
        else  
                PID=$(cat $PIDFILE)  
                echo "Stopping ..."  
                $REDIS_CLI -p $REDISPORT SHUTDOWN  
                while [ -x ${PIDFILE} ]  
               do  
                    echo "Waiting for Redis to shutdown ..."  
                    sleep 1  
                done  
                echo "Redis stopped"  
        fi  
        ;;  
   restart|force-reload)  
        ${0} stop  
        ${0} start  
        ;;  
  *)  
    echo "Usage: /etc/init.d/redis {start|stop|restart|force-reload}" >&2  
        exit 1  
esac

```

- 然后：wq保存退出

*注意：以下信息需要根据安装目录进行调整：*

> EXEC=/usr/local/bin/redis-server # 执行脚本的地址
>
> REDIS_CLI=/usr/local/bin/redis-cli # 客户端执行脚本的地址
>
> PIDFILE=/var/run/redis.pid # 进程id文件地址
>
> CONF="/usr/local/src/redis-3.0.2/redis.conf" #配置文件地址

*若是和上面的路径一致，则不需要修改任何路径*

- 设置权限：

```shell
chmod 755 /etc/init.d/redis
```

- 启动测试：

```shell
/etc/init.d/redis start
```

- 启动成功会提示如下信息：

```shell
Starting Redis server...
Redis is running...
```

- 设置开机自启动

```shell
chkconfig --add /etc/init.d/redis
chkconfig redis on
```

