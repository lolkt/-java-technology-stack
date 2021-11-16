#### 1、概述

MQ是消息中间件，是一种在分布式系统中应用程序借以传递消息的媒介，常用的有ActiveMQ，RabbitMQ，kafka。ActiveMQ是Apache下的开源项目，完全支持JMS1.1和J2EE1.4规范的JMS Provider实现。 

#### 2、原理与设计

ActiveMQ消息形式：topic（非安全的），queue（安全的）

 

topic和queue区别：

topic：不安全 、没有状态的、一对多 、数据容易丢失的、传输速率高。

queue：安全、有状态的、一对一、数据不容易丢失的、传输速率低。 

工作流程：

a用户把消息发送到 mq的消息池里，mq负责接收这条消息，如果接到消息了，把连接断开了。mq有一个监控的功能，监控b用户是否上线。如果b用户上线了，把a用户的消息发送给b用户。b用户会把消息的返回值告诉连接池，完了b用户和连接池就断开了。mq接收到了b用户给它的返回值。（a用户只是把消息发过去了，还没有接收到信息 ）连接池时刻监视a用户是否在线，如果a用户上线了再把返回值发送给a用户。发送完成之后连接断开了。

topic消息形式工作流程：首先用户a把发送消息到消息池里， mq会存放你的消息，监控到b用户上线，将这条消息发过去。发送过去后，mq会把这条消息删掉，（如果b在返回的时候断掉了，那消息就丢了）。如果返回没断监控a用户上线，把消息返回给a。

queue消息形式工作流程：首先用户a把发送消息到消息池里， mq会存放你的消息，监控到b用户上线，将这条消息发过去。发送过去后，mq不会把这条消息删掉，会一直保留消息池里。当把消息给b，b接收到消息。mq接收到b的返回值，mq把b的返回值给a之后，才会把这个消息删掉。

（简单地说：queue会保存消息确认a收到了才会删掉，topic不会保存消息，）

 

JMS 简介

• JMS（Java Message Service），即：java消息服务应用程序接口。

• 是Java平台面向消息中间件（MOM）的API/技术规范。

• 场景：应用与两个应用程序之间，或者分布式系统架构中分发消息，可进行异步/同步方式的通讯，和平台API无关，基本多数的MOM都提供对JMS的支持。（异步用得比较多，不产生堵塞，效率高。同步经常产生堵塞）。

JMS 体系架构

• JMS提供者 – 连接面向消息中间件的，JMS接口的一个实现。提供者可以是Java平台的JMS实现，也可以是非Java平台的面向消息中间件的适配器。

• JMS客户 – 生产或消费基于消息的Java的应用程序或对象。

• JMS生产者 – 创建并发送消息的JMS客户。

• JMS消费者 – 接收消息的JMS客户。

• JMS消息 – 包括可以在JMS客户之间传递的数据的对象。

• JMS队列 – 一个容纳那些被发送的等待阅读的消息的区域。与队列名字所暗示的意思不同，消息地接受顺序并不一定要与消息的发送顺序相同。一旦一个消息被 阅读，该消息将被从队列中移走。

• JMS主题 – 一种支持发送消息给多个订阅者的机制。（必须选择一个消息机制，不选择发送失败）

JMS 属性

• Destination（接口/目标）

• Product（生产者）

• Consumer（消费者）

• Broker（消息转发器）

• P2P,Pub/Sub（模型）

 – P2P：消息队列（Queue）、发送者（Sender）、接收者（Receiver）

 – Pub/Sub：主题（Topic）、发布者（Publisher）、订阅者（Subscriber）

 

 

#### 2、ActiveMQ功能与作用

多种协议 、持久化 、 安全 、 群集 、监控、其他

AvtiveMQ多种协议

• VM vm://brokername

• TCP tcp://host:port

• SSL ssl://host:port

• HTTP http://host:port

• UDP udp://host:port

• peer peer://group/brokername

• multicast multicast://IPAddress

• static static(list uris)

• failover failvoer(list uris)

• discovery discovery://host:port

ActiveMQ集群 ：Broker（消息转发器）

 

ActiveMQ的主要特性

• (1)JMS1.1、J2EE1.4

• (2)J2EE servers(Tomcat,JBoss4,GlassFish,WebLogic…)

• (3）多语言客户端（Java,C,C++,C#,Ruby,PhP）

• (4）多种协议（VM,TCP,SSL,UDP,multicast,JGroups…)

• (5）集成Spring

• (6）支持Ajax

• (7)CXF,Axis（WebService的两个流行的框架）

• (8)REST（安全消息才有，状态传递）

• (9)Message Groups,Virtual Destinations,Wildcards,Composite ， Destinations

• (10）持久化（journal,JDBC)

• (11）性能（client-server,cluster,peer…)

 

#### 3、ActiveMQ必要性

1、支持多种语言编写客户端 

2、对spring的支持，很容易和spring整合 

3、支持多种传输协议：TCP,SSL,NIO,UDP等 

4、支持AJAX 

优点：提高数据的安全性，提高资源的使用率