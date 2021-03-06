## 1、解压rar压缩文件 ##

Ubuntu刚安装的时候是不能解压rar文件的，只有在安装了解压工具之后，才可以解压。

安装：sudo apt-get install unrar

卸载：sudo apt-get remove unrar

unrar常用命令主要有:

e 将文件解压到当前目录
  例:rar e test.rar
    注:用e解压的话，不仅原来的file1.txt和file2.txt被解压到当前目录，就连dir1里面的所有文件
    也被解压到当前目录下，不能保持压缩前的目录结构，如果想保持压缩前的目录结构，用x解压

x 带路径解压文档中内容到当前目录
  例:rar x test.rar
    这样解压的话，dir1就会保持原来的目录结构

a 添加文件到操作文档
例:rar a test.rar file1.txt 若test.rar文件不存在，则打包file1.txt文件成test.rar
    例:rar a test.rar file2.txt 若test.rar文件已经存在，则添加file2.txt文件到test.rar中
    (这样test.rar中就有两个文件了）
    注，如果操作文档中已有某文件的一份拷贝，则a命令更新该文件，对目录也可以进行操作
  例:rar a test.rar dir1

c 对操作文档添加说明注释
  rar c test.rar
    （会出现Reading comment from stdin字样，然后输入一行或多行注释，以ctrl+d结束）
    cf 添加文件注释，类似上面的c，不过这个是对压缩文档中每个文件进行注释
    cw 将文档注释写入文件
    例:rar cw test.rar comment.txt

d 从文档中删除文件
    例:rar d test.rar file1.txt

k 锁定文档
  例:rar k test.rar 锁定文档后，该文档就无法进行任何更新操作了

r 修复文档
  例:rar r test.rar
    当rar文件有问题时，可以尝试用该命令进行修复（鬼知道有多少用）

s 转换文档成自解压文档
  例:rar s test.rar
    会生成一个test.sfx的可执行文档，运行它的效果就相当于rar x test.rar，
    适合于向没有rar的用户传输文件

t 检测文档
  例:rar t test.rar
    检测test.rar的完整性，一般压缩完大型文件准备传输前最好用这个命令来确保文件的正确性

##2、安装指定版本node##

下载指定版本压缩包（[https://nodejs.org/download/release/](https://nodejs.org/download/release/)）

通过命令下载到指定文件夹

    wget https://nodejs.org/download/release/v6.14.1/node-v6.14.1-linux-x64.tar.gz /home/victor

解压文件压缩包

	tar -xvf /home/victor/node-v6.14.1-linux-x64.tar.gz

如果找不到tar命令，说明没有装对应的包，通过如下命令安装：

	sudo apt-get install unrar

利用`ln`命令安装（`ln`命令为某一个文件或目录在另外一个位置建立一个同步的链接）

	sudo ln -s /home/victor/node-v6.14.1-linux-x64/bin/node /usr/bin/node

	sudo ln -s /home/victor/node-v6.14.1-linux-x64/bin/npm /usr/bin/npm

通过以下命令查看版本，是否安装成功：

	node -v
	
	npm -v

##3、防火墙操作##

1、关闭ubuntu的防火墙

	ufw disable

2、开启防火墙

	ufw enable

3、卸载了iptables

	apt-get remove iptables

4、关闭ubuntu中的防火墙的其余命令

    iptables -P INPUT ACCEPT
    iptables -P FORWARD ACCEPT
    iptables -P OUTPUT ACCEPT
    iptables -F