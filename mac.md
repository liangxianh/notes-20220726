### mac电脑初始化相关问题（部分丢失）

#### 1 安装homebrew

```
有关brew常用的指令如下：

1. brew搜索软件命令： brew search nginx
2. brew安装软件命令： brew install nginx
3. brew卸载软件命令: brew uninstall nginx
4. brew升级命令： sudo brew update
5. 查看安装信息(比如查看安装目录等) sudo brew info nginx
6. 查看已经安装的软件：brew list
```

1 mac环境安装nginx
```
安装：
brew install nginx

查看相关信息
brew info nginx

查看安装目录
open /opt/homebrew/etc/nginx

启动nginx服务，如下命令：
brew services start nginx // 重启的命令是: brew services restart nginx

启动之前需要先检测一下
nginx -t

重启nginx 的方法
1 brew services restart nginx

停止nginx方法
brew services stop nginx  
brew services kill nginx  
```
![image](https://user-images.githubusercontent.com/31762176/200459295-116dca39-a677-4020-8f93-e1f846a05539.png)

如上面的截图，From:xxx 这样的，是nginx的来源，
Docroot为 /opt/homebrew/var/www,
在/opt/homebrew/etc/nginx/nginx.conf 配置文件中默认的端口为8080，
且nginx将在/opt/homebrew/etc/nginx/servers/ 目录中加载所有文件。并且我们可以通过最简单的命令'nginx' 来启动nginx.

可以直接open /opt/homebrew/etc/ngxin 或者cd 路径找到文件进行编辑

[mac环境安装nginx参考文档1](https://www.cnblogs.com/tugenhua0707/p/9863885.html)

2 wget

Linux wget命令用来从指定的URL下载文件。wget非常稳定，它在带宽很窄的情况下和不稳定网络中有很强的适应性，如果是由于网络的原因下载失败，wget会不断的尝试，直到整个文件下载完毕。如果是服务器打断下载过程，它会再次联到服务器上从停止的地方继续下载。这对从那些限定了链接时间的服务器上下载大文件非常有用。

安装
```
brew install wget
```

语法
```
wget(选项)(参数)
```
选项
```
-a<日志文件>：在指定的日志文件中记录资料的执行过程；
-A<后缀名>：指定要下载文件的后缀名，多个后缀名之间使用逗号进行分隔；
-b：进行后台的方式运行wget；
-B<连接地址>：设置参考的连接地址的基地地址；
-c：继续执行上次终端的任务；
-C<标志>：设置服务器数据块功能标志on为激活，off为关闭，默认值为on；
-d：调试模式运行指令；
-D<域名列表>：设置顺着的域名列表，域名之间用“，”分隔；
-e<指令>：作为文件“.wgetrc”中的一部分执行指定的指令；
-h：显示指令帮助信息；
-i<文件>：从指定文件获取要下载的URL地址；
-l<目录列表>：设置顺着的目录列表，多个目录用“，”分隔；
-L：仅顺着关联的连接；
-r：递归下载方式；
-nc：文件存在时，下载文件不覆盖原有文件；
-nv：下载时只显示更新和出错信息，不显示指令的详细执行过程；
-q：不显示指令执行过程；
-nh：不查询主机名称；
-v：显示详细执行过程；
-V：显示版本信息；
--passive-ftp：使用被动模式PASV连接FTP服务器；
--follow-ftp：从HTML文件中下载FTP连接文件。
```
参数
URL：下载指定的URL地址。

3 
