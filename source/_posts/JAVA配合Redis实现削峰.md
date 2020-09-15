## JAVA配合Redis实现削峰## 限流思路图
![限流思路图](https://myblog-1252842020.cos.ap-guangzhou.myqcloud.com/xuefneg_1574578240013.png)
## 实现代码
**生产者**
```java
/**
 * 提交订单队列
 *
 * @param userId      用户id
 * @param commodityId 订单id
 * @param key         唯一key值
 * @param request     请求
 * @return 返回提交结果
 */
@PostMapping("/participate")
public ResultMsg participate(@RequestParam("userId") Long userId
        , @RequestParam("commodityId") Long commodityId
        , @RequestParam("key") String key
        , HttpServletRequest request) {

    //获取用户排队状态
    BoundHashOperations<String, Object, Object> boundHashOps = redisTemplate.boundHashOps("active:kill");

    //1. 查询当前秒杀状态
    String status = (String) boundHashOps.get(userId + "");
    //2. 判断是否已有状态保存,如果等于null表示没有在排队
    if (null == status) {
        //进入队列
        if (addQueue(userId, commodityId)){
            // 排队成功
            redisTemplate.opsForValue().set("active:kill:" + userId, "2", 1, TimeUnit.DAYS);
            status="2";
        } else {
            // 排队失败
            redisTemplate.opsForValue().set("active:kill:" + userId, "1", 1, TimeUnit.SECONDS);
            status="1";
        }
    }

    ResultMsg msg = null;
    if ("2".equals(status)){
        msg =  new ResultMsg(200, true, "正在排队中");
    } else if ("3".equals(status)){
        msg =  new ResultMsg(200, false, "购买成功");
    } else {
        msg =  new ResultMsg(402, false, "当前人数过多");
    }
    request.setAttribute("body", JSON.toJSONString(msg));
    request.setAttribute("key", key);

    return msg;
}

/**
 * 进入队列
 * @param userId 用户id
 * @param commodityId 商品id
 * @return 返回进入状态  true成功  false失败
 */
private boolean addQueue(Long userId, Long commodityId) {
    //判断当前队列长度
    Long size = redisTemplate.boundListOps("active:exChange").size();
    if (size >= 1000) {
        return false;
    }
    System.out.println("进入排队状态");
    // 返回值的判断
    redisTemplate.opsForList().rightPush("active:exChange", userId + "_" + commodityId);
    return true;
}
```
**消费者**
```java
package com.panshi.qssys.tesk;

import com.panshi.qssys.marketing.service.SecondKillService;
import com.panshi.qssys.share.exception.BusinessException;
import org.apache.dubbo.config.annotation.Reference;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.data.redis.core.BoundHashOperations;
import org.springframework.data.redis.core.BoundListOperations;
import org.springframework.data.redis.core.BoundValueOperations;
import org.springframework.data.redis.core.StringRedisTemplate;
import org.springframework.scheduling.annotation.Scheduled;
import org.springframework.stereotype.Component;

import java.util.concurrent.ExecutorService;

/**
 * @description: 商品秒杀定时任务
 * @author: 蒋文豪
 * @create: 2019/08/05
 */
@Component
public class SecondTask {

    /**
     * redis操作对象
     */
    @Autowired
    private StringRedisTemplate redisTemplate;

    /**
     * 秒杀功能服务接口
     */
    @Autowired
    private SecondKillService secondKillService;

    @Autowired
    private ExecutorService executorService;

    /**
     * 秒杀队列
     */
    @Scheduled(cron = "0/1 * * * * ?")
    public void secondKillQueue() {
        System.out.println("定时任务启动");
        //获取到队列消息
        BoundListOperations<String, String> boundListOps = redisTemplate.boundListOps("active:exChange");
        //排队状态
        if (boundListOps.size() <= 0) {
            return;
        }
        System.out.println(boundListOps.size());
        System.out.println("获取到任务消息");
        //循环10次
        for (int i = 0; i < 10; i++) {
            //使用线程执行
            executorService.submit(() -> {
                //1. 获取到队列中最左边的节点，并删除
                String ids = boundListOps.leftPop();
                System.out.println("ids" + ids);
                if (null == ids) {
                    return;
                }
                //2. 将节点拆分出用户id和商品id
                String[] idArray = ids.split("_");
                if (null == idArray || 2 != idArray.length) {
                    return;
                }
                BoundValueOperations<String, String> valueOperations = redisTemplate.boundValueOps("active:kill:" + idArray[0]);
                //3. 对商品进行消费
                boolean b = secondKillService.submitOrder(Long.parseLong(idArray[0]), Long.parseLong(idArray[1]));
                //4. 修改用户排队状态未已完成
                if (b) {
                    valueOperations.set("3");
                }
            });
        }

    }
}
```