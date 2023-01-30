### mac 电脑运行和配置nginx


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


