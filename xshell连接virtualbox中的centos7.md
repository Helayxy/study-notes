### xshell连接virtualbox中的centos7

#### 一、下载centos7：

1.来到这个[网址](http://isoredirect.centos.org/)选择适合自己电脑操作系统的centos7的镜像（一般选择第一个，即for CentOS 7 x86_64 iso images这个版本的）

2.进入该版本的[链接地址](http://isoredirect.centos.org/centos/7/isos/x86_64/)，The following mirrors in your region should have the ISO images available:表示当前所在国家或地区的镜像；Other mirrors further away:表示周边国家或地区的镜像。这里我们一般选择当前所在国家或地区的镜像，因为这样下载速度较快

3.选择阿里云的镜像，这个[链接地址](http://mirrors.aliyun.com/centos/7.6.1810/isos/x86_64/)，下载CentOS-7-x86_64-DVD-1810.iso  这个版本的镜像，centos7的大小约为4个多G，下载可能需要四五分钟

#### 二、下载virtualbox：

1.进入[腾讯软件中心官网](https://pc.qq.com/)，搜索virtualbox，选择下载

2.默认安装即可，第二步**注意需要点击VirtualBox Networking，选择将整个功能安装到本机硬盘，**接下来一路“下一步”即可

3.注意安装过程会提示网络界面的警告，全部选择“是”即可，不然可能出在最后虚拟机不能上网

#### 三、使用virtualbox打开centos7

1.打开virtualbox，选择新建，填写虚拟电脑的名称，版本这里选择Red Hat 64-bit，点击下一步

2.下面的步骤全部选择默认即可

3.打开虚拟机，根据向导设置语言，选择中文

4.中间过程会出现警告框让设置密码，自行设置即可

5.安装过程中还会出现让你分配内存的警告框，点击进去，点击左上角的“完成”即可

#### 四、网络设置

1.在virtualbox中选择设置，找到网络，在接有网线的前提下，选择桥接网卡，点击“OK”

2.输入用户名和密码登录成功后，执行cd  /etc/sysconfig/nerwork-scripts进入该目录

3.ls列出nerwork-scripts这个目录下的所有文件，选择ifcfg-en*开头的文件，每个人的这个文件名不一样

4.vi  ifcfg-en*这个文件，进入编辑模式，将文件的最后一行的ONBOOT的值由no置位yes，按Esc键退出编辑模式，按下：wq进行保存退出（：q！表示不保存强制退出  ）

5.退出后执行service  network   restart命令中心启动网络配置

6.使用ip  addr查看网络情况，注意：centos7已经不支持ipconfig命令，所以这个命令在centos7中无效

#### 五、xshell连接centos7

1.打开xshell连接工具，选择新建，在主机一栏填写ip  addr命令查看到的网络地址

2.选择用户身份验证，填写centos中的用户名和密码

3.没有其他情况应该可以连接成功











