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

![](https://raw.githubusercontent.com/SaberDa/Computer-Science-Basic-Knowledge/master/System%20Design/notes/pic/pull.png)

### Storage 存储 - Pull 模型有什么缺陷

```
getNewsFeed(request)
    followings = DB.getFollowings(user = request.user)
    news_feed = empty
    for follow in followings:
        # N 次 DB reads 非常慢
        # 且发生在用户获得 News Feed 的请求过程中
        tweets = DB.getTweets(follow.to_user, 100)
        news_feed.merge(tweets)
    sort(news_feed)
    return news_feed

postTweet(request, tweet)
    DB.insertTweet(request.user, tweet)
    return success
```

### Storage 存储 - Push Model

- 算法
    - 为每个用户建立一个 List 存储他的 News Feed 信息
    - 用户发一个 Tweet 之后，将该推文逐个推送到每个用户的 News Feed 中
        - 关键词： Fanout
    - 用户需要查看 News Feed 时，只需要从该 News Feed List 中读取最新的 100 条即可
- 复杂度分析
    - News Feed => 1 次 DB Read
    - Post a tweet => N 个粉丝，需要 N 次 DB Write
        - 好处是可以用异步任务在后台执行，无需用户等待

| News Feed Table |  Prop |
| :---- | :---- |
| id | integer |
| owner_id | Foreign Key |
| tweet_id | Foreign Key |
| created_at | timestamp |


```
# XXX 想看到所有的 News Feed
select * from news_feed_table where owner_id=XXX order_by created_at desc limit 20;
```

### Storage 存储 - Push 原理图

![](https://raw.githubusercontent.com/SaberDa/Computer-Science-Basic-Knowledge/master/System%20Design/notes/pic/push.png)


### Storage 存储 - Push 模型有什么缺陷

```
getNewsFeed(request)
    return DB.getNewsFeed(request.user)

postTweet(request, tweet_info)
    tweet = DB.insertTweet(request.user, tweet_info)
    # 异步执行
    AsyncService.fanoutTweet(request.user, tweet)
    return success

AsyncService::fanoutTweet(user, tweet)
    followers = DB.getFollowers(user)
    for follower in followers:
        # follower 的数目可能很大
        DB.insertNewsFeed(tweet, follower)
```

### Storage 存储 - Pull vs Push

- 热门 Social APP 的模型
    - Facebook -> Pull
    - Instagram -> Pull + Push
    - Twitter -> Pull
- 误区
    - 不坚定想法，摇摆不定
    - 不能表现出 Tradeoff 的能力
    - 无法解决待定的问题


---


# 4S

- 用过前3个步骤的分析，我们已经得到了一个可行方案
- Scenario 场景
    - 和面试官讨论
    - 搞清楚需要设计哪些功能
    - 并分析出所设计的系统大概所需要支持的 Concurrent Users/ QPS/ Memory/ Storage
- Service 服务
    - 合并需要设计功能，相似的功能整合为一个 Service
- Storage 存储
    - 对每个 Service 选择合适的存储结构
    - 细化数据表单
    - 画图展示数据存储和读取的流程
- 得到一个 Work Solution 而不是 Product Solution
- 这个 Work Solution 可以存在很多待解决的问题



---

# Scale 扩展

How to scale? 系统如何优化与维护？

- Optimize 优化
- Maintenance 维护

### Scale 扩展 - 如何优化系统

- 第一步：Optimize
    - 解决设计缺陷 Solve Problems
        - Pull vs Push, Normalize vs De-normalize
    - 更多功能设计
        - Edit, Delete, Media, Ads
    - 一些特殊用例
        - Lady Gaga, Inactive user
- 第二步：Maintenance
    - 鲁棒性 Robust
        - 如果有一台服务器/数据库挂了怎么办
    - 扩展性 Scalability
        - 如果有流量暴增，如何扩展

### Scale 扩展 - 解决 Pull 的缺陷

- 最慢的部分发生在用户读请求时（需要耗费用户等待时间）
    - 在 DB 访问之前加入 Cache
    - Cache 每个用户的 Timeline
        - N 次 DB 请求 -> N 次 Cache 请求（N 是你关注的好友个数）
        - Trade off：Cache 所有的？ 还是 Cache 最近的 1000 条？
    - Cache 每个用户的 News Feed
        - 没有 Cache News Feed 的用户：归并 N 个用户最近的 100 条 Tweets，然后取出结果的前 100 条
        - 有 Cache News Feed 的用户：归并 N 个用户的在某个时间戳之后的所有 Tweets
- 对比 MySQL 和 Memcached 的 QPS
    - 时间相差几个数量级



### Scale 扩展 - 解决 Push 的缺陷

- 浪费更多的存储空间 Disk
    - 与 Pull 模型将 News Feed 存在内存（Memory）中相比
    - Push 模型将 News Feed 存在硬盘（Disk）里完全不是事
    - Disk is cheap
- 不活跃用户 Inactive Users
    - 粉丝排序 Rank followers by weight(for example: last login time)
- 粉丝数目 followers >> 关注数目 following
    - Lady Gaga 问题
    - 无解？完全切换回 Pull？
    - Trade off: (Pull + Push) vs Pull


### Scale 扩展 - Lady Gaga 问题

- 粉丝 Followers 65.5M
- Push 的挑战
    - Fanout 的过程可能需要几个小时
- 面试时错误的回答方式：
    - 既然 Push 不行，那我们就切换回 Pull 吧
- 正确的思路
    - 尝试在现有的模型下做最小的改动来优化
        - 比如多加几台用于做 Push 任务的机器，Problem Solvec
    - 对长期的增长进行估计，并评估是否值得转换整个模型
- Push 结合 Pull 的优化方案
    - 普通的用户仍然 Push
    - 将 Lada Gaga 这类的用户，标记为明星用户
    - 对于明星用户，不 Push 到用户的 News Feed 中
    - 当用户需要的时候，来明星用户的 Timeline 里取，并合并到 News Feed 里
- 摇摆问题
    - 明星定义
        - Followers > 1m
    - 掉粉
        - 一天内掉了几十万粉
    - 解决方法
        - 明星用户发 Tweet 后，已然继续 Push 他们的 Tweet 到所有用户的 News Feed里
            - 原来的代码完全不用改了
        - 将关注对象中的明星用户的 Timeline 与自己的 News Feed 进行合并后展示
            - 但并不存储自己的 News Feed 列表，因为 Push 会来负责这个事情
- 为什么既然大家都用 Pull，我们仍然要学习 Push？
    - 系统设计不是选择一个最好的方案
    - 而是选择一个最合适的方案
    - 如果你没有很大的流量，Push 是最经济最省力的做法
- 系统设计面试也并不是期望你答出最优的解决方案，而是从你的分析当中判断你对系统的理解和知识储备
- 什么时候用 Push？
    - 质押少
    - 想偷懒，少写代码
    - 实时性要求不高
    - 用户发帖比较少
    - 双向好友关系，没有明星问题（比如朋友圈）
- 什么时候用 Pull？
    - 资源充足
    - 实时性要求高
    - 用户发帖很多
    - 单向好友关系，有明星问题



### Scale 扩展 - 如何实现 Follow & Unfollow?

- 如何实现 Follow & Unfollow
    - Follow 一个用户之后，异步地将他的 Timeline 合并到你的 News Feed中
        - Merge timeline into news feed asynchronously
    - Unfollow 一个用户之后，异步地将他的 Tweets 从你的 News Feed 删除
        - Pick out tweets from news feed asynchronously
- 为什么需要异步 Async？
    - 因为这个过程不快
- 异步的好处
    - 用户迅速得到反馈，似乎马上就 follow/ unfollow 成功了
- 异步的坏处
    - Unfollow 之后立即刷新 News Feed，还能发现他的信息
    - 最终还是会被删除掉


### Scale 扩展 - 如何存储 Likes

![](https://raw.githubusercontent.com/SaberDa/Computer-Science-Basic-Knowledge/master/System%20Design/notes/pic/like-table.png)

### Scale 扩展 - 惊群效应 Thundering Herd

- 数据库承受不住压力
    - 对于同一条数据短时间出现大量的请求
        - 因为是同一条数据，所以什么 load balancer, sharding, consistent hashing 都不管用
- 解决方案：那我加上Cache？
    - 真的有那么容易？
- Follow up:
    - 点赞、转发、评论，都会修改这条 Tweet 的基本信息，如何更新？
        - Keywords: Write through, Write back, Look aside
- Follow up:
    - Cache 失效怎么办？
        - 因为内存不够或者 Cache 决策错误，热点信息被提出了 Cache，会发生什么？
            - DB 会瞬间收到一大波对该数据请求，然后挂了
    - 解决方案：
        - Facebook Lease Get
        - http://bit.ly/1jDzKZK

# Design Twitter - 总结

- 需求分析 Scenario
- 服务图谱 Service Graph
- 表单设计 Schema Design
- Push vs Pull
    - 明星问题
    - 不活跃用户问题 Inactive users
- Follow & Unfollow
- 标准化与非标准化 Normalize vs De-normalized
- 热点问题 Hot Spot/ Thundering Herd
