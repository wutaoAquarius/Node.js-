## 命令参考地址：  
```
https://blog.csdn.net/ydfok/article/details/1486451
```

## 常用命令
### 根据名称查询文件:  
```
find 文件路径 -name 文件名称
```

### 指定时间查询文件 
 
```
(find /website/costa-dist/webapi/NLogs -type f -mtime +2 -exec ls -l {} \;) | grep InterfaceCall.log
```
###