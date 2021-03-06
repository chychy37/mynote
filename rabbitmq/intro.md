# Intro

## 特点
+ 高可靠
+ 易扩展
+ 高可用


## 作用
+ 解偶
+ 冗余
+ 扩展性
+ 削峰
+ 可恢复性
+ 顺序保证
+ 缓冲
+ 异步通信


## 消息
+ 消息体：payload
+ 标签：交换器名称，路由键 等（路由到队列后删除）
+ props:
  + contentType
  + contentEncoding
  + headers
  + deliveryMode
  + priority
  + correlationId
  + replyTo
  + expiration
  + messageId
  + timestamp
  + type
  + userId
  + appId
  + clusterId
+ mandatory: ture，消息无法到达queue时，Basic.Return; false，消息直接丢弃或发送到备份交换机
+ immediate: 已废弃


## 队列
+ 一个队列可以对应多个消费者，消息平均分摊给每个消费者
+ 不支持，也不建议队列层面的消费广播

### api args
+ queue: 队列名称
+ durable: 是否持久化
+ exclusive: 是否排他（该队列只对首次声明它的连接可见，并在连接断开时自动删除（无视durable））
+ autoDelete: 是否自动删除（至少有一个消费者连接到这个队列，之后所有与这个队列连接的消费者都断开时，自动删除）
+ arguments: 
  + x-message-ttl
  + x-expires
  + x-max-length
  + x-max-length-bytes
  + x-dead-letter-exchange
  + x-dead-letter-routing-key
  + x-max-priority


## 交换器
### type
#### fanout
+ 无视binding_key，将消息路由到所有绑定的队列

#### direct
+ 将消息路由到binding_key和routing_key完全匹配的队列中

#### topic
+ 将消息路由到binding_key和routing_key模糊匹配的队列中
+ `*`：匹配一个单词
+ `#`：匹配零个或多个单词

#### headers
+ 不依赖路由键的匹配规则，通过headers属性进行匹配，来路由消息到队列中
+ 性能差，不实用

### api args
+ name: 交换器名字
+ type: 交换器类型
+ durable: 是否持久化
+ autoDelete: 是否自动删除（至少有一个队列或交换器与这个交换器绑定，之后所有与这个交换器绑定的队列或交换器与此解绑 -> 自动删除）
+ internal: 是否是内置的（客户端无法直接发送消息到这个交换器，只能通过其他交换器路由到此交换器）
+ arguments: alternate-exchange, 配置备份交换机，在消息无法路由时，发送给备份交换机

## 运转流程
### 生产者
1. 连接到broker，建立connection，建立channel
2. 声明交换器
3. 声明队列
4. 绑定交换器和队列
5. 发送消息至broker
6. 处理可能的broker的回调（消息发送失败/消息未到达queue）
7. 关闭channel，关闭connection

### 消费者
1. 连接到broker，建立connection，建立channel
2. 向broker请求消息，设置回调函数
3. 接受消息，处理
4. ack
5. 关闭channel，关闭connection


## connection和channel
+ channel复用connection的tcp连接，节约资源
+ 当channel的流量很大时，复用一个connection会产生性能瓶颈，需要开辟多个connection