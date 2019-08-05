---
title: 面向对象-类的自动加载
date: 2019-08-05 20:45:00
tag: php
catalog: true
---

#### 基础用法

> 当使用一个没有定义的类时，就会自动触发 spl_autoload_register 方法

```php
spl_autoload_register(function($class){
  include 'classes/' . $class . '.class.php';
});
```

#### 优化方案

##### 定义一个类的映射数组

```
$class_map = array(
	'Person'=>'model/person',
	'Config'=>'lib/config',
);
```

##### 调用 spl_autoload_register 方法

```
spl_autoload_register(function($class){
  global $class_map;
  include $class_map[$class] . '.class.php';
});
```

