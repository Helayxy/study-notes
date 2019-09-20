#### centos7系统下安装jdk1.8

##### 1.将linux版本jdk1.8的tar压缩包上传至虚拟机任意位置

```shell
[root@localhost /]# ls
bin   dev  home   jdk-8u211-linux-x64.tar.gz  lib    media  opt   root  sbin  sys  usr
boot  etc  lib64  mnt    proc  run   srv   tmp  var
```

##### 2.解压

```shell
tar  -zxvf  jdk-8u131-linux-x64.tar.gz
```

##### 3.查看解压是否成功

```shell
[root@localhost /]# ls
bin   dev  home   jdk-8u211-linux-x64.tar.gz  lib    media  opt   root  sbin  sys  usr
boot  etc  jdk1.8.0_211  leyou   lib64  mnt    proc  run   srv   tmp  var
```

**可以看到有jdk1.8.0_211文件夹，表示解压成功！**

##### 4.删除没有用的压缩包

```shell
[root@localhost /]# rm  jdk-8u211-linux-x64.tar.gz
rm：是否删除普通文件 "jdk-8u211-linux-x64.tar.gz"？y
```

##### 5.创建jdk的安装位置

```shell
[root@localhost /]# mkdir /usr/java
```

**说明：我这里安装到usr目录下的java文件夹中**

##### 6.移动解压后的jdk至指定的安装位置

```shell
[root@localhost java]# mv /jdk1.8.0_211 /usr/java
```

##### 7.修改环境变量

```shell
[root@localhost jdk1.8.0_211]# vi /etc/profile
```

**说明：/etc/profile为系统的配置文件，在改文件末尾添加如下配置信息：**

```shell
export JAVA_HOME=/usr/java/jdk1.8.0_211
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib:$CLASSPATH
export JAVA_PATH=${JAVA_HOME}/bin:${JRE_HOME}/bin
export PATH=$PATH:${JAVA_PATH}
```

**使用：wq保存退出**

##### 8.让这个环境变量配置信息里面生效

```shell
[root@localhost jdk1.8.0_211]# source /etc/profile
```

**说明：此命令可以是配置信息立马生效，不适用此命令需要重启虚拟机**

##### 9.验证jdk是否安装成功

输入javac命令查看：

```shell
[root@localhost jdk1.8.0_211]# javac
用法: javac <options> <source files>
其中, 可能的选项包括:
  -g                         生成所有调试信息
  -g:none                    不生成任何调试信息
  -g:{lines,vars,source}     只生成某些调试信息
  -nowarn                    不生成任何警告
  -verbose                   输出有关编译器正在执行的操作的消息
  -deprecation               输出使用已过时的 API 的源位置
  -classpath <路径>            指定查找用户类文件和注释处理程序的位置
  -cp <路径>                   指定查找用户类文件和注释处理程序的位置
  -sourcepath <路径>           指定查找输入源文件的位置
  -bootclasspath <路径>        覆盖引导类文件的位置
  -extdirs <目录>              覆盖所安装扩展的位置
  -endorseddirs <目录>         覆盖签名的标准路径的位置
  -proc:{none,only}          控制是否执行注释处理和/或编译。
  -processor <class1>[,<class2>,<class3>...] 要运行的注释处理程序的名称; 绕过默认的搜索进程
  -processorpath <路径>        指定查找注释处理程序的位置
  -parameters                生成元数据以用于方法参数的反射
  -d <目录>                    指定放置生成的类文件的位置
  -s <目录>                    指定放置生成的源文件的位置
  -h <目录>                    指定放置生成的本机标头文件的位置
  -implicit:{none,class}     指定是否为隐式引用文件生成类文件
  -encoding <编码>             指定源文件使用的字符编码
  -source <发行版>              提供与指定发行版的源兼容性
  -target <发行版>              生成特定 VM 版本的类文件
  -profile <配置文件>            请确保使用的 API 在指定的配置文件中可用
  -version                   版本信息
  -help                      输出标准选项的提要
  -A关键字[=值]                  传递给注释处理程序的选项
  -X                         输出非标准选项的提要
  -J<标记>                     直接将 <标记> 传递给运行时系统
  -Werror                    出现警告时终止编译
  @<文件名>                     从文件读取选项和文件名
```

输入java -version查看java版本信息：

```shell
[root@localhost jdk1.8.0_211]# java -version
java version "1.8.0_211"
Java(TM) SE Runtime Environment (build 1.8.0_211-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.211-b12, mixed mode)
```

输入echo $PATH查看路径：

```shell
[root@localhost jdk1.8.0_211]# echo $PATH
/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/root/bin:/usr/java/jdk1.8.0_211/bin:/usr/java/jdk1.8.0_211/jre/bin
```

##### 10.以上都正确则表示jdk安装成功！