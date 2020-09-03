# spike
处理秒杀的高并发主要有三种方式，第一种就是缓存，将商品详情页手动进行渲染，然后对真个页面进行缓存，减少并发对数据库造成的压力，增大系统的可用容量。第二种就是限流，利用guava的令牌桶算法限流器，对请求进行限流，对在一定时间内没有获取到令牌的请求进行降级处理，直接提示客户端系统繁忙，对已经处理过的秒杀商品进行标记并保存在缓存中，如果下次再进行访问，直接从缓存中取标记，如果标记过直接返回秒杀结束。将秒杀商品的库存信息保存在缓存中，增加请求的访问速率。对成功秒杀的请求放入到消息队列中，实时的对订单进行处理。为了防止因为并发下单导致的超卖问题，在库存表中增加了版本字段，增加了乐观锁，保证修改库存不会出现脏数据
