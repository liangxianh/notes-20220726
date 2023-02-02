


1 nginx配置/相关问题

```
第一个版本，稳定运行近1个半月
    location /publicPath {
        proxy_pass https://api.*.com;
    }
第二个版本，稳定运行近1个半月
    location /publicPath/ {
        proxy_pass https://api.*.com/publicPath/;
     }
第三个版本，直接404,out
     location /publicPath {
        proxy_pass https://api.*.com/;
     }
最终版本
    location /publicPath/ {
        proxy_pass https://api.*.com;
    }
```
针对第一个出现的现象是：
* 稳定运行一段时间后404
* pending---- canceled
* 需要登录网站域名
 ![image](https://user-images.githubusercontent.com/31762176/215442920-0aa02085-cad0-410f-8b82-ebdcf3f95394.png)

针对第二个出现的现象有四种

* pending ---- canceled
* 503
* 302 ---- 被重定向的地址存在跨域问题
  ![image](https://user-images.githubusercontent.com/31762176/215443403-a7e7b677-2612-49e3-9646-468d8a75923a.png)
* 301
![image](https://user-images.githubusercontent.com/31762176/215443551-9e839abf-9f51-4c26-8c4e-3ba49da7ad4d.png)



Nginx配置proxy_pass转发的 / 路径问题
在nginx中配置proxy_pass时，如果是按照^~匹配路径时，要注意 proxy_pass 后的 url 最后的 / :

* 当加上了 /，相当于绝对根路径，则 nginx 不会把 location 中匹配的路径部分代理走
* 如果没有 /，相当于相对路径，则会把匹配的路径部分也给代理走

示例

> 有 /
```
location /publicPath/ {
    proxy_pass https://api.*.com/;
 }
```
如上面的配置，如果请求的 url 是 http://your_domain/publicPath/add，会被代理成 http://api.*.com/add

> 无 /
```
location /publicPath/ {
    proxy_pass https://api.*.com;
 }
```
如上面的配置，如果请求的 url 是 http://your_domain/publicPath/add，会被代理成 http://api.*.com/publicPath/add

当然，我们可以用如下的 rewrite 来实现 / 的功能：
```
location /publicPath/ {
    proxy_pass https://api.*.com;
    rewrite: (path) => path.replace(/^\/api/, ''),
 }
 
  以本地链接测试环境进行测试。。
 ```
        # 405
        location  /publicPath {
          proxy_pass https://test.*.com/;
        }

        # 405
        location  /publicPath/ {
          proxy_pass https://test.*.com/;
        }
        # 不能访问的  m.*.com/publicPath/v1/add ---> https://test.*.com/v1/add
        
        # 200正常访问
        location  /publicPath {
          proxy_pass https://test.*.com;
        }

        # 200正常访问
        location  /publicPath/ {
          proxy_pass https://test.*.com;
        }

        # 200正常访问
        location  /publicPath/ {
          proxy_pass https://test.*.com/publicPath/;
        }
        # 正常访问的  m.*.com/publicPath/v1/add ---> https://test.*.com/publicPath/v1/add
 ```

可能原因总结 
1 web--->pod（nginx）-----> api.server.com (多个ip)
若nginx缓存了某个ip了，但是该ip不服务了就会造成这种情况
验证:进入终端即nginx上；
利用dig或者nslookup命令看看配置的域名IP是不是一直在变

![image](https://user-images.githubusercontent.com/31762176/216212121-8e1d2e5d-a6a3-4f2c-9bfe-48bec74b6ee2.png)
输入
nslookup baidu.com

输出部分:
最上面的 Server 和 Address 是该词查询的 DNS 服务器。可以自己指定，也可以默认。
默认情况下 DNS 服务器的端口为53
非权威应答（Non-authoritative answer）意味着answer来自于其他服务器的缓存，而不是权威的Baidu DNS服务器。缓存会根据 ttl（Time to Live）的值定时的进行更新。这个链接或许对你有所帮助: [What is the meaning of the non-authoritative-answer?](https://serverfault.com/questions/413124/dns-nslookup-what-is-the-meaning-of-the-non-authoritative-answer)
注意到，在对http://google.com进行查询时，Mac 返回的结果并没有非权威服务器提示，而Windows下的命令却提示了，这是因为 8.8.8.8 这个 DNS 服务器正是谷歌的权威名字服务器，可以使用nslookup -ty=ptr 8.8.8.8验证，ptr也是一种记录类型，可以用进行反向DNS解析(Reverse DNS Lookup)，拓展链接: reverse-dns-lookup

缓存会根据 ttl（Time to Live）的值定时的进行更新

[nslookup参考](https://zhuanlan.zhihu.com/p/361451835)

尝试的解决方法在.conf文件内部配置  resolver  8.8.8.8  valid=10s;

```
?????????

server {
    server_name www.*.com;

    resolver  8.8.8.8  valid=10s;
    
    server_tokens off;
    
    gzip on;
    gzip_static on;

    location /publicPath/ {
        proxy_pass https://api.*.com;
    }

    location / {
        root /usr/share/nginx/html;
        index  index.html index.htm;
        try_files $uri $uri/ /index.html;
    }                         
} 
```
<b>使用Nginx resolver注意点
使用 resolver 功能，通过 resolver 这种方式来实现nginx动态解析代理域名，相当于放弃了upstream，也就无法使用upstream相关配置功能，比如回话保持、健康检测等等
针对如下妖怪问题，建议严谨测试后再上线。</b>

[参考文章](https://www.nginx-cn.net/products/nginx/load-balancing/)


## nginx 基础知识整理

1 http反向代理：作为web服务器

正向代理：是代理客户端的，我门知道目标服务器的链接，但是无法直接访问目标服务器，必须通过代理的方式（比如翻墙访问google）

反向代理：是代理服务端的，客户端对代理是无感知的类似nginx接口转发

[参考文章1](https://juejin.cn/post/6865213076174536712)
[参考文章2](https://www.cnblogs.com/tugenhua0707/p/9863885.html)


