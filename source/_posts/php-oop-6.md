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

#### 属性重载

```
class Person{
    private $prop_array = array();
    
    function __set($prop_name,$val){
        $this->prop_array[$prop_name] = $val;
    }
    
    function __get($prop_name){
        if(isset($this->$prop_array[$prop_name])){
            return $this->$prop_array[$prop_name]
        }else {
            return '属性不存在';
        }
    }
}
```

#### 方法重写

> 如果子类需要访问父类的方法

```
class Animal{
    function cry(){
        echo '父类的cry方法...';
    }
}
```

- 如果子类不含有父类的方法

```
class Cat extends Animal{
    function cry2(){
        parent::cry(); // 第一种调用方式
        $this->cry(); // 第二种调用方式
    }
}
```

- 如果子类含有父类的方法

```
class Dog extends Animal{
    function cry(){
        parent::cry();
    }
} // 如果使用$this的方式调用就会递归死循环
```

##### 注意事项

1.子类方法的参数个数、参数名称要和父类方法的参数个数、参数名称保持一致

2.如果父类方法的参数使用了类型约束，还必须保证数据类型一致

```
class A {
    function mergeArr(array $arr1, array $arr2){
        return array_merge($arr1,$arr2);
    }
}
```

```
class B extends A {
    function mergeArr(array $arr1, array $arr2){
        echo '重写父类的mergeArr';
    }
}
```

3.子类方法不能缩小父类方法的访问权限

````
class A {
    protected function foo(){}
}
````

````
class B extends A {
    public function foo(){}
}
````

#### 多态

> 在面向对象中，对象在不同情况下的多种状态。

```
class Animal{
    public $name;
    function __construct($name){
        $this->name =$name;
    }
    function showInfo(){
        return $this->name;
    }
}

class Cat extends Animal{}
class Dog extends Animal{}
```

```
class Food{
    public $food;
    function __construct($food){
        $this->food = $food;
    }
    function showFood(){
        return $this->food;
    }
}

class Fish extends Food{}
class Bone extends Food{}
```

```
class Master{
    function feed(Animal $animal, Food $food){
        echo '<br>'.$animal->showInfo() . '喜欢吃' . $food->showFood();
    }
}
```

```
$c1 = new Cat('波斯猫');
$d1 = new Dog('二哈');

$f1 = new Fish('<・)))><<');
$b1 = new Bone('骨头');

$xiaobai = new Master;
$xiaobai->feed($c1,$f1);
$xiaobai->feed($d1,$b1);
```

