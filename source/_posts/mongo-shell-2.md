---
title: mongo shell 更新操作
date: 2018-08-07 14:14:00
tag: mongodb
catalog: true
---

#### update 方法

> 只能更新查询结果集中的第一条数据

```
db.user.update({name:'sy'},{name:'sy',age:16}) // @p1:查询条件 @p2:更新后的文档
```

##### $set 操作符

```
db.user.update({name:'sy'},{$set:{
	age:17,
	addr:['Japan','Tokyo']
}}) // 更新age字段，添加addr字段
```

```
db.user.update({name:'sy'},{$set:{
	'addr.1':'东京'
}}) // 如果文档的参数为对象或数组的时候用'.'操作符选取
```

##### $unset 操作符

```
db.user.update({name:'sy'},{$unset:{
	age:0
	'addr.1':0
}}) // {name:'sy',addr:['Japan',null]}
```

##### $rename 操作符

> $rename中的旧字段和新字段都不可以指向数组元素

```
db.user.update({name:'sy'},{$rename:{
	addr:'location'
}}) // {name:'sy',location:['Japan',null]}
```

```
db.user.update({name:'sy'},{$rename:{
	location:'info.location'
}}) // {name:'sy',info:{ location:['Japan',null] } }
```

##### $inc（increment） 操作符

```
db.user.update({},{$inc:{
	age:-1
}}) // 年龄字段的值减一
```

##### $mul （multiplication:乘法）操作符

```
db.user.update({},{$mul:{
	balance: 0.5
}}) // 余额字段除以2
```

##### $min 和 $max 操作符

```
db.user.update({},{$min:{
	balance: 1000
}}) // 如果更新后的值比原值小则更新，反之不更新
```

```
db.user.update({},{$max:{
	balance: 1000
}}) // 如果更新后的值比原值大则更新，反之不更新
```

> 如果被更新的字段不存在，$min 和 $max 会创建改字段，并把值设为更新的值

##### 与更新数组字段有关的操作符

###### $addToSet : 向数组字段添加新值

```
db.user.update({},{$addToSet:{
	location:'ToKyo'
}}) // 如果数组中没有相同的val则更新数组，反之不更新
```

###### $each：一次向数组字段添加多个新值

```
db.user.update({},{$addToSet:{
	occupation:{
		$each:['actress','idol']
	}
}}) // {...,occupation:['actress','idol']}
```

###### $pop：从数组字段中删除元素

```
db.user.update({},{$pop:{
	location:1
}}) // 1:从数组最后删除一个元素，-1：从数组最前删除一个元素
```

###### $pull：从数组字段中删除特定的元素

```
db.user.update({},{$pull:{
	addr:{
		$regex:/^ja/i
	}
}}) // {... addr:['ToKyo']}
```

##### $ ：query中匹配的元素

```
db.user.update({addr:'ToKyo'},{$set:{
	'addr.$':'东京'
}}) // {...,addr:['东京']}
```