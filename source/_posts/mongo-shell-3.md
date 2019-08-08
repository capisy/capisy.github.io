---
title: mongo shell 聚合管道
date: 2018-08-08 09:55:00
tag: mongodb
catalog: true
---

```
db.user.insert({name:'sy',age:16,addr:{country:'Japan',city:'Tokyo'}})
```

#### $project:对查找到的结果重新映射（类似于mysql的视图）

```
db.user.aggregate([
	{$project:{_id:0,username:"$name"}}
])
```

```
db.user.aggregate([
	{$project:{_id:0,name:1,city:"$addr.city"}}
])
```

#### $match：匹配

```
db.user.aggregate([
	{$match:{"addr.city":"Tokyo"}},
	{$project:{_id:0}}
])
```

```
db.user.aggregate([
	{$match:{
		$or:[
			{"addr.city":'Tokyo'},
			{ age:{$lt:24} }
		]
	}}
]) // 地址是东京或者年龄小于24
```

####  $limit 和 $skip

```
db.user.aggregate([
	{$limit:1}
])
```

```
db.user.aggregate([
	{$skip: 2}
])
```

#### $unwind：数组字段展开

```
db.user.insert({name:'ycy',addr:['JS','SZ']})
```

```
db.user.aggregate([
	{$unwind:{
		path:"$addr"
	}},
	{$project:{_id:0}}
]) // 把数组字段按值分离成多条数据
// {name:'ycy',adddr:'JS'}
// {name:'ycy',adddr:'SZ'}
```

```
db.user.aggregate([
	{$unwind:{
		path:"$addr",
		includeArrayIndex:'cityIdx' // 新生成的字段在原数组中的索引
	}},
	{$project:{_id:0}}
])
// {name:'ycy',adddr:'JS',cityIdx:NumberLong(0)}
// {name:'ycy',adddr:'SZ',cityIdx:NumberLong(1)}
```

#### $sort：排序

```
db.user.aggregate([
	{$sort:{age:1, name：-1}}
])
```

#### $lookup: 类似于连表查询

```
db.user.inert([
	{name:'ycy',cid:0}
	{name:'sy',cid:1}
])
```

```
db.country.insert([
	{cname:'China',cid:0},
	{cname:'Japan',cid:1}
])
```

````
db.user.aggregate([
	$lookup:{
		from:'country',
		localField:'cid', // user->cid
		foreignField:'cid', // country->cid
		as:'country_info'
	}
])
// {name:'ycy', cid:0, country_info:{cname:'China', cid:0 } }
// {name:'sy', cid:1, country_info:{cname:'Japan', cid:1 } }
````

#### $group:分组

```
db.goods.insert([
	{gtyp:'pc',price:5555,amount:10},
	{gtyp:'pc',price:12555,amount:8},
	{gtyp:'phone',price:3555,amount:12},
	{gtyp:'phone',price:2999,amount:11},
])
```

```
db.goods.aggregate([
	{ $group:{ _id:'$gtyp' } }
])  // [ {'_id':'phone'},{'_id':'pc'} ]
```

```
db.goods.aggregate([
	{$group:{
		_id:'$gtyp',
		amounts:{ $sum:'$amount'},
		totalPrice:{ $sum:{ $multiply:['$price','$amount'] } },
		avgPrice:{ $avg:'$price' },
		count: { $sum:1}, // 分组中的数据条数
		maxPrice: { $max:'$price'},
		minTotalPrice:{ $min:{$multiply:['$price','$amount']} }
	}}
]) 
```

```
db.goods.aggregate([
	{$group:{
		_id:null,
		amounts:{ $sum:'$amount'},
	}}
]) // 不分组，但可以使用计算功能
```

#### $out: 把聚合管道的输出结果写入新的collection

```
db.goods.aggregate([
	{ $group:{ _id:'$gtyp' } },
	{ $out:'output' }
])
```

```
db.output.find()
```

#### allowDiskuse, 在数据量超过100M时，使用磁盘存储

```
db.goods.aggregate([

],{
	allowDiskUse:true // 默认为false
})
```