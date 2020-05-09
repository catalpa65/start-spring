## Nginx基本概念
* Nginx是什么，做什么事情：Nginx是一个高性能的HTTP反向代理服务器，特点是占用内存小，并发能力强。
* 反向代理：说反向代理之前，要先说说正向代理，比如用户不能访问的网站（www.google.com），我们在客户端浏览器配置代理服务器
            (www.abc.com)，通过代理服务器进行互联网访问。代理用户。
            反向代理，比如我们只需要把请求发送给反向代理服务器，然后让反向代理服务器获取数据返回，暴露的是反向代理服务器
            客户端并不知道真实服务器，代理真实服务器。
* 负载均衡：单个服务器处理不了，我们增加服务器的数量，然后将请求分发给各个服务器上
* 动静分离：为了加快网站解析速度，可以把静态页面和动态页面由不同的服务器来解析，降低单个服务器压力

## Nginx软件安装、常用命令、配置文件
* 安装nginx：先安装一些依赖环境，然后安装nginx
* nginx常用命令：先进入nginx目录（/usr/local/nginx/sbin）
    *版本号，nginx -v
    *启动，nginx start
    *关闭，nginx -s stop
    *重新加载，nginx -s reload

* nginx配置文件:（/usr/local/nginx/conf） 
    *全局块，nginx服务器全局配置，比如worker_processes 1值越大，可以支持的并发处理越多
    *events块，主要影响nginx服务器与用户的网络连接，比如worker_connections 1024,最大网络连接数
    *http块（http全局块，文件引入，MIME-TYPE，日志，连接超时时间，单连接请求上限）
           （server块（全局server块）（location块））
## Nginx配置实例1-反向代理
* 实例一：我们想在浏览器输入 192.168.17.129：80  访问linux的tomcat主页 127.0.0.1：8080
  就需要配置nginx: server_name 192.168.17.129 listen 80
           proxy_pass   http://127.0.0.1:8080
* 实例二：访问127.0.0.1：9001/edu 跳转到127.0.0.1：8080
          访问127.0.0.1：9001/vod 跳转到127.0.0.1：8081
  就需要配置nginx: server_name 192.168.17.129 listen 9001
           location需要用正则过滤
           location ~ /edu/ {
            proxy_pass   http://127.0.0.1:8080
           }
           location ~ /vod/ {
            proxy_pass   http://127.0.0.1:8081
           }
           
## Nginx配置实例2-负载均衡
* 实例三：我们想在浏览器输入 192.168.17.129/edu/a.html 均衡负载到8080 8081端口上
    我们需要配置nginx。策略包含：轮询（默认），权重，IP Hash，far(根据响应时间)
    
        //负载均衡的服务列表
            upstream myserver{
                server 192.168.17.129:8080;
                server 192.168.17.129:8081;
            }
        //location里面加个属性
           location / {
            proxy_pass   http://myserver;
           }
## Nginx配置实例3-动静分离
* 将动态请求和静态请求分离，可以理解成使用nginx处理静态页面，tomcat处理动态页面
## Nginx高可用集群
* 可以借助keepalive做高可用，两台Nginx，一台主一台备份
## Nginx原理

