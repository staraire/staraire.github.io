---
title: 黑马教程金融类安全传输平台项目部署
date: 2023-08-29 22:44:17
tags:
---


> 看完这个教程之后[金融类安全传输平台项目（C/C++阶段五）](https://www.bilibili.com/video/BV1fg411j7SF/)，看见评论区有个兄弟分享了源码，https://github.com/Peachol/Secure_data_transmission
于是我就clone下来尝试跑一下，结果发现事情并没有那么简单，花了一下午才把项目跑起来，然后目前还有报错，但是已经能让客户端和服务端通信了，故记录一下环境搭建的过程。

## Clone源码和补充Makefile脚本

上面clone下来的源码，有两个文件夹，`server`和`client`，其中`client`写了Makefile，server里面没写，那我们就补充一个Makefile，把client的复制下来稍微改一改

```makefile
src=$(wildcard *.cpp)
objs=$(patsubst %.cpp,%.o,$(src))

target=serverMain
LIBS=-lstdc++ -pthread -lssl -lcrypto -locci -lclntsh
DEFINE=-D_GLIBCXX_USE_CXX11_ABI=0

$(target):$(objs)
	g++ -o $@ $^ $(LIBS)  $(DEFINE)

%.o:%.cpp
	g++ -std=c++11 -c $< 

.PHONY:clean
clean:
	rm -f $(target) $(objs)

```

尝试make，有报错，别急，一点一点配置环境。


## 客户端

客户端的环境配置比较简单，就是需要一个openssl的库


### 安装openssl

打开网址 https://www.openssl.org/source/

下载1版本：`openssl-1.1.1v.tar.gz`

拷贝到linux里，然后解压`tar -xzvf`

然后执行`./config`

编译`make -j4`

安装`make install`

测试`openssl version -a`

如果报错

搜索动态库路径`find / -name libcrypto.so`

然后将搜索到的路径放到`/etc/ld.so.conf`里：`vim /etc/ld.so.conf`
然后生效：`sudo ldconfig`

再次测试：`openssl version -a`

通过，此时就可以编译客户端的程序了

over



## 服务端


服务端主要繁琐在数据库的配置，如果没有配置过数据库，那真是要死要活，这里我原本打算直接在linux里安装oracle，但是搜了一下安装过程，简直要命，然后我就想到了docker。

### docker安装oracle

docker安装oracle我是参考的这两个教程：
https://blog.csdn.net/momo1938/article/details/100514093
https://blog.csdn.net/m0_57179014/article/details/120182269
但并不能解决100%的问题。

- 安装docker和启动docker

`sudo apt install docker.io`
`systemctl start docker`

加入开机自启：`systemctl enable docker`

- 安装oracle11g

`sudo docker pull registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g`

`> sudo docker run -d -p 1521:1521 --name oracle registry.cn-hangzhou.aliyuncs.com/helowin/oracle_11g`

启动容器`sudo docker start oracle`

进入容器镜像`sudo docker exec -it oracle bash`

- 配置环境变量

切换root权限：`su root`
> 密码是：`helowin`

这里默认进去是`oracle`的用户，这个用户的密码我不知道，我是通过`root`用户去修改用户的密码的`passwd oracle`

`vi /etc/profile`

下面的SID这个不能错

```shell
export ORACLE_HOME=/home/oracle/app/oracle/product/11.2.0/dbhome_2
export ORACLE_SID=helowin 
export PATH=$ORACLEHOME/bin:PATH
```
保存`wq!`后执行`source /etc/profile`

切回`oracle`：`su - oracle`

- 进入数据库，创建用户，修改配置

`sqlplus / as sysdba`

` create user SECMNG identified by SECMNG;` 创建内部管理员账号密码；
` grant resource,connect,dba to SECMNG;` 将dba权限授权给内部管理员账号和密码；
`ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;` 设置密码永不过期：
`alter system set processes=1000 scope=spfile;` 修改数据库最大连接数据；

修改后

```shell
conn /as sysdba;--保存数据库
shutdown immediate; --关闭数据库
startup; --启动数据库
show user;
```

### 配置数据库客户端

如上，数据库服务器我们已经安装和配置好了，接下来就是要配置一下服务器的客户端

那么我们这个工程的服务端其实就是作为数据库的客户端(很绕)，所以我们需要配置一下这个客户端的环境

我们要用到的就是这个`Oracle Instant Client`

从官网中(要注册才能下载)
https://www.oracle.com/cn/database/technologies/instant-client/linux-x86-64-downloads.html

下载三个文件
`instantclient-basic-linux.x64-11.2.0.4.0.zip`
`instantclient-sdk-linux.x64-11.2.0.4.0.zip`
`instantclient-sqlplus-linux.x64-11.2.0.4.0.zip`

拷贝到opt目录底下(决定了下面环境变量的配置目录)

做两个软连接
```
ln -s libclntsh.so.11.1 libclntsh.so
ln -s libocci.so.11.1 libocci.so
```
在根目录下新建两级文件夹`/network/admin`
创建文件`tnsnames.ora`

写入
```
helowin =
(DESCRIPTION =
 (ADDRESS = (PROTOCOL = TCP)(HOST = 127.0.0.1)(PORT = 1521))
 (CONNECT_DATA =
  (SERVER = DEDICATED)
  (SERVICE_NAME = orcl)
 )
)

```

配置：`sudo vim /etc/profile`

```shell
export ORACLE_SID=helowin
# OCCI_HOME
export OCCI_HOME=/opt/instantclient_11_2
export OCCI_INCLUDE_DIR=$OCCI_HOME/sdk/include
export OCCI_LIBRARY_PATH=$OCCI_HOME
export LD_LIBRARY_PATH=$OCCI_HOME:$OCCI_LIBRARY_PATH
# 程序编译时搜索的库目录
export LIBRARY_PATH=$LIBRARY_PATH:$OCCI_LIBRARY_PATH
# 程序编译时搜索的头文件目录
export CPLUS_INCLUDE_PATH=$CPLUS_INCLUDE_PATH:$OCCI_INCLUDE_DIR
export PATH=$OCCI_HOME:$PATH
export TNS_ADMIN=$OCCI_HOME/network/admin


```


生效：`source /etc/profile`

验证：执行`sqlplus`

报错：缺少`libaio.so`

```shell
sqlplus: error while loading sharedlibraries: libaio.so.1: cannot open shared object file: No such file ordirectory
```

安装：`sudo apt-get install libaio*`

再次验证`sqlplus`

跳出登录界面即成功？只成功了一半。。。接下来才是噩梦的开始

上面跟着做基本上能规避掉一些问题，我再强调一下有哪些点

第一：配置环境变量的时候`export ORACLE_SID=helowin`这个双方不能少，docker里和本机里都要配置
第二：本机里的`tnsnames.ora`第一行是实例名，这个不能瞎写，我们这里是`helowin`
如何查询：通过docker进入到数据库`sqlplus / as sysdba`，输入`select instance_name from v$instance;`可以查到，这里要一致

```
INSTANCE_NAME
----------------
helowin

```

第三：上面两个软连接不要忘了，不然make编译时就会报错各种undefine 

```shell
ln -s libclntsh.so.11.1 libclntsh.so
ln -s libocci.so.11.1 libocci.so
```



最后一个就是终极问题了，上面的做完之后，应该可以make，然后执行之后，会报错

```shell
 ORA-24960: the attribute  OCI_ATTR_USERNAME is greater than the maximum allowable length of 255
```

最后查到了这两篇文章
https://www.coder.work/article/7822497
https://stackoverflow.com/questions/42890553/ubuntu-ora-24960-the-attribute-oci-attr-username-is-greater-than-the-maximum-al

因为occi需要g++版本低于4.8，然后我尝试了获取4.8版本的g++，但是包管理说找不到(shit！我还FQ更新了一下`sudo add-apt-repository ppa:ubuntu-toolchain-r/test`，还是找不到)
然后最终我在stackflow里看到了这句，`Also help to use -D_GLIBCXX_USE_CXX11_ABI=0 flag.`（stackflow yyds）

然后加进了`makefile`

```makefile
DEFINE=-D_GLIBCXX_USE_CXX11_ABI=0
```
至此，基本上就能跑通了。

完结


> 上面的记述可能有点混乱，没办法，这是我跑通之后回忆的，前后可能会比较乱，望谅解，如果碰到问题，可以试着从我上面总结的几个问题中找找解决方法。










