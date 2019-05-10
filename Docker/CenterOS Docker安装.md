# CenterOS Docker 安装  
## 前提条件
目前，CentOS 仅发行版本中的内核支持 Docker。  

Docker 运行在 CentOS 7 上，要求系统为64位、系统内核版本为 3.10 以上。  

Docker 运行在 CentOS-6.5 或更高的版本的 CentOS 上，要求系统为64位、系统内核版本为 2.6.32-431 或者更高版本。  

## 使用yum安装 (CenterOS 7)
Docker 要求 CentOS 系统的内核版本高于 3.10 ，查看本页面的前提条件来验证你的CentOS 版本是否支持 Docker 。  

通过 uname -r 命令查看你当前的内核版本  
`[root@runoob ~]# uname -r`  

### 安装Docker  
1. 移除旧的版本：  
(```)
$ sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine
(```) 
2. 安装一些必要的系统工具：
(```)
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
(```)
3. 添加软件源信息
`sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo`
4. 更新 yum 缓存
`sudo yum makecache fast`
5. 安装 Docker-ce:
`sudo yum -y install docker-ce`
6. 启动 Docker 后台服务
`sudo systemctl start docker`
7. 测试运行 hello-world
`docker run hello-world`