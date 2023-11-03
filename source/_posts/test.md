---
title: test
date: 2023-11-02 23:31:04
tags:
---



> 哈哈哈哈哈哈哈


```java
package com.hmdp.utils;

/**
 * 生成全局唯一id
 */
@Component
public class RedisIdWorker {
  
    //设置起始时间，我这里设定的是2022.01.01 00:00:00
    //开始时间戳
    private static final long BEGIN_TIMESTAMP = 1640995200L; 
    //序列号的位数
    private static final int COUNT_BITS = 32;

    private StringRedisTemplate stringRedisTemplate;

    public RedisIdWorker(StringRedisTemplate stringRedisTemplate) {
        this.stringRedisTemplate = stringRedisTemplate;
    }

    public long nextId(String keyPrefix) {

        // 1.生成时间戳(当前时间减去开始时间的秒数)

        //获取当前时间
        LocalDateTime now = LocalDateTime.now();
        //获取当前时间的秒数
        long nowSecond = now.toEpochSecond(ZoneOffset.UTC);
        //获取时间戳
        long timestamp = nowSecond - BEGIN_TIMESTAMP;

        // 2.生成序列号
        // 2.1.获取当前日期，精确到天
        String date = now.format(DateTimeFormatter.ofPattern("yyyy:MM:dd"));
        // 2.2.自增长
        long count = stringRedisTemplate.opsForValue().increment("icr:" + keyPrefix + ":" + date);

        // 3.拼接并返回
        return timestamp << COUNT_BITS | count;
    }

    public static void main(String[] args) {
	//定义开始时间
        LocalDateTime time = LocalDateTime.of(2022, 1, 1,0,0,0);
        long second = time.toEpochSecond(ZoneOffset.UTC);
        System.out.println("second = " + second);//1640995200L

    }
}
```

{% meting "60198" "netease" "playlist" "autoplay" "mutex:false" "listmaxheight:340px" "preload:none" "theme:#ad7a86"%}

{% biliplayer 33053034 11 1 %}

{% pen https://codepen.io/ch1ny/pen/yLvENLp?editors=0010 %}
