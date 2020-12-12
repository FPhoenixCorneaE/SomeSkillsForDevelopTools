

Git clone 大文件代码，造成克隆中断，retry多次还是出现如下问题：

	$ git clone https://github.com/boostorg/boost.git
	Cloning into 'boost'...
	remote: Counting objects: 183543, done.
	remote: Compressing objects: 100% (69361/69361), done.
	fatal: The remote end hung up unexpectedly
	fatal: early EOF
	fatal: index-pack failed

​	

### 解决方案一：（通过命令来完成）

#### 为 git 添加配置项，通过下面的命令可以简单完成
#### 在这之前可以执行 git config -l 命令看看已有配置项有哪些
git config --add core.compression -1



### 解决方案二：（通过修改 .gitconfig 文件，该文件在用户目录下）

[user]
    name = ...
    email = ...
[core]
    compression = -1
	
	
	
	
	
compression 是压缩的意思，从 clone 的终端输出就知道，服务器会压缩目标文件，
然后传输到客户端，客户端再解压。取值为 [-1, 9]，-1 以 zlib 为默认压缩库，
0 表示不进行压缩，1..9 是压缩速度与最终获得文件大小的不同程度的权衡，数字越
大，压缩越慢，当然得到的文件会越小。