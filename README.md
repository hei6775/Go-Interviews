# Interviews

Golang 面试知识点总结，不定期更新

# 金山办公 金山云

1. 介绍一下 Golang 的调度模式
2. osi 七层模型分别是什么？
3. select 用过吗？当多个 case 满足条件的时候，会怎么执行呢？
4. 使用 mysql 怎么实现高并发访问不挂掉？
5. 介绍一下你的项目构成，并发量如何？数据量如何？项目中碰到的最大的困难是什么？如何解决？
6. 之前的工作中，你觉得有什么让你觉得自豪的东西？
7. http 中，request 中的 body 是发送时就可以读取，还是要发送完成？假如发送量很大呢，怎么读取？
8. 你们使用什么方式读取 request 的 body？（答：ioutil。感觉面试官不是很满意，但是自己确实没有踩到这个坑）
9. json.unmarshl 使用过吗？解析到 map[string]string 时，int 型的数字会怎么样？（不知道）
10. 说说对微服务的理解
11. redis 有几种数据格式？
12. 你是如何设计一张数据库的？
13. 从浏览器输入 baidu.com 到响应发生了什么，越详细越好
14. golang 是怎么把来自客户端的请求解析到`r *request`中去的
15. 快速排序法的实现思路以及时间复杂度
16. 解释面向对象编程和面向过程编程，并说明各自的利弊
17. 说明 http1.0,1.1,2 的主要区别
18. mongodb 如何分析查询效率
19. 如何定位 http 状态码错误的问题

# 晓信科技

1. 介绍 golang 的调度模型
2. osi 七层模型并说明每层的作用
3. 介绍一下 Mongodb，并说明 mongodb3.0 和 4.0 的区别
4. osi 七层模型是怎么保证可靠的数据传输服务呢？

# 常见问题

1. 浏览器的一个请求从发送到返回都经历了什么，讲的越详细越好

```
我大概讲下我的答案：

1、先从网络模型层面：

client （浏览器）与 server 通过 http 协议通讯，http 协议属于应用层协议，http 基于 tcp 协议，所以 client 与 server 主要通过 socket 进行通讯；

而 tcp 属于传输层协议、如果走 https 还需要会话层 TLS、SSL 等协议； 传输层之下网络层，这里主要是路由协议 OSPF 等进行路由转发之类的。再向下数据链路层主要是 ARP、RARP 协议完成 IP 和 Mac 地址互解析，再向下到最底层物理层基本就是 IEEE 802.X 等协议进行数据比特流转成高低电平的的一些定义等等；

当浏览器发出请求，首先进行数据封包，然后数据链路层解析 IP 与 mac 地址的映射，然后上层网路层进行路由查表路由，通过应用层 DNS 协议得到目标地址对应的 IP ，在这里进行 n 跳的路由寻路；而传输层 tcp 协议可以说下比较经典的三次握手、四次分手的过程和状态机，这里放个图可以作为参考：



2、应用层方面：

数据交换主要通过 http 协议， http 协议是无状态协议，这里可以谈一谈 post、get 的区别以及 RESTFul 接口设计，然后可以讲服务器 server 模型 epoll、select 等，接着可以根据实际经验讲下 server 处理流程，比如我： server 这边 Nginx 拿到请求，进行一些验证，比如黑名单拦截之类的，然后 Nginx 直接处理静态资源请求，其他请求 Nginx 转发给后端服务器，这里我用 uWSGI, 他们之间通过 uwsgi 协议通讯，uWSGI 拿到请求，可以进行一些逻辑， 验证黑名单、判断爬虫等，根据 wsgi 标准，把拿到的 environs 参数扔给 Django ，Django 根据 wsgi 标准接收请求和 env， 然后开始 start_response ，先跑 Django 相关后台逻辑，Django 拿到请求执行 request middleware 内的相关逻辑，然后路由到相应 view 执行逻辑，出错执行 exception middleware 相关逻辑，接着 response 前执行 response middleware 逻辑，最后通过 wsgi 标准构造 response， 拿到需要返回的东西，设置一些 headers，或者 cookies 之类的，最后 finish_response 返回，再通过 uWSGI 给 Nginx ，Nginx 返回给浏览器。
```
