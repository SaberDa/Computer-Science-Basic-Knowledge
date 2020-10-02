# System Design Websites



## GitHub



- [System Design Preparation](https://github.com/shashank88/system_design)
  - How to prepare for and answer system design questions
    -  Starting point
    -  basics
    -  How to answer in interviews
    -  Steps how I approach the system design questions in interviews
    -  Common Design questions
    -  architecture
    -  company engineering blog links
    -  Low on time ?

- [How to prepare system design questions for an IT company](https://github.com/checkcheckzz/system-design-interview)
  -  System Design Interview Tips
  -  Basic Knowledge about System Design
  -  Company Engineering Blogs
  -  Products and Systems
  -  Hot Questions and Reference
  -  Good Books
  -  Object Oriented Design

- [Tech Interview Handbook](https://github.com/yangshun/tech-interview-handbook)
  - How to prepare for coding interviews
  - Interview Cheatsheet - Straight-to-the-point Do's and Don'ts
  - Algorithm tips and the best practice questions categorized by topic
  - "Front-end Job Interview Questions" answers
  - Interview formats of the top tech companies
  - Behavioral questions asked by the top tech companies
  - Good questions to ask your interviewers at the end of the interviews
  - Helpful resume tips to get your resume noticed and the Do's and Don'ts

- [System Design and Architecture](https://github.com/puncsky/system-design-and-architecture)
  - 实践
    - 短网址系统
    - Uber
    - Facebook 储存大规模社交图谱
    - Apache Kafka
    - Facebook 图片储存系统
    - HTTP 协议向移动设备传输视频（HTTP Live Streaming）
    - Lyft 的营销自动化平台 (Symphony)
    - 负债均衡器
    - i18n 工程方案
    - 区块链服务端架构
  - 理论
    - Introduction to Architecture
    - How to scale a web service?
    - ACID vs BASE
    - Data Partition and Routing
    - Replica, Consistency, and CAP theorem
    - Load Balancer Types
    - Concurrency Model
    - Improving availability with failover
    - Bloom Filter
    - Skiplist
    - B tree vs. B+ tree
    - Intro to Relational Database
    - 4 Kinds of No-SQL
    - Key value cache
    - Stream and Batch Processing Frameworks
    - Cloud Design Patterns
    - Public API Choices
    - Lambda Architecture
    - iOS Architecture Patterns Revisited
    - What can we communicate in soft skills interview?
    - Experience Deep Dive
    - 3 Programming Paradigms
    - SOLID Design Principles

- [System Design Primer](https://github.com/donnemartin/system-design-primer/blob/master/README-zh-Hans.md)
  - 系统设计入门
    - 性能与扩展性
    - 延迟与吞吐量
    - 可用性与一致性
    - 一致模式
    - 可用模式
    - 域名系统
    - CDN
    - 负载均衡器
    - 反向代理
    - 应用层
    - 数据库
    - 缓存
    - 异步
    - 通讯
    - 安全
    - 附录

---



## Links



------

Here are some articles about system design related topics.

- [How to Rock a Systems Design Interview](http://www.palantir.com/2011/10/how-to-rock-a-systems-design-interview/)
- [System Interview](http://www.hiredintech.com/app#system-design)
- [Scalability for Dummies](http://www.lecloud.net/tagged/scalability)
- [Scalable Web Architecture and Distributed Systems](http://www.aosabook.org/en/distsys.html)
- [Numbers Everyone Should Know](http://everythingisdata.wordpress.com/2009/10/17/numbers-everyone-should-know/)
- [Scalable System Design Patterns](http://horicky.blogspot.com/2010/10/scalable-system-design-patterns.html)
- [Introduction to Architecting Systems for Scale](http://lethain.com/introduction-to-architecting-systems-for-scale/)
- [Transactions Across Datacenters](http://snarfed.org/transactions_across_datacenters_io.html)
- [A Plain English Introduction to CAP Theorem](http://ksat.me/a-plain-english-introduction-to-cap-theorem/)
- [The CAP FAQ](https://github.com/henryr/cap-faq)
- [Paxos Made Simple](http://research.microsoft.com/en-us/um/people/lamport/pubs/paxos-simple.pdf)
- [Consistent Hashing](http://www.tom-e-white.com/2007/11/consistent-hashing.html)
- [NOSQL Patterns](http://horicky.blogspot.com/2009/11/nosql-patterns.html)
- [Scalability, Availability & Stability Patterns](http://www.slideshare.net/jboner/scalability-availability-stability-patterns)

---



## **Hot Questions and Reference:**



**Design a CDN network**

Reference:

- [Globally Distributed Content Delivery](http://www.akamai.com/dl/technical_publications/GloballyDistributedContentDelivery.pdf).

**Design a Google document system**

Reference:

- [google-mobwrite](https://code.google.com/p/google-mobwrite/)
- [Differential Synchronization](https://neil.fraser.name/writing/sync/).

**Design a random ID generation system**

Reference:

- [Announcing Snowflake](https://blog.twitter.com/2010/announcing-snowflake)
- [snowflake](https://github.com/twitter/snowflake/).

**Design a key-value database**

Reference:

- [Introduction to Redis](http://www.slideshare.net/dvirsky/introduction-to-redis).

**Design the Facebook news seed function**

Reference:

- [What are best practices for building something like a News Feed?](http://www.quora.com/What-are-best-practices-for-building-something-like-a-News-Feed)
- [What are the scaling issues to keep in mind while developing a social network feed?](http://www.quora.com/Activity-Streams/What-are-the-scaling-issues-to-keep-in-mind-while-developing-a-social-network-feed)
- [Activity Feeds Architecture](http://www.slideshare.net/danmckinley/etsy-activity-feeds-architecture)

**Design the Facebook timeline function**

Reference:

- [Building Timeline](https://www.facebook.com/note.php?note_id=10150468255628920)
- [Facebook Timeline](http://highscalability.com/blog/2012/1/23/facebook-timeline-brought-to-you-by-the-power-of-denormaliza.html).

**Design a function to return the top k requests during past time interval**

Reference:

- [Efficient Computation of Frequent and Top-k Elements in Data Streams](https://icmi.cs.ucsb.edu/research/tech_reports/reports/2005-23.pdf)
- [An Optimal Strategy for Monitoring Top-k Queries in Streaming Windows](http://davis.wpi.edu/xmdv/docs/EDBT11-diyang.pdf)

**Design an online multiplayer card game**

Reference:

- [How to Create an Asynchronous Multiplayer Game](http://www.indieflashblog.com/how-to-create-an-asynchronous-multiplayer-game.html)
- [How to Create an Asynchronous Multiplayer Game Part 2: Saving the Game State to Online Database](http://www.indieflashblog.com/how-to-create-async-part2.html)
- [How to Create an Asynchronous Multiplayer Game Part 3: Loading Games from the Database](http://www.indieflashblog.com/how-to-create-async-part3.html)
- [How to Create an Asynchronous Multiplayer Game Part 4: Matchmaking](http://www.indieflashblog.com/how-to-create-async-part4-html.html#comment-4447)
- [Real Time Multiplayer in HTML5](http://buildnewgames.com/real-time-multiplayer/)

**Design a graph search function**

Reference:

- [Building out the infrastructure for Graph Search](https://www.facebook.com/notes/facebook-engineering/under-the-hood-building-out-the-infrastructure-for-graph-search/10151347573598920)
- [Indexing and ranking in Graph Search](https://www.facebook.com/notes/facebook-engineering/under-the-hood-indexing-and-ranking-in-graph-search/10151361720763920)
- [The natural language interface of Graph Search](https://www.facebook.com/notes/facebook-engineering/under-the-hood-the-natural-language-interface-of-graph-search/10151432733048920) and [Erlang at Facebook](http://www.erlang-factory.com/upload/presentations/31/EugeneLetuchy-ErlangatFacebook.pdf).

**Design a picture sharing system**

Reference:

- [Flickr Architecture](http://highscalability.com/flickr-architecture)
- [Instagram Architecture](http://highscalability.com/blog/2011/12/6/instagram-architecture-14-million-users-terabytes-of-photos.html).

**Design a search engine**

Reference:

- [How would you implement Google Search?](http://programmers.stackexchange.com/questions/38324/interview-question-how-would-you-implement-google-search)
- [Implementing Search Engines](http://www.ardendertat.com/2012/01/11/implementing-search-engines/)

**Design a recommendition system**

Reference:

- [Hulu’s Recommendation System](http://tech.hulu.com/blog/2011/09/19/recommendation-system/)
- [Recommender Systems](http://ijcai13.org/files/tutorial_slides/td3.pdf)

**Design a tinyurl system**

Reference:

- [System Design for Big Data-tinyurl](http://n00tc0d3r.blogspot.com/)
- [URL Shortener API](https://developers.google.com/url-shortener/?csw=1).

**Design a garbage collection system**

Reference:

- [Baby's First Garbage Collector](http://journal.stuffwithstuff.com/2013/12/08/babys-first-garbage-collector/).

**Design a scalable web crawling system**

Reference:

- [Design and Implementation of a High-Performance Distributed Web Crawler](http://cis.poly.edu/suel/papers/crawl.pdf)

**Design the Facebook chat function**

Reference:

- [Erlang at Facebook](http://www.erlang-factory.com/upload/presentations/31/EugeneLetuchy-ErlangatFacebook.pdf)
- [Facebook Chat](https://www.facebook.com/note.php?note_id=14218138919&id=9445547199&index=0)

**Design a trending topic system**

Reference:

- [Implementing Real-Time Trending Topics With a Distributed Rolling Count Algorithm in Storm](http://www.michael-noll.com/blog/2013/01/18/implementing-real-time-trending-topics-in-storm/)
- [Early detection of Twitter trends explained](http://snikolov.wordpress.com/2012/11/14/early-detection-of-twitter-trends/)

**Design a cache system**

Reference:

- [Introduction to Memcached](http://www.slideshare.net/oemebamo/introduction-to-memcached).