# problem

#### 缓存雪崩
+ 大规模key同时失效，导致数据库压力过大宕机
+ 不同的过期时间
+ redis高可用（主从+哨兵）
+ 热点key不失效
+ 互斥+熔断

#### 缓存击穿
+ 热点key失效，导致数据库压力过大
+ 热点key不失效
+ 互斥

#### 缓存穿透
+ 频繁没命中缓存，导致频繁访问数据库
+ 布隆过滤器

#### 缓存预热
+ 初始化时，提前将数据存入，减少数据库压力

#### 缓存降级
+ 将部分热点数据缓存进服务器内存中，来减少对业务的影响