---
title: Redis-PHP操作-有序集合
categories: Redis   
tags: Redis
keywords: [Redis,zset]
description: Redis的数据类型-zset(有序列表)

---

[TOC]

# Redis-PHP操作-有序集合
  Redis 数据库是key-value类型存储方式, 支持的数据类型有 `字符串`(string)、 `哈希`(Hash)、 `列表`(list)、 `集合`(set)、 `有序集合`(Zset). 

  现在学习一下**有序列表**类型的相关操作
**1. 在列表中添加或者更新成员 `zadd`**
``` php
 /*@param key 
  *@param score
  *@param member
  *@return int 成功为1, 否则为0
  *@更新时, 成功也是为0
  */
 $this->redis->zadd($key, $score, $member);
```

**2. 获取有序集合的成员数 `zcard`**
```php
 /*@param key 
  *@return int 有序集合的数量, 不存在key 返回0
  */
 $this->redis->zcard($key);
```

**3. 计算在有序集合中指定区间分数的成员数 `zcount`**
```php
 /*@param key 
  *@param min
  *@param max
  *@return int 获取score分数min/max之间的成员数
  */
 $this->redis->zcount($key, $min, $max);
```

**4. 有序集合中对指定成员的分数加上增量 N `zincrby`**
```php
 /*@param key 
  *@param N
  *@param member
  *@return float 增加后的分数, 若key不存在, 则创建该key, 并赋值为N
  */
 $this->redis->zincrby($key, $N, $member);
```

**5. 计算有序集合的交集并将结果存储在新的有序集合key中 `zinterstore`**
```php
  /*@param newKey 
  *@param array(key1, key2,...)
  *@return int 交集的数量
  */
 $this->redis->zinterstore($newKey,array($key1, $key2...));
```
**注:** 交集时获取交集中分数最大的值

**6. 获取有序集合中计算指定字典区间内成员数量 `zlexcount`**
```php
 ZLEXCOUNT key min max  # 获取key中min-max间的成员数量
```

**7. 获取索引区间返回有序集合指定区间内的数量 `zrange`**
```php
 ZRANGE key start stop [WITHSCORES]
  /*@param key 
  *@param start
  *@param stop
  *@param [WITHSCORES] true/false
  *@return int 交集的数量
  */
 // 返回member=>score 形式
 $this->redis->zrange($key,$start,$stop,true);
 // 返回member的数组
 $this->redis->zrange($key,$start,$stop,false);
```

**8. 获取字典区间有序集合的成员 `ZRANGEBYLEX`**
```php
 ZRANGEBYLEX key min max [LIMIT offset count]
```

**9. 通过分数返回有序集合指定区间内的成员 `ZRANGEBYSCORE`**
```
 ZRANGEBYSCORE key min max [WITHSCORES] [LIMIT] 
```

**10. 获取有序集合中指定成员的索引 `ZRANK`**
```
 ZRANK key member
```

**11. 移除有序集合中成员 `zrem`**
```
 ZREM key member [member1 member2 ... ]
```

**12. 移除有序集合中给定的字典区间的所有成员 `zremrangebylex`**
```
 ZREMRANGEBYLEX key min max
```

**13. 移除有序集合中给定的排名区间的所有成员 `zremrangebyrank`**
```
 ZREMRANGEBYRANK key start stop
```

**14. 移除有序集合中给定的分数区间的所有成员 `zremrangebyscore`**
```
 ZREMRANGEBYSCORE key min max
```

**15. 返回有序集中指定区间内的成员，通过索引，分数从高到底 `ZREVRANGE`**
```
 ZREVRANGE key start stop [WITHSCORES]
```

**16. 返回有序集中指定分数区间内的成员，分数从高到低排序 `ZREVRANGEBYSCORE`**
```
 ZREVRANGEBYSCORE key max min [WITHSCORES]
```

**17. 获取有序集合中指定成员的排名，有序集成员按分数值递减(从大到小)排序 `ZREVRANK`**
```
 ZREVRANK key member
```

**18. 获取成员的分数值 `ZSCORE`**
```
 ZSCORE key member 
```

**19. 给定的一个或多个有序集的并集，并存储在新的 key 中 `ZUNIONSTORE`**
```
 ZUNIONSTORE destination numkeys key [key1 ...]
```

**20. 迭代有序集合中的元素（包括元素成员和元素分值）`ZSCAN`**
```
 ZSCAN key cursor [MATCH pattern] [COUNT count]
```

Author [@Snaker95][1]

[1]: http://www.sharedsea.com


