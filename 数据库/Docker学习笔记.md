# Docker学习笔记

## docker服务相关命令

* 启动docker服务

	```shell
	sudo systemctl start docker
	```

* 停止docker服务

	```shell
	sudo systemctl stop docker
	```

* 重启docker服务

	```shell
	sudo systemctl restart docker
	```

* 查看docker运行状态

	```shell
	sudo systemctl status docker
	```

* 开机自启动docker

	```shell
	sudo systemctl enable docker
	```

## docker镜像相关命令

* 查看本地镜像

	```shell
	sudo docker images	#	可以看到所有镜像的表格
	sudo docker images -q	#	可以显示所有镜像的id，一般用于删除全部的镜像
	```

* 在网上查找需要的镜像

	```shell
	sudo docker search 镜像名称
	```

* 拉取镜像到本地

	```shell
	sudo docker pull 镜像名称
	#	当你需要指定镜像版本时
	sudo docker pull 镜像名称:版本号
	#	不知道具体版本号的需要到docker hub上查看
	```

* 删除镜像

	```shell
	sudo docker rmi 镜像id
	sudo docker rmi 镜像名称:版本号
	sudo docker rmi 'docker images -q'	#	可以删除本地所有镜像
	```

## docker容器相关命令

* 查看容器

	```shell
	sudo docker ps # 查看正在运行的容器
	sudo docker ps -a # 查看所有运行过的容器
	```

* 创建及启动容器

	```shell
	sudo docker run 参数 # 参数说明请看下面
	```

	1. `-i`: 保持容器运行。

	2. `-t`:为容器创建一个输入终端，通常与`-i`合并成`-it`，效果是容器创建之后自动进入，退出时自动关闭。

	3. `-d`:以后台模式运行，退出后容器也不会关闭

	4. `-it`:交互式容器，`-id`:守护式容器

	5. `--name`:对容器起名字

	6. 实例

		```shell
		sudo docker run -it --name=c1 centos:latest /bin/bash
		sudo docker run -id --name=c2 centos:latest /bin/bash
		```

* 退出和进入容器

	```shell
	exit # 退出当前容器
	sudo docker exec -it c2 /bin/bash # 进入后台运行的容器
	sudo docker stop 容器名称 # 停止容器
	sudo docker rm 容器名称 # 删除容器，不能删除正在运行的容器
	sudo docker inspect 容器名称 # 显示容器信息
	sudo docker start 容器名称 # 重启刚刚关闭的容器
	```
```shell

## docker挂载数据卷

在创建容器时：

​```shell
docker run ... -v 宿主机目录(文件):容器内目录(文件)...
# for example
# terminal win1
sudo docker run -it --name=c3 -v ~/data:/root/data centos:7
# terminal win2
sodo docker run -it --name=c4 -v ~/data:/root/data centos:7
# 这个时候两个容器挂在到同一个宿主文件夹中

# 在cd进root的时候，记得开管理员权限
su
# terminal win1
cd ~/data
echo itcast > itcast.txt
# teiminal win2
cd ~/data
cat itcast.txt
# 可以发现，两个容器共享了数据

# terminal
exit
sudo docker rm c3
# teiminal
exit
sudo docker rm c4
# 即使把容器删除，挂在到数据卷的数据也不会被清除
su
cd ~/data
ll
cat itcast.txt
```

## 数据卷容器

```shell
sudo docker run -it --name=c3 -v /volume centos:7
# 建立一个数据卷容器，不用像以前那样指定宿主机的位置，只需要写一个容器目录

sudo docker run -it --name=c1 --volumes-from c3 centos:7
# 建立一个容器，挂在到数据卷容器上
--数据卷容器目录-from 数据卷容器名称
```

## 部署MySQL并连接到sqlyog上

1. 搜索mysql镜像

```shell
docker search mysql
```

2. 拉取mysql镜像

```shell
docker pull mysql:5.6
```

3. 创建容器，设置端口映射、目录映射

	```shell
	# 在/root目录下创建mysql目录用于存储mysql数据信息
	mkdir ~/mysql
	cd ~/mysql
	```

	```shell
	docker run -id \
	-p 3307:3306 \
	--name=c_mysql \
	-v $PWD/conf:/etc/mysql/conf.d \
	-v $PWD/logs:/logs \
	-v $PWD/data:/var/lib/mysql \
	-e MYSQL_ROOT_PASSWORD=123456 \
	mysql:5.6
	```

- 参数说明：
	- **-p 3307:3306**：将容器的 3306 端口映射到宿主机的 3307 端口。
	- **-v $PWD/conf:/etc/mysql/conf.d**：将主机当前目录下的 conf/my.cnf 挂载到容器的 /etc/mysql/my.cnf。配置目录
	- **-v $PWD/logs:/logs**：将主机当前目录下的 logs 目录挂载到容器的 /logs。日志目录
	- **-v $PWD/data:/var/lib/mysql** ：将主机当前目录下的data目录挂载到容器的 /var/lib/mysql 。数据目录
	- **-e MYSQL_ROOT_PASSWORD=123456：**初始化 root 用户的密码。



4. 进入容器，操作mysql

```shell
sudo docker exec –it c_mysql /bin/bash
```

5. 使用外部机器连接容器中的mysql

![1573636765632](https://raw.githubusercontent.com/HYBB-rash/cnBlogs/master/img/20200406202524.png)

如果出现报错，报的错误代码是2058，解决办法：更改MySQL的权限，在MySQL中进行如下操作

```mysql
 // 授权
flush privileges; // 刷新权限
ALTER USER 'root'@'localhost' IDENTIFIED BY '123456' PASSWORD EXPIRE NEVER; // 更改成旧版本的加密方式
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456';  // 更新root用户密码
flush privileges; // 刷新权限
exit; //退出mysql
```

如果不知道本机的IP地址（linux）:

```shell
sudo apt update
sudo apt install net-tools
sudo ifconfig -a
```

弹出信息：

```shell
docker0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        inet6 fe80::42:93ff:fe32:6895  prefixlen 64  scopeid 0x20<link>
        ether 02:42:93:32:68:95  txqueuelen 0  (Ethernet)
        RX packets 954  bytes 417501 (417.5 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 1025  bytes 116247 (116.2 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

ens33: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.253.131  netmask 255.255.255.0  broadcast 192.168.253.255
        inet6 fe80::830c:e766:1212:ffa7  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:f9:df:7b  txqueuelen 1000  (Ethernet)
        RX packets 86054  bytes 122445794 (122.4 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 34149  bytes 2648779 (2.6 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 5184  bytes 427089 (427.0 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 5184  bytes 427089 (427.0 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

vethee86c18: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet6 fe80::787a:65ff:fef0:7f8a  prefixlen 64  scopeid 0x20<link>
        ether 7a:7a:65:f0:7f:8a  txqueuelen 0  (Ethernet)
        RX packets 26  bytes 14122 (14.1 KB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 56  bytes 6866 (6.8 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

```

找`ens33`的`inet`的ip地址。如果不是`ens33`可能就是类似命名的。

完成以后就可以在sqlyog操作虚拟机里docker的MySQL了。

## 部署tomcat

1. 搜索tomcat镜像

```shell
docker search tomcat
```

2. 拉取tomcat镜像

```shell
docker pull tomcat
```

3. 创建容器，设置端口映射、目录映射

```shell
# 在/root目录下创建tomcat目录用于存储tomcat数据信息
mkdir ~/tomcat
cd ~/tomcat
```

```shell
docker run -id --name=c_tomcat \
-p 8080:8080 \
-v $PWD:/usr/local/tomcat/webapps \
tomcat 
```

- 参数说明：

	- **-p 8080:8080：**将容器的8080端口映射到主机的8080端口

		**-v $PWD:/usr/local/tomcat/webapps：**将主机中当前目录挂载到容器的webapps



4. 使用外部机器访问tomcat

![1573649804623](https://raw.githubusercontent.com/HYBB-rash/cnBlogs/master/img/20200406204156.png)



