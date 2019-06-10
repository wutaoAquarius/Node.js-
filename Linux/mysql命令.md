## mysql 截取字符串
https://www.cnblogs.com/lcngu/p/6200477.html  

 

## linux 下 mysql导出数据命令  
`https://www.cnblogs.com/0201zcr/p/5846153.html`

```
--replace : 将insert 替换为 replace
-t : 去除表结构
--compact: 压缩导出文件
--where: 按条件导出
```

### 全表导出
`mysqldump -u root -pPwd Database table > filename.sql`
### 有条件导出
`mysqldump -u root -pPwd Database 表名 --where "where条件" > filename.sql`
### 导出replace sql
`mysqldump -u USER -pPwd Database table --replace -t --compact --where "where条件" > filename.sql`  

* 

## linux 下 mysql 导出Excel命令
### 全表导出
`mysql -u USER -pPwd -hMysql服务器IP Database -e table > filename.csv`

### 带条件导出
`mysql -u USER -pPwd -hMysql服务器IP Database -e "sql语句" > 文件路径/文件.xls`



