


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

原因总结
1 web---pod（nginx）----- api.server.com (多个ip)
若nginx缓存了某个ip了，但是该ip不服务了就会造成这种情况

尝试的解决方法在.conf文件内部配置
```
 resolver  8.8.8.8  valid=10s;
```

## nginx 基础知识整理

1 http反向代理：作为web服务器

正向代理：是代理客户端的，我门知道目标服务器的链接，但是无法直接访问目标服务器，必须通过代理的方式（比如翻墙访问google）

反向代理：是代理服务端的，客户端对代理是无感知的类似nginx接口转发

[参考文章](https://juejin.cn/post/6865213076174536712)



