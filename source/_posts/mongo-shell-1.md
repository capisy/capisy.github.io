---
title: mongo shell 增加、删除、查询操作
date: 2018-08-06 09:55:00
tag: mongodb
catalog: true
---

#### 开启mongo shell

- 配置path
- cmd -> mongo

####  mongo shell 操作

##### 新建数据库

```
use mf_egg
```

##### 显示数据库列表

```
show dbs // 这里没有显示mf_egg,因为新建的数据库是空的
```

##### 添加一条数据

```
db.user.insertOne({name:'shiyuan'}) // 新建一个名为user的集合，并插入一条数据
```

```
show dbs // 显示mf_egg 数据库
show collections // user
```

##### 删除一条数据

```
db.user.remove({name:'shiyuan'})
```

##### 删除一个集合

```
db.user.drop() // 如果一个数据库中没有集合存在，数据库会被自动删除
```

##### 一次添加多条数据

```
db.user.insertMany([{name:'十元'}，{name:'sy'}])
```

##### 使用insert插入数据（insert = insertOne + insertMany）

```
db.user.insert({name:'石原里美',contact:['Japan','Tokyo']})
```

##### 查询单条数据

````
db.user.findOne({name:'石原里美'})
````

###### 使用$all修饰符查询

```
db.user.findOne({contact:{$all:['Japan','Tokyo']}})
```

###### 使用$gt(大于)，$lt(小于)修饰符查询

```
db.user.insertOne({name:'sy',age:16})
db.user.findOne({age:{$gt:10,$lt:20}}) // 查询年龄在10-20岁之内的数据
```

###### 使用$elemMatch修饰符查询

> $elemMatch的匹配条件必须是一个Object
>
> 文档中只要有一个元素满足查询条件即可

```
db.user.insertOne({name:'sy',tel:[1550000]})
db.user.insertOne({name:'shiyuan',tel:[155，1550000]})

// 查询tel>1000000的数据
// 上面的两条数据都可以被查询到
db.user.findOne({tel:{$elemMatch:{$gt:1000000}}}) 
```

###### $all和$elemMatch 配合使用

```
db.user.insertOne({name:'sy',tel:[1550000,5550000]})

// 查询电话号码同时在1000000-2000000和5000000-6000000的数据
db.user.findOne(
	{
		tel:{
			$all:[
				{ $elemMatch:{$gt:1000000,$lt:2000000} }
				{ $elemMatch:{$gt:5000000,$lt:6000000} }
			]
		}
	}
)
```

###### 正则表达式修饰符

```
db.user.find({name:/^s/}) // 查找name以's'开头的数据
```

###### $pattern + $all

```
db.user.find({ name:{$all:[/^s/,/y$/]} }) // 查找name以's'开头且以'y'结尾的数据
```

###### $pattern + $in

```
db.user.find({ name:{$in:[/^s/,/y$/]} }) // 查找name以's'开头或以'y'结尾的数据
```

##### skip 和 limit 方法

``` 
db.user.find({name:'sy'}).limit(5) // 从结果集中取前5条数据
```

```
db.user.find({name:'sy'}).skip(2) // 返回结果集中前两条数据之外的数据
```

```
db.user.find({name:'sy'}).skip(2).limit(5) // 从结果集的第三天数据开始，选取之后的5条数据
```

##### count 方法：返回结果集的length

```
db.user.find({name:'sy'}).count() // 假设结果集中有6条数据
```

```
db.user.find({name:'sy'}).limit(1).count()  // 6
```

```
db.user.find({name:'sy'}).limit(1).count(true) // 1 
```

```
db.user.find({name:'sy'}).skip(1).count(true) // 5
```

##### sort 方法（排序）

```
db.user.find().sort({name:1}) // 按照name递增排序
```

```
db.user.find().sort({name:1,age:-1}) // name递增，age递减排序
```

##### sort + limit：查询最xx的一条数据

```
db.user.find().sort({age:-1}).limit(1) // 查询年龄最大的一条数据
```

##### sort, skip, limit 的执行顺序

```
db.user.find().sort().skip().limit()
```

##### 查询指定的字段

> 除了主键文档外，不可以在投影文档中混用包含和不包含操作

```
db.user.find({},{name:1,_id:0}) // 只查询 name 字段
```

```
db.user.find({},{age:0}) // 查询除了 age 之外的所有字段
```

###### $slice 操作符返回数组字段中的部分元素

```
db.user.find({},{contact:{ $slice:1 }}) // $slice的取值为返回数组的前几项
```

```
db.user.find({},{contact:{ $slice:-1 }}) // 返回数组的后几项
```

```
db.user.find({},{contact:{ $slice:[1,2] }}) // 类似 skip(1),limit(2) 操作
```

###### $elemMatch 操作符可以返回数组字段中满足筛选条件的第一个元素

```
db.user.insert({name:'sy',location:['Japan','Tokyo','xxx']})
```

```
db.user.find({},{
	_id:0,
	name:1,
	location:{
		$elemMatch:{ $gt:'Japan' }
	}
}) // {name:'sy',location:['Tokyo']}
```