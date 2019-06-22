# Redis 安装
由于我主要使用 window系统以及CenterOs系统，所以只记录window、Linux的安装教程,完整教程如下网址：
https://www.runoob.com/redis/redis-install.html

## Window 安装
下载地址：https://github.com/MSOpenTech/redis/releases。
Redis 支持 32 位和 64 位。这个需要根据你系统平台的实际情况选择，这里我们下载 Redis-x64-xxx.zip压缩包到 C 盘，解压后，将文件夹重新命名为 redis
我是安装在D盘，下载后，解压文件如下：
![图片来自菜鸟教程](https://www.runoob.com/wp-content/uploads/2014/11/C2CEBAA0-30B9-4340-8D23-78F6FEB8CBE2.png%22)

打开一个 cmd 窗口 使用 cd 命令切换目录到 C:\redis 运行：
`redis-server.exe redis.windows.conf`

![图片来自菜鸟教程](https://www.runoob.com/wp-content/uploads/2014/11/redis-install1.png)

这时候另启一个 cmd 窗口，原来的不要关闭，不然就无法访问服务端了。
切换到 redis 目录下运行
`redis-cli.exe -h 127.0.0.1 -p 6379`  
设置键值对:
`set myKey abc`
取出键值对:
`get myKey`

![图片来自菜鸟教程](https://www.runoob.com/wp-content/uploads/2014/11/redis-install2.jpg)

可以在将Redis 加入系统环境变量，这样在就无须每次输入Redis目录路径
![](https://img-blog.csdn.net/2018060918221379?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjQyMzgxOQ==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

打开一个cmd窗口，执行redis-server.exe就可以启动redis了，因为执行redis-server.exe redis.windows.conf，这时会报错Invalid argument during startup: Failed to open the .conf file: redis.windows.conf CWD=C:\Users\Administrator

## linux 安装
下载地址：http://redis.io/download，下载最新稳定版本。
本教程使用的最新文档版本为 2.8.17，下载并安装：
```shell
$ wget http://download.redis.io/releases/redis-2.8.17.tar.gz
$ tar xzf redis-2.8.17.tar.gz
$ cd redis-2.8.17
$ make
```

make完后 redis-2.8.17目录下会出现编译后的redis服务程序redis-server,还有用于测试的客户端程序redis-cli,两个程序位于安装目录 src 目录下：
下面启动redis服务.
```shell
$ cd src
$ ./redis-server
```

redis.conf 是一个默认的配置文件。我们可以根据需要使用自己的配置文件。
启动redis服务进程后，就可以使用测试客户端程序redis-cli和redis服务交互了。 比如：
```shell
$ cd src
$ ./redis-cli
redis> set foo bar
OK
redis> get foo
"bar"
```

## Redis 配置
https://www.runoob.com/redis/redis-conf.html
