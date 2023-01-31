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
