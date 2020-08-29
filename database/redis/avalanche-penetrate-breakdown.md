# 雪崩 & 击穿 & 穿透

## ✏ 1、缓存雪崩

在高并发下，同一时间`redis`中的key大面积失效，这一瞬间`Redis`跟没有一样，所有请求都落到了数据库上。

目前电商首页以及热点数据都会去做缓存 ，一般缓存都是定时任务去刷新，或者是查不到之后去更新的，定时任务刷新就有一个问题。

**举个简单的例子**：如果所有首页的Key失效时间都是12小时，中午12点刷新的，我零点有个秒杀活动大量用户涌入，假设当时每秒 6000 个请求，本来缓存在可以扛住每秒 5000 个请求，但是缓存当时所有的Key都失效了。此时 1 秒 6000 个请求全部落数据库，数据库必然扛不住，它会报一下警，真实情况可能 DBA 都没反应过来就直接挂了。此时，如果没用什么特别的方案来处理这个故障，DBA 很着急，重启数据库，但是数据库立马又被新的流量给打死了。这就是缓存雪崩。

### 解决方案

1、设置缓存的失效时间加一个随机值

```cpp
setRedis(Key, value, time + Math.random() * 10000);
```

2、使用缓存集群，保证缓存高可用

如果**Redis**是集群部署，将热点数据均匀分布在不同的**Redis**库中也能避免全部失效的问题。

3、设置热点数据永远不过期，有更新操作就更新缓存就好了（比如运维更新了首页商品，那你刷下缓存就完事了，不要设置过期时间），电商首页的数据也可以用这个操作，保险。

4、使用`Hystrix`

`Hystrix`是一款开源的“防雪崩工具”，它通过 熔断、降级、限流三个手段来降低雪崩发生后的损失。`Hystrix`就是一个Java类库，它采用命令模式，每一项服务处理请求都有各自的处理器。所有的请求都要经过各自的处理器。处理器会记录当前服务的请求失败率。一旦发现当前服务的请求失败率达到预设的值，`Hystrix`将会拒绝随后该服务的所有请求，直接返回一个预设的结果。这就是所谓的**“熔断”**。当经过一段时间后，`Hystrix`会放行该服务的一部分请求，再次统计它的请求失败率。如果此时请求失败率符合预设值，则完全打开限流开关；如果请求失败率仍然很高，那么继续拒绝该服务的所有请求。这就是所谓的**“限流”**。而`Hystrix`向那些被拒绝的请求直接返回一个预设结果，被称为**“降级”**。

## ✏ 2、缓存击穿

## ✏ 3、缓存穿透


