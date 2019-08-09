---
title: mongo shell 索引
date: 2018-08-09 08:34:00
tag: mongodb
catalog: true
---

> 索引就是对指定的字段进行排序的数据结构,可以加快文档查询和文档排序的速度。
>
> 复合索引只支持前缀子查询

####  创建索引

##### 创建单键索引

```
db.user.createIndex({ name:1 }) // 为name字段创建索引
```

##### 创建复合键索引

```
db.user.createIndex({ name:1, age:-1 }) // 顺序很重要(前缀子查询)
```

##### 创建多键索引

```
db.user.createIndex({ addr:1 }) // 针对数组字段建立的索引
```

##### 创建唯一性索引

```
db.user.createIndex({ name:1 },{ unique:true })
```

##### 创建稀疏性索引（ 只将包含索引字段的文档加入到索引中 ）

```
db.user.createIndex({ addr:1 },{ sparse:true })
```

##### 查看当前collection的索引

````
db.user.getIndexes()
````

#####  explain,查看数据库查询过程

```
db.user.explain().find({ name:1 })
```

#### 删除索引

```
db.user.dropIndex({ name:1 })
```

#### 索引的生存时间(删除document)

> 针对日期字段，或者包含日期元素的数组字段，可以使用设定了生存时间的索引来自动删除字段值超过生存时间的文档

```
db.user.insert({ name:'sy', lastAccess:new Date() }) // 这里用时间戳是无效的
```

```
db.user.createIndex({ lastAccess:1 },{  expireAfterSeconds:20 })
```

