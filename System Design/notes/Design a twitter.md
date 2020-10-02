# Scenario 

### 询问面试官

- 需要设计哪些功能？

#### First Step : Enumerate

- 把 Twitter 的功能一个个罗列出来
- Register/ Login
- User Profile Display/ Edit
- Upload Image/ Video
- Search
- Post/ Share a tweet
- Timeline/ News Feed
- Follow/ Unfollow a user

#### Second Step : Sort

- 选出核心功能，因为你不可能这么短的时间内什么都设计
- Post a tweet
- Timeline
- News Feed
- Follow/ Unfollow a user
- Register/ Login


### Analysis 分析

- 需要承受多大的访问量？
    - 日活跃用户 (Daily Active Users)
    - Twitter: MAU 313M, DAU ~150M+

#### 并发用户 Concurrent User

- 日活跃 * 每个用户平均请求次数 / 一天多少秒 = 150M * 60 / 86400 ~ 100k
- 峰值 Peak = Average Concurrent User * 3 ~ 300k
- 快速增长的产品 Fast Growing
    - Max peak users in 3 months = Peak users * 2

#### 读频率 Read QPS(Queries Per Second)

#### 写频率 Write QPS

重要的是计算过程，而不是计算结果

#### 分析出 QPS 有什么用？

- QPS = 100
    - 用你的笔记本做 Web 服务器就好
- QPS = 1k
    - 用一台好点的 Web 服务器就差不多了
    - 需要考虑 Single Point Failure
- QPS = 1m
    - 需要建设一个 1000 台 Web 服务器的集群
    - 需要考虑如何 Maintainance (某一台挂了怎么办)

#### QPS 和 Web Server(服务器)/ Database(数据库)之间的关系

- 一台 Web Server 约承受量是 1k 的 QPS (考虑到逻辑处理时间以及数据库查询的瓶颈)
- 一台 SQL Database 约承受量是 1k 的 QPS (如果 JSON 和 INDEX query 比较多的话，这个值会更小)
- 一台 NoSQL Database (Cassandra) 约承受量是 10k 的 QPS
- 一台 NoSQL Database (Memcached) 约承受量是 1M 的 QPS


---

# Service 服务

将大系统拆分成小服务

- Router：
    - User Service: Register, Login
    - Tweet Service: Post a tweet, News Feed, Timeline
    - Media Service: Upload Image/ Video
    - Friendship Service: Follow/ Unfollow




### Replay 重放需求

重新过一遍每个需求，为每个需求添加一个服务



### Merge 归并需求

归并相同的服务

### 什么是服务 Service?

- 可以认为是逻辑处理的整合
- 对于同一类问题的逻辑处理归并在一个 Service 中
- 把整个 System 细分为若干个小的 Service

---

# Storage 存储

数据如何存储和访问

### Select 为每个 Service 选择存储结构

结构化信息，放在数据库中
    结构化信息，简单来讲就是你能写出一个类来表示这些信息
非结构化信息，放在文件系统

- 关系型数据库 SQL Database
    - 小调查：Twitter 的那些信息适合放在关系型数据库中？
    - 用户信息 User Table
- 非关系型数据库 NoSQL Database
    - 小调查：Twitter 的那些信息适合放在非关系型数据库中？
    - 推文 Tweet
    - 社交图谱 Social Graph (followers)
- 文件系统 File System
    - 小调查： Twitter 的那些信息适合放在文件系统中？
    - 图片、视频 Media Files


- 第一步： Select
    - 为每个 Application/ Service 选择合适的存储结构
- 第二步： Schema
    - 细化数据表结构

- 程序 = 算法 + 数据结构
- 系统 = 服务 + 数据存储

- Receptionist
    - User Service -> SQL
    - Tweet Service -> NoSQL
    - Media Service -> File System
    - Friendship Service -> SQL/ NoSQL 

### Schema 细化表结构

Please design schema

- Receptionist
    - User Service
        - id -> integer
        - username -> varchar
        - email -> varchar
        - password -> varchar
    - Friendship Service
        - from_user_id -> Foreign Key
        - to_user_id -> Foreign Key
    - Tweet Service
        - id -> integer
        - user_id -> Foreign Key
        - content ->text
        - created_at -> timestamp


---

## News Feed 如何存取

### 什么是 News Feed

- 你登录之后看到的信息流
- 你的所有朋友发的信息的集合

### 有哪些经典的 News Feed 系统

- Facebook
- Twitter
- 朋友圈
- RSS Reader

### News Feed 的核心因素

- 关注于被关注
- 每个人看到的新鲜事都是不同的

### Storage 存储 - Pull Model

- 算法
    - 在用户查看 News Feed 时，获取每个好友的前 100 条 Tweets，合并出前 100 条 News Feed
        - K 路归并算法 Merge K Sorted Arrays
- 复杂度分析
    - News Feed => 假如有 N 个关注对象，则为 N 次 DB Reads 的时间 + K 路归并时间（可忽略）
        - 为什么 K 路归并的时间可忽略？因为数据库访问的时间远远大于算法执行时间
    - Post a tweet => 1 次 DB Write 的时间

### Storage 存储 - Pull 原理图

