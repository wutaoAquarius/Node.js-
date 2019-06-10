# Redis linux设置守护进程
参考网址
```
https://www.cnblogs.com/onephp/p/6245902.html
https://www.cnblogs.com/xuzhengzong/p/7676745.html
```

### Redis安装 
```shell
wget http://download.redis.io/releases/redis-5.0.5.tar.gz
tar xzf redis-5.0.5.tar.gz
cd redis-5.0.5
make
```

### 创建存储Redis 文件目录
```shell
cd ./src
mkdir -p /usr/local/redis/
cp ./redis-server /usr/local/redis/
cp ./redis-cli /usr/local/redis/
cp ../redis.conf /usr/local/redis/
```

### 编辑redis配置文件
```
vim /usr/local/redis/redis.conf
```
将配置文件中的 daemonize 改为yes，daemonzie默认为no  
daemonize介绍:`https://www.jianshu.com/p/5a187bfd4a06`
1. daemonize介绍
  A、redis.conf配置文件中daemonize守护线程，默认是NO。
  B、daemonize是用来指定redis是否要用守护线程的方式启动。
2. daemonize 设置yes或者no区别
  daemonize:yes:redis采用的是单进程多线程的模式。当redis.conf中选项daemonize设置成yes时，代表开启守护进程模式。在该模式下，redis会在后台运行，并将进程pid号写入至redis.conf选项pidfile设置的文件中，此时redis将一直运行，除非手动kill该进程。
  daemonize:no: 当daemonize选项设置成no时，当前界面将进入redis的命令行界面，exit强制退出或者关闭连接工具(putty,xshell等)都会导致redis进程退出。

### 添加开机启动服务
```
vim /etc/systemd/system/redis-server.service
```
内容
```shell
[Unit]
Description=The redis-server Process Manager
After=syslog.target network.target

[Service]
Type=simple
PIDFile=/var/run/redis_6379.pid
ExecStart=/usr/local/redis/redis-server /usr/local/redis/redis.conf         
ExecReload=/bin/kill -USR2 $MAINPID
ExecStop=/bin/kill -SIGINT $MAINPID

[Install]
WantedBy=multi-user.target
```
***关于 服务相关参数对应的意思,请查看 https://github.com/wutaoAquarius/Technical-Notes/blob/master/Linux/%E7%BC%96%E5%86%99systemd%20%E6%9C%8D%E5%8A%A1%E8%84%9A%E6%9C%AC.md***

### 设置开机自启
```
systemctl daemon-reload 
systemctl start redis-server.service 
systemctl enable redis-server.service
```
验证是否安装成功
```
ps -aux | grep redis
```
创建软连接
```
ln -s /usr/local/redis/redis-cli /usr/bin/redis
```
此时通过redis能够进入到 redis的命令台；


### 问题
在使用 service redis-server restart 后，redis重启失败，并且无法再 通过 redis 进入到 redis命令台；
此时使用 ps -aux | grep redis 会redis进程并未打开
使用 vim /var/run/redis_6379.pid 会发现此文件未创建
结合 https://www.cnblogs.com/xuzhengzong/p/7676745.html 来看，原因应该是 redis先前并未设置启动脚本

解决方法：
1. 将文件 redis文件 cp 到/usr/local 目录下
```shell
# rm -rf /usr/local/redis # 也可以不执行
cp -r /root/redis-5.0.5 /usr/local/
cd /usr/local/redis-5.0.5
```
2. 在/etc/redis/目录下建立一个 redis.conf 的软连接，名称为6379.conf,redis启动脚本会自动读取这个脚本文件
```
mkdir -p /etc/redis
ln -s /usr/local/redis-5.0.5/redis.conf /etc/redis/6379.conf
```
3. 修改redis启动脚本参数
```shell
vim utils/redis_init_script
```
修改前
```shell
REDISPORT=6379
EXEC=/usr/local/bin/redis-server
CLIEXEC=/usr/local/bin/redis-cli

PIDFILE=/var/run/redis_${REDISPORT}.pid
CONF="/etc/redis/${REDISPORT}.conf"
```
修改后
```
REDISPORT=6379
EXEC=/usr/local/redis-5.0.5/src/redis-server
CLIEXEC=/usr/local/redis-5.0.5/src/redis-cli

PIDFILE=/var/run/redis_${REDISPORT}.pid
CONF="/etc/redis/${REDISPORT}.conf"
```
4. 修改软连接地址
```
rm /usr/bin/redis 
ln -s /usr/local/redis-5.0.5/src/redis-cli /usr/bin/redis
```
5. 修改开机启动服务
```
vim /etc/systemd/system/redis-server.service
```
内容
```shell
[Unit]
Description=The redis-server Process Manager
After=syslog.target network.target

[Service]
Type=simple
ExecStart=/usr/local/redis-5.0.5/redis-server
ExecReload=/bin/kill -USR2 $MAINPID
ExecStop=/bin/kill -SIGINT $MAINPID

[Install]
WantedBy=multi-user.target
```
6. 设置开机自启
```
systemctl daemon-reload 
systemctl start redis-server.service 
systemctl enable redis-server.service
```
验证是否安装成功
```
ps -aux | grep redis
```
创建软连接
```
ln -s /usr/local/redis/redis-cli /usr/bin/redis
```
此时通过redis能够进入到 redis的命令台；
并且使用 restart 后之前出现的问题并未再次出现，使用stop后，在使用redis命令无法进入redis命令台

### 测试重启服务器 redis是否自启
```
reboot
```
等待大约15秒左右重启成功
```
[root@VM_0_7_centos ~]# ps -aux | grep redis
root       904  0.1  0.2 153892  8128 ?        Ssl  15:59   0:00 /usr/local/redis-5.0.5/src/redis-server *:6379
root      1441  0.0  0.0 112708   980 pts/0    S+   15:59   0:00 grep --color=auto redis
```
redis 进程自启成功；

### 守护进程安装完成
