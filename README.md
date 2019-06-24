# Interviews

Golang 面试知识点总结

主要集中于 golang 的面试，建议准备三份源码分析以便面试，顺带刷一刷 leetcode 会有奇效哦，不定期更新

本人能力有限，欢迎各位指正

# 金山办公 金山云

1. 介绍一下 Golang 的调度模式

   &emsp;&emsp;答：因为`golang`想要实现高并发所以采取的 M:N 的内核线程与用户线程的印射，即一个用户线程可以对应多个内核线程，
   一个内核线程也可以对应多个用户线程，而且为了便于管理 golang 的 GC，需要 golang 自己实现 goroutine 的调度。
   golang 的调度系统主要基于 M-P-G 的结构，M 是内核线程，P 是上下文管理器，G 是 goroutine 也就是需要被调度的任务，
   G 需要绑定 P 才能被 M 执行，当使用一个 go 关键字调起一个 goroutine 时，底层通过调用 newproc 生成一个新的调度任务，放入 GlobalRunningQueue 或者 LocalRunningQueue 中等待调度。当某个 P 阻塞的时候，该 P 下的 G 就会被放到其他的 P 中执行，如果
   P 中的 G 执行完成，那么这个 P 会从 GlobalRunningQueue 中获取 G 或者从其他的 P 中偷一半的 G 来执行，当然 P 也会定期检测 GlobalRunningQueue，防止 G 不被调用，P 不会饿死。

2. osi 七层模型分别是什么？

   &emsp;&emsp;答：从下到上分别是：

&emsp;&emsp;物理层：主要进行比特流的透明传输和高低电平的定义，为上层屏蔽具体的物理通信介质。

&emsp;&emsp;数据链路层：在节点之间建立数据链路连接，通过 ARP 和 RARP 协议进行 MAC 地址和 IP 的转换，传输以“帧”为单位的数据包，并且提供差错检测功能。

&emsp;&emsp;网络层：IP 协议就在这一层，通过路由选择协议（OSPF）为通信子网选择合适的路径，实现拥塞控制、网络互连等功能，路由选择最小单位——分组（包）报文。

&emsp;&emsp;传输层:TCP，UDP 协议就在这一层，传输层主要是为两个节点建立传输连接，提供可靠和非可靠的传输服务（传输控制协议，用户报数据协议）。

&emsp;&emsp;会话层：SSL，TTL 就在这一层，主要是负责两个节点之间的建立和解除

&emsp;&emsp;表示层：确保一个系统的应用层发送的消息可以被另一个系统的应用层读取；数据格式变换；数据加密与解密；数据压缩与恢复

&emsp;&emsp;应用层：主要是主机内的进程建立通讯，为应用程序提供网络服务。

3. select 用过吗？当多个 case 满足条件的时候，会怎么执行呢？

   &emsp;&emsp;答：随机执行。

4. 使用 mysql 怎么实现高并发访问不挂掉？

   &emsp;&emsp;答：

5. 介绍一下你的项目构成，并发量如何？数据量如何？项目中碰到的最大的困难是什么？如何解决？

6. 之前的工作中，你觉得有什么让你觉得自豪的东西？

7. http 中，request 中的 body 是发送时就可以读取，还是要发送完成？假如发送量很大呢，怎么读取？

8. 你们使用什么方式读取 request 的 body？（答：ioutil。感觉面试官不是很满意，但是自己确实没有踩到这个坑）

9. json.unmarshl 使用过吗？解析到 `map[interface{}]interface{}`时，`int`型的数字会怎么样？（不知道）

&emsp;&emsp;答：会被转换为 `float64`类型。

10. 说说对微服务的理解

11. redis 有几种数据格式？

&emsp;&emsp;答：

&emsp;&emsp;&emsp;&emsp;string 类型，对应的就是 key-value。

&emsp;&emsp;&emsp;&emsp;hash 类型，类似 golang 中的 map，一个 Key 可以放一个对象

&emsp;&emsp;&emsp;&emsp;list 类型，string 类型的列表

&emsp;&emsp;&emsp;&emsp;set 类型，是 string 类型的无序集合，通过哈希表实现

&emsp;&emsp;&emsp;&emsp;zset 类型，string 类型的有序集合，跳跃表实现

12. 你是如何设计一张数据库的？
13. 从浏览器输入 baidu.com 到响应发生了什么，越详细越好
14. golang 是怎么把来自客户端的请求解析到`r *request`中去的？

&emsp;&emsp;答：首先我们要知道一个 HTTP 请求是怎么样的。它是由请求行、
请求头、空行、请求主体构成。首先解析请求行，根据`空格`区分以请求方法，URI，协议版本进行读取。
然后请求头将字段与值根据：区分，有多个值的时候以`，`隔开。
随后是空行，将请求头和请求主体分开。

15. 快速排序法的实现思路以及时间复杂度
16. 解释面向对象编程和面向过程编程，并说明各自的利弊

&emsp;&emsp;答：面向过程编程是将解决问题的方法分步实现，而面
向对象编程则是以一个对象为基础，为它添加不同的属性，不同的行为方法。

很明显面向对象编程更加的适合现在的编程思维，扩展性好，可维护性也好，
但是它较为费内存，在单片机等内存有限的地方还是会使用面向过程编程的，
性能要求较高。

17. 说明 http1.0,1.1,2 的主要区别

&emsp;&emsp;答： HTTP1.0 和 HTTP1.1 的主要区别是长连接(keep-alive)，节约带宽(header 方法，如果
客户端有权限访问，则返回 100，这时客户端才把请求 body 传过来。否则返回 401。支持部分传输
Range)，HOST 域(字段，支持多个虚拟站点共享同一个 IP 和端口)。

&emsp;&emsp;HTTP1.1 和 HTTP2.0 的主要区别是服务器推送（当客户端发起请求的时候，服务端可以顺带推
送一些资源），多路复用（同一个连接并发处理多个请求，而且并发请求的数量比 HTTP1.1 大了
好几个数量级），数据压缩（支持请求头数据压缩）

18. mongodb 如何分析查询效率

&emsp;&emsp;答：可以从几个方面来进行回答：

&emsp;&emsp;一、使用 `db.setProfilingLevel()`来进行慢查询数据的收集（具体参见这篇[MongoDB 查询优化分析](https://www.cnblogs.com/zhoujinyi/p/3566773.html)）。

&emsp;&emsp;二、使用`explain()`分析单次查询(具体参见[官网 分析查询性能](http://www.mongoing.com/docs/tutorial/analyze-query-plan.html))

19. 如何定位 http 状态码错误的问题

# 晓信科技

1. 介绍 golang 的调度模型

2. osi 七层模型并说明每层的作用

3. 介绍一下 Mongodb，并说明 mongodb3.0 和 4.0 的区别

4. osi 七层模型是怎么保证可靠的数据传输服务呢？

# 常见问题

## 1. 浏览器的一个请求从发送到返回都经历了什么，讲的越详细越好

我大概讲下我的答案：

1、先从网络模型层面：

首先 client 通过 DNS 协议获取目标域名的 IP，然后 client （浏览器）与 server 通过 http 协议通讯，http 协议属于应用层协议，http 基于 tcp 协议，所以 client 与 server 主要通过 socket 进行通讯；

而 tcp 属于传输层协议、如果走 https 还需要会话层 TLS、SSL 等协议； 传输层之下网络层，这里主要是路由协议 OSPF 等进行路由转发之类的。再向下数据链路层主要是 ARP、RARP 协议完成 IP 和 Mac 地址互解析，再向下到最底层物理层基本就是 IEEE 802.X 等协议进行数据比特流转成高低电平的的一些定义等等；

当浏览器发出请求，首先进行数据封包，然后数据链路层解析 IP 与 mac 地址的映射，然后上层网路层进行路由查表路由，通过应用层 DNS 协议得到目标地址对应的 IP ，在这里进行 n 跳的路由寻路；而传输层 tcp 协议可以说下比较经典的三次握手、四次分手的过程和状态机。

2、应用层方面：

数据交换主要通过 http 协议， http 协议是无状态协议，这
里可以谈一谈 post、get 的区别以及 RESTFul 接口设计，然
后可以讲服务器 server 模型 epoll、select 等，接着可以根据实际经验讲下 server 处理流程，比如我： server 这边 Nginx 拿到请求，进行一些验证，比如黑名单拦截之类的，然后 Nginx 直接处理静态资源请求，其他请求 Nginx 转发给后端服务器，这里我用 uWSGI, 他们之间通过 uwsgi 协议通讯，uWSGI 拿到请求，可以进行一些逻辑， 验证黑名单、判断爬虫等，根据 wsgi 标准，把拿到的 environs 参数扔给 Django ，Django 根据 wsgi 标准接收请求和 env， 然后开始 start_response ，先跑 Django 相关后台逻辑，Django 拿到请求执行 request middleware 内的相关逻辑，然后路由到相应 view 执行逻辑，出错执行 exception middleware 相关逻辑，接着 response 前执行 response middleware 逻辑，最后通过 wsgi 标准构造 response， 拿到需要返回的东西，设置一些 headers，或者 cookies 之类的，最后 finish_response 返回，再通过 uWSGI 给 Nginx ，Nginx 返回给浏览器。
