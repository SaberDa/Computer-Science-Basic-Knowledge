# Load Balancing



Load balancer(LB) 是另一个对于任何分布式系统都很重要的关键成分。它根据某种标准（随机，循环，根据内存或者CPU利用率加权的随机）来分配多个资源的负载。LB 同时还会跟踪在发布请求时所有资源的状态。如果一个服务器不可用于执行新的请求，或者当前服务器无响应，或者该服务器错误率提高，那么LB将停止向该服务器发送数据流。



为了充分利用可扩展性（scalability ）和冗余性（redundancy ），我们可以尝试平衡系统每一层的负载。我们可以在系统的三个位置添加 LB：



- 在用户和服务器之间
- 在服务器和内部平台层之间，比如应用服务器(application servers)或者缓存服务器(cache servers)
- 在内部平台层和数据库之间

![](https://raw.githubusercontent.com/SaberDa/System-Design-Notes/master/notes/pic/Load Balancing-1.png)



## 实现负载均衡的方式



### 1. Smart Clients



Smart Client 将使用一个服务主机池来平衡它们之间的负载。同时Smart Client还会检测未响应主机，以避免发送请求。Smart Client同时还需要检测修复的主机，并且添加新主机等。

对于开发者而言，在数据库客户端（cache，service，etc.）添加负载均衡功能是一种很有效的解决方案。它看起来易于开发和管理，特别是针对于小型系统。但是随着系统的增大，LB有可能需要它自己的独立服务器。



### 2. Hardware Load Balancers

Hardware Load Balancers 虽然贵，但是它的性能很好，类似于 Citrix NetScaler。虽然使用hardware load balancers可以解决各种问题，但是这个方案十分昂贵，而且不易配置和维护。

因此，即使是预算很大公司，也通常会避免使用hardware load balancers来满足所有的负载均衡需求。相反，他们仅将hardware load balancers用作用户向其基础结构提出请求的第一基点，而后使用其他机制(smart client  or software load balancer) 来均衡其网络中的流量。



### 3. Software Load Balancers

如果我们想避免创建smart client的麻烦，或者避免购买过多的hardware load balancers，我们可以使用一种称为 software load balancer的混合方法。

HAProxy是目前较流行的开源LB之一。负载均衡器可以放置在客户端和服务器之间，或者放置在两个服务器层之间。如果我们可以控制运行客户端的机器，那么HAProxy可以在同一台机器上运行。针对于每个准备进行负载均衡的服务器，都拥有一个在其本地绑定的端口（比如 localhost:9000），客户端可以使用这个端口连接到服务器。该端口实际上由HAProxy管理。该端口上的每个来自客户端的请求都会被代理接受，并且以一种高效的方式（distributing load）将其传递给后端。

如果我们无法管理运行客户端的机器，那么HAProxy可以在中间服务器上运行。类似的，我们可以在不同的服务器端的组件间运行代理。HAProxy将会管理状态检查，并将计算机从主机池中删除或者添加。同时它还会在这些主机池中平衡各个主机之间的请求。

---



**对于大多数系统，我们应该从software load balancer入手，并根据需求转向smart client或者hardware load balancers。**

---



## Load Balancer 的作用



- 防止请求进入无响应或者无法处理该请求的服务器
- 防止资源过载
- 帮助消除单一的故障点

---



## 使用 Load Balancer 可以带来的额外好处



- SSL termination 
  - 对传入的请求解密，或者对服务器的请求加密。
  - 这样可以避免后端服务器进行无必要的高消耗运算。
  - 不需要再对每台服务器上安装 [x.509 certificates](https://en.wikipedia.org/wiki/X.509)

- Session presistence
  - 如果web应用不跟踪回话，则发出cookie并将特定的客户端的请求路由到同一实例

---



为了避免发生故障，通常使用复式的LB，比如active-passive 或者 active-active 模式。

---



## Load Balancer 的缺点



- 如果没有足够的资源配置，或者配置上出现错误，那么LB就会变成一个影响系统性能发挥的瓶颈
- 引入LB虽然会帮助消除单点故障，但是这也增加了额外的操作
- 单个LB会导致单点故障，但是配置复式的LB会进一步增加系统的复杂性