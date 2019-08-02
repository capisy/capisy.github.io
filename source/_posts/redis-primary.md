---
title: Redis-CRUD
date: 2019-04-01 17:25:02
tags: redis
catalog: true
---

#### 安装
```linux
https://redis.io/download
```
```linux
wget http://download.redis.io/releases/redis-5.0.4.tar.gz
tar xzf redis-5.0.4.tar.gz
cd redis-5.0.4
make 
```
#### 修改配置文件
> 安装目录 redis.conf
```vi
daemonize yes // 后台启动，默认为no
port 6379 // 这个不建议改 
```
> 如果想要远程连接redis，需要更改下面两项配置
```vi
# bind 127.0.0.1 // 注释掉
protected-mode no // 默认为yes
```
####  redis 启动服务
> 我的配置文件目录：/usr/loca/src/redis/
```linux
redis-server // 默认启动
redis-server redis.conf // 在redis的配置文件目录用配置文件启动
```

#### redis 客户端关闭和启动
```linux
redis-cli shutdown // 关闭
redis-cli -h 10.0.1.188 -p 6379  // 开启
redis-cli -h 10.0.1.188 -p 6379 --raw // 可以识别中文
```
#### redis 数据操作
查看所有数据的key值
```redis
keys *
```
清空所有数据
```redis
flushall
```

#### 字符串
添加
```redis
set key val // set name yjy
get key // get name-> 'yjy'
```
查看
```redis
mset key val [key val] // mset n1 a n2 b n3 c
mget key [key]// mget n1 n2 n3 -> 'a' 'b' 'c'
```
更改
```redis
set age 20 
incr age // 21
decr age // 20
incrby age 10 // 30
decrby age 5 // 25
```
```redis
append key val // 追加
append name -aaa
get name // 'yjy-aaa'
```
删除
```redis
del name // 删除操作
```
#### list
```redis
lpush key val [val]  // 插入一个或多个数据
lpop key // 删除数据，并返回删除的值
llen key // 获取list的长度
lrange books [start,end] // 获取范围内list的值
lrange books 0 -1 // 获取list中所有的值
```
#### hash
添加
```redis
hset hashName key val  // drinks:{ coffee: latte }，一次可以存多个值 
```
查看
```redis
hget drinks coffee  // 一次只能取一个值
hgetall drinks // 一次返回所有值，key value key value... 形式
hvals drinks // 查看所有值
hkeys drinks // 查看所有字段
```
修改
```redis
// 对hash内的数值型字段做增（10）减（-10）
hset xiaomin age 20
hincrby xiaomin age 10  // 没有decrby命令
hget xiaomin age // 30
```
删除
```redis
del name key // 删除操作，可以一次删掉多个
```
判断是否存在
```redis
hexists drink water // 判断hash是否存在某个字段，返回1或0
```
#### set
添加
```redis
sadd key val [val] 
sadd books js php linux
```
查看
```redis
smembers key
smebers books
```
判断是否是成员
```redis
sismember key value
```
随机取出n个成员
```redis
srandmember key amount
srandmember books 3
```
两个集合之间的操作
```redis
sdiff key1 key2 // 差集运算
sinter key1 key2 // 交集运算
sunion key1 key2 // 并集运算
```
#### 有序集合
添加
```radis
zadd key power  value
zadd  code_lang 10000 js
zadd code_lang  5000 php 
```
查询
```radis
zcard key // 查询个数
zcard code_lang // 3
```
```redis
zrange key start  end // 范围查询
zrevrange key start end // 反向查询
zrange code_lang 0 -1 // 查询code_lang 中所有的成员
zrange code_lang  0 -1 withscores // 查询所有成员及权重
```
```redis
zrangebyscore key min max 
zrangebyscore code_lang -inf 2000 // 负无穷到2000内的成员
```
删除
```redis
zrem key val [val]
zrem code_lang java
```

#### 过期时间
以秒为单位
```redis
expire key time
expire xiaomin 50
```
查看剩余秒数
```redis
ttl key
ttl xiaoming
```
setex 设置是加时间
```redis
setex key time val // 只有set可以在设置是加时间
setex schook 100 antu
```
#### 事务
```redis
multi // 事务开始 
incr age
incr age 
....
exec // 事务结束
discard // 回滚
```
