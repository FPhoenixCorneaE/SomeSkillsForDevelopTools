

使用 cmd 命令批处理获取文件夹下文件完整路径步骤：

1、在文件夹下新建文本文档，输入一下内容：

	@echo off & setlocal EnableDelayedExpansion  
	for /f "delims=" %%i in ('"dir /a/s/b/on *.*"') do (  
	set file=%%~fi  
	set file=!file:/=/!  
	echo !file! >> 路径.txt  
	) 
	
	然后保存；

2、把新建文本文档扩展后缀名.txt改为.bat;

3、双击运行.bat文件，命令完成将生成文档>>路径.txt文件。