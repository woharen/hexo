---
title: Redis实现Redlock分布式锁
date: 2021-03-18 15:08:05
tags:
  - Redis
---


## 依赖
```xml
<!--Springboot环境下 redis分布式锁 -->
<dependency>
    <groupId>org.redisson</groupId>
    <artifactId>redisson-spring-boot-starter</artifactId>
    <version>3.11.1</version>
</dependency>

<!-- Springboot环境下 -->
<dependency>
    <groupId>org.redisson</groupId>
    <artifactId>redisson</artifactId>
    <version>3.11.1</version>
</dependency>

```
## 代码实现
```java

public void test(){
    //创建连接1
    RedissonClient redissonClient1 = getRedissonClient("redis://127.0.0.1:6379", 0);
    //创建连接2
    RedissonClient redissonClient2 = getRedissonClient("redis://127.0.0.1:6379", 1);
    //创建连接3
    RedissonClient redissonClient3 = getRedissonClient("redis://127.0.0.1:6379", 2);
    
    //设置锁的key
    String resourceName = "REDLOCK_KEY";
    
    //获取锁
    RLock lock1 = redissonClient1.getLock(resourceName);
    RLock lock2 = redissonClient2.getLock(resourceName);
    RLock lock3 = redissonClient3.getLock(resourceName);
    // 向3个redis实例尝试加锁
    RedissonRedLock redLock = new RedissonRedLock(lock1, lock2, lock3);
    boolean isLock;
    
    List<MaxLimitRankingVo> list = null;
    try {
        // 500ms拿不到锁, 就认为获取锁失败。10000ms即10s是锁失效时间。
        isLock = redLock.tryLock(500, 10000, TimeUnit.MILLISECONDS);
        System.out.println("isLock = "+isLock);
        if (isLock) {
            Thread.sleep(8000);
        }
    } catch (Exception e) {
    } finally {
        // 无论如何, 最后都要解锁
        redLock.unlock();
    }
}
    

/**
 * 获取 RedissonClient
 * @param address 地址
 * @return
 */
public RedissonClient getRedissonClient(String address, int database){
    Config config = new Config();
    config.useSingleServer().setAddress(address).setDatabase(database);
    return Redisson.create(config);

}
```

## 文档参考
[Redis分布式锁的正确实现方式](https://zhuanlan.zhihu.com/p/60309808)