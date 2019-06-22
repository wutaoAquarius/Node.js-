## cmd常用命令之创建文件
https://blog.csdn.net/sunjinshengli/article/details/53557684

## 创建文件
type null>path/XXX.后缀  
cd.>a.txt  
copy nul a.txt 
echo a 3>a.txt  
fsutil file createnew d:\a.txt 0  

## window下暴力结束所有node.exe进程
taskkill /fi "imagename eq node.exe" /f