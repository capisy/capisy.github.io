---
title: 面向对象-继承和多态
date: 2019-08-09 19:20:00
tag: php
catalog: true
---

> 继承不能简单的理解成子类定义时，会把父类的属性声明、方法定义拷贝一份，而是建立了继承查找的关系。

> 在创建某个子类对象时，如果子类没有定义构造函数，则会调用父类的构造函数。如果子类定义了构造函数，则会覆盖父类的构造函数。



#### 子类调用父类方法的两种方式

```
class Father{
    function __construct(){}
    function sayHello(){
        echo 'Hello !';
    }
}
```

```
class Child extends Father{
    function __construct(){
        $this->sayHello(); // 方法一
        parent::sayHello(); // 方法二
    }
}  // 但是不能用方法一（$this）调用父类的__construct构造函数，会递归调用，死循环
```

####  方法重载

> 通过魔术方法实现动态的创建属性和方法

```
class Reload{
	private function sum($args){
		list($n1,$n2) = $args;
		return $n1 + $n2;
	}
	
	private function getMax($args){
        return max($args);
	}

    public function __call($method,$args){
        var_dump($args);
        if($method ==='getVal'){
    		if(count($args)===2){
              return $this->sum($args);   
    		}else {
                return $this->getMax($args);
    		}     
        }
    }
}

$r1 = new Reload;
$r1->getVal(30,40); // 求和
$r1->getVal(30,40,50); // 求最大值
```

