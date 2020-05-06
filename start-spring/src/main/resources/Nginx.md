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

## Nginx配置实例2-负载均衡

## Nginx配置实例3-动静分离

## Nginx高可用集群

## Nginx原理

