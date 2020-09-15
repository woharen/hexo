## JAVA分布式锁## zk分布式锁实现原理

![zk分布式锁流程图](https://img-blog.csdnimg.cn/20181026092826685.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xvdmV4aWFvdGFvemk=,size_27,color_FFFFFF,t_70)

1. 客户端调用方法
2. 服务端在zookeeper下创建临时节点并获取节点名
3. 判断创建的节点是否为第一个节点
4. 如果是，则拥有锁，使用完后释放锁
5. 如果不是，等待监听其他节点释放。

## redis分布式锁实现原理

### 单个redis实例

- **加锁**

  1. 服务端获取到唯一的value值，即能代表自己唯一身份的特征

  2. set值到redis中，如果值key不存在就插入并且设定好过期时间。

  代码:

  ```java
  public class RedisTool {
  
      private static final String LOCK_SUCCESS = "OK";
      private static final String SET_IF_NOT_EXIST = "NX";
      private static final String SET_WITH_EXPIRE_TIME = "PX";
      /**
       * 尝试获取分布式锁
       * @param jedis Redis客户端
       * @param lockKey 锁
       * @param requestId 请求标识
       * @param expireTime 超期时间
       * @return 是否获取成功
       */
      public static boolean tryGetDistributedLock(Jedis jedis, String lockKey, String requestId, int expireTime) {
          String result = jedis.set(lockKey, requestId, SET_IF_NOT_EXIST, SET_WITH_EXPIRE_TIME, expireTime);
          if (LOCK_SUCCESS.equals(result)) {
              return true;
          }
          return false;
      }
  }
  ```

- **解锁**

  1. 首先获取锁对应的value值
  2. 检查是否与requestId相等
  3. 如果相等则删除锁（解锁）。
  4. 要确保上述操作是原子性的。

  ```java
  public class RedisTool {
  
      private static final Long RELEASE_SUCCESS = 1L;
      /**
       * 释放分布式锁
       * @param jedis Redis客户端
       * @param lockKey 锁
       * @param requestId 请求标识
       * @return 是否释放成功
       */
      public static boolean releaseDistributedLock(Jedis jedis, String lockKey, String requestId) {
  
          String script = "if redis.call('get', KEYS[1]) == ARGV[1] then return redis.call('del', KEYS[1]) else return 0 end";
          Object result = jedis.eval(script, Collections.singletonList(lockKey), Collections.singletonList(requestId));
  
          if (RELEASE_SUCCESS.equals(result)) {
              return true;
          }
          return false;
      }
  }
  
  ```

#### 疑问点：

**如果设置锁之后再设置过期时间会怎么样?**

​	有可能在获取锁之后服务挂掉，导致过期时间还未设置，结果锁一直不释放。

**使用`jedis.setnx()`命令实现加锁，其中key是锁，value是锁的过期时间。有可能导致的问题**

执行过程：

1. 通过setnx()方法尝试加锁，如果当前锁不存在，返回加锁成功。
2. 如果锁已经存在则获取锁的过期时间，和当前时间比较，如果锁已经过期，则设置新的过期时间，返回加锁成功。

```java
public static boolean wrongGetLock2(Jedis jedis, String lockKey, int expireTime) {

    long expires = System.currentTimeMillis() + expireTime;
    String expiresStr = String.valueOf(expires);
    // 如果当前锁不存在，返回加锁成功
    if (jedis.setnx(lockKey, expiresStr) == 1) {
        return true;
    }
    // 如果锁存在，获取锁的过期时间
    String currentValueStr = jedis.get(lockKey);
    if (currentValueStr != null && Long.parseLong(currentValueStr) < System.currentTimeMillis()) {
        // 锁已过期，获取上一个锁的过期时间，并设置现在锁的过期时间
        String oldValueStr = jedis.getSet(lockKey, expiresStr);
        if (oldValueStr != null && oldValueStr.equals(currentValueStr)) {
            // 考虑多线程并发的情况，只有一个线程的设置值和当前值相同，它才有权利加锁
            return true;
        }
    }
       
    // 其他情况，一律返回加锁失败
    return false;

}
```

**错误点**

1. 客户端设置的时间，可能导致时间不同步。
2. 如果某一个客户端卡了很长一点时间，导致实际的锁已经过期，并且出现了下一个锁。
3. 因为没有一个唯一标识，所以可能上一个客户端在解锁下一个客户端的锁。

### 多个redis实例

1. 客户端调用方法
2. 服务端获取当前Unix时间，以毫秒为单位。
3. 依次尝试从多个实例，使用相同的key和**具有唯一性的value**（例如UUID）获取锁。向Redis请求获取锁时，客户端应该设置一个网络连接和响应超时时间，这个超时时间应该小于锁的失效时间。这样可以避免服务器端Redis已经挂掉的情况下，客户端还在死死地等待响应结果。如果服务器端没有在规定时间内响应，客户端应该尽快尝试去另外一个Redis实例请求获取锁。
4. 客户端使用当前时间减去开始获取锁时间（步骤1记录的时间）就得到获取锁使用的时间。**当且仅当从大多数**（N/2+1，这里是3个节点）**的Redis节点都取到锁，并且使用的时间小于锁失效时间时，锁才算获取成功**。
5. 如果取到了锁，key的真正有效时间等于有效时间减去获取锁所使用的时间（步骤3计算的结果）。
6. 如果因为某些原因，获取锁失败（没有在至少N/2+1个Redis实例取到锁或者取锁时间已经超过了有效时间），客户端应该在**所有的Redis实例上进行解锁**（即便某些Redis实例根本就没有加锁成功，防止某些节点获取到锁但是客户端没有得到响应而导致接下来的一段时间不能被重新获取锁）。

#### 疑问点

**如果获取到锁之后挂掉了怎么办？**

​	因为设置了有效时间，所有在多少时间后会自动删除掉key，这样就不会导致死锁

**会不会出现两把锁的情况？**

因为每个使用锁的对象需要获取到 （N/2+1），也就是一半的实例，所以就不会出现两把锁的情况

## 参考：

[Redis分布式锁的正确实现方式（Java版）](https://wudashan.cn/2017/10/23/Redis-Distributed-Lock-Implement/#releaseLock-wrongDemo2)