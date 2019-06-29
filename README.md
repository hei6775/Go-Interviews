# Interviews

Golang 面试知识点总结

主要集中于 golang 的面试，建议准备三份源码分析以便面试，顺带刷一刷 leetcode 会有奇效哦，不定期更新

本人能力有限，欢迎各位指正

<div align="center">
    <a href="https://golang.google.cn/"><img src="https://badgen.net/badge/golang/loading/blue?icon=zeit"></a>
    <a href="https://github.com/hei6775/Algorithm"><img src="https://badgen.net/badge/hei6775/Blogs/blue?icon=deepscan">
</div>

<br>

| &nbsp;[算法](#Algorithm-算法)&nbsp; | [操作系统](#System-操作系统) | &nbsp;[网络](#Net-网络)&nbsp; | &nbsp;[数据库](#DB-数据库) | &nbsp;[Golang](#Golang-Go)&nbsp; | &nbsp;[面试](#Interviews-面试)&nbsp; | &nbsp;[其它](#Others-其它)&nbsp; |
| :---------------------------------: | :--------------------------: | :---------------------------: | :------------------------: | :------------------------------: | :----------------------------------: | :------------------------------: |


## Algorithm 算法

## System 系统

## Net 网络

## DB 数据库

## Golang Go

## Interviews 面试

## Others 其它

# 每天一道面试题

# 常见问题

### 1. 浏览器的一个请求从发送到返回都经历了什么，讲的越详细越好

我大概讲下我的答案：

1、先从网络模型层面：

首先 client 通过 DNS 协议获取目标域名的 IP，然后 client （浏览器）与 server 通过 http 协议通讯，http 协议属于应用层协议，http 基于 tcp 协议，所以 client 与 server 主要通过 socket 进行通讯；

而 tcp 属于传输层协议、如果走 https 还需要会话层 TLS、SSL 等协议； 传输层之下网络层，这里主要是路由协议 OSPF 等进行路由转发之类的。再向下数据链路层主要是 ARP、RARP 协议完成 IP 和 Mac 地址互解析，再向下到最底层物理层基本就是 IEEE 802.X 等协议进行数据比特流转成高低电平的的一些定义等等；

当浏览器发出请求，首先进行数据封包，然后数据链路层解析 IP 与 mac 地址的映射，然后上层网路层进行路由查表路由，通过应用层 DNS 协议得到目标地址对应的 IP ，在这里进行 n 跳的路由寻路；而传输层 tcp 协议可以说下比较经典的三次握手、四次分手的过程和状态机。

2、应用层方面：

数据交换主要通过 http 协议， http 协议是无状态协议，这
里可以谈一谈 post、get 的区别以及 RESTFul 接口设计，然
后可以讲服务器 server 模型 epoll、select 等，接着可以根据实际经验讲下 server 处理流程，比如我： server 这边 Nginx 拿到请求，进行一些验证，比如黑名单拦截之类的，然后 Nginx 直接处理静态资源请求，其他请求 Nginx 转发给后端服务器，这里我用 uWSGI, 他们之间通过 uwsgi 协议通讯，uWSGI 拿到请求，可以进行一些逻辑， 验证黑名单、判断爬虫等，根据 wsgi 标准，把拿到的 environs 参数扔给 Django ，Django 根据 wsgi 标准接收请求和 env， 然后开始 start_response ，先跑 Django 相关后台逻辑，Django 拿到请求执行 request middleware 内的相关逻辑，然后路由到相应 view 执行逻辑，出错执行 exception middleware 相关逻辑，接着 response 前执行 response middleware 逻辑，最后通过 wsgi 标准构造 response， 拿到需要返回的东西，设置一些 headers，或者 cookies 之类的，最后 finish_response 返回，再通过 uWSGI 给 Nginx ，Nginx 返回给浏览器。
