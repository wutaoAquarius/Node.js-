# git add . 时出现
```
warning: LF will be replaced by CRLF in config/default.js.  
The file will have its original line endings in your working directory.
```
解决方法：git config --global core.autocrlf true
方法来源：https://www.zhihu.com/question/50862500