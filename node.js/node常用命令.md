#搭建一个node.js项目
###参考网址：https://www.jianshu.com/p/a6d430a00242

##初始化项目 创建package.json文件
npm init 

##安装express框架，轻量灵活的node.js Web应用框架”。它可以帮助你快速搭建web应用。
npm install express --save

----

#使用express快速创建node项目
###Express是基于Nodejs的一个快速web开发框架,它提供了一个Express application generator可以帮助我们快速创建应用的骨架。
###资料参考网址：http://mclspace.com/2016/01/18/nodejs-tutorials-1/
##使用如下命令安装generator
npm install express-generator -g

##安装完成后用以下命令生成骨架,-e选项代表使用EJS模板引擎:
express -e 项目名称

生成后的应用结构如下：
.
├── LICENSE
├── README.md
├── app.js
├── bin
│   └── www
├── package.json
├── public
│   ├── images
│   ├── javascripts
│   └── stylesheets
│       └── style.css
├── routes
│   ├── index.js
│   └── users.js
└── views
    ├── error.ejs
    └── index.ejs
7 directories, 10 files

app.js是express的设置文件
bin/www是express执行文件
package.json是nodejs项目的配置文件，用于保存应用信息与依赖管理
public文件夹为web应用的资源文件夹
routes保存路由文件
views保存网站的ejs视图代码

##安装项目依赖
npm install

##启动项目
npm start 或者直接 node ./bin/www 启动项目

