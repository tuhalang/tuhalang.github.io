---
layout: wiki
title: Redis lock
cate1: redis
cate2: spring-boot
description: redis, distributed-lock, spring-boot
keywords: redis, distributed-lock, spring-boot
type:
link:
---

## RedisConfig.java

```java
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.data.redis.connection.jedis.JedisConnectionFactory;
import org.springframework.data.redis.core.RedisTemplate;

@Configuration
public class RedisConfig {

    @Bean
    JedisConnectionFactory jedisConnectionFactory() {
        return new JedisConnectionFactory();
    }

    @Bean
    public RedisTemplate<String, Object> redisTemplate() {
        final RedisTemplate<String, Object> template = new RedisTemplate<>();
        template.setConnectionFactory(jedisConnectionFactory());
        return template;
    }


}
```

## RedisLock.java

```java
import java.util.ArrayList;
import java.util.List;
import java.util.concurrent.TimeUnit;

import org.springframework.data.redis.core.RedisTemplate;
import org.springframework.data.redis.core.script.DefaultRedisScript;
import org.springframework.stereotype.Component;

import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;

@Component
@RequiredArgsConstructor
@Slf4j
public class RedisLock {

    private final RedisTemplate<String, String> redisTemplate;

    public boolean tryLock(String lockKey, String identity, long expireTime, int maxRetries, long previous) {
        int times = 0;
        while (true) {
            if (times < maxRetries) {
                if (lock(lockKey, identity, expireTime)) {
                    return true;
                }
                times += 1;
                log.info("Fail to acquire lock, waiting to retry");
                try {
                    Thread.sleep(previous);
                } catch (InterruptedException e) {
                    log.error(e.getMessage(), e);
                }
            } else {
                log.warn("Cannot acquire lock");
                return false;
            }
        }
    }

    public boolean tryLock(String lockKey, String identity, long expireTime) {
        if (lock(lockKey, identity, expireTime)) {
            return true;
        } else {
            log.warn("Fail to lock, waiting to retry");
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                log.error(e.getMessage(), e);
            }
            return tryLock(lockKey, identity, expireTime);
        }
    }

    public boolean lock(String lockKey, String identity, long expireTime) {
        if (Boolean.TRUE.equals(redisTemplate.opsForValue().setIfAbsent(lockKey, identity))) {
            redisTemplate.expire(lockKey, expireTime, TimeUnit.SECONDS);
            return true;
        }
        return false;
    }

    public boolean releaseLock(String lockKey, String identity) {
        final String luaScript = "if " +
                                 "  redis.call('get', KEYS[1]) == ARGV[1] " +
                                 "then " +
                                 "  return redis.call('del', KEYS[1]) " +
                                 "else " +
                                 "  return 0 " +
                                 "end";
        final DefaultRedisScript<Boolean> redisScript = new DefaultRedisScript<>();
        redisScript.setResultType(Boolean.class);
        redisScript.setScriptText(luaScript);
        final List<String> keys = new ArrayList<>();
        keys.add(lockKey);
        return Boolean.TRUE.equals(redisTemplate.execute(redisScript, keys, identity));
    }
}
```