---
title: 面向对象-类的定义
date: 2019-08-03 09:57:01
tag: php
catalog: true
---

#### 定义一个类

```php
class Person {
  public $name;
}
```

#### 对象传递机制

```php
$p1 = new Person; // 如果在实例化对象时没有参数，可省略括号
$p1->name = 'old name';

$p2 = $p1;
echo $p2->name; // 'old name'

$p1->name = 'new name';
echo $p2->name; // 'new name'
```

![示意图](/images/blog/php/01.jpg)

##### 如果是引用传递

```php
$p3 = &$p1;
$p3 = 'shiyuan';
echo $p1; // shiyuan
```

![示意图](/images/blog/php/02.jpg)

#### 成员方法

```php
class Person{
    public function calc($num=10){
     $res =0;
     for($i=1;$i<=$num;$i++){
       $res+=$i;
     }
   	return $res;
  }
}

$p4 = new Person;
echo($p4->calc()); //55
echo($p4->calc(100)); // 5050
```

#### 构造函数

- 作用：完成新对象实例的初始化

- 特点：

  （1）没有返回值 

  （2）在创建新实例时，系统会自动调用  

  （3）构造函数的访问修饰符可以省略

  （4）如果没有定义，系统会自动生成一个空的构造函数

```php
class Person {
  public function __construct($name){
    $this->name = $name;
  }
};

$p5 = new Person('shiyuan');
echo($p5->name); // shiyuan
```

#### 成员方法中调用构造函数中的属性

```php
class Person {
  function __construct($name){
    $this->name = $name;
  }
  public function showName(){
    return $this->name;
  }
};

$p6 = new Person('十元');
echo($p6->showName()); // 十元
```

#### 析构函数

> 析构函数会在到某个对象的所有引用都被删除或者当对象被显式销毁时执行。

```php
class Person {
  function __construct($name){
    $this->name = $name;
  }
  function __destruct(){
    print "Destroying " . $this->name . "\n";
  }
};

$p7 = new Person('shiyuan');
```

##### 析构函数的用途 (用于当前文件代码量较大时)

- 希望在程序没有结束前，及时销毁对象创建的资源，eg：数据库连接

```php
class HeavySource{
    function __construct($source){
        $this->source=$source;
    }
    function __destruct(){
        // 手动销毁 heavy source
    }
}
$s1 = new HeavySource('heavy source');
....
....
$sl = null; // 释放资源
....
....
```

#### php中的垃圾回收机制

##### 垃圾是如何定义的

> 判断是否为垃圾，主要看有没有变量指向变量容器zval，如果没有则认为是垃圾，需要释放。

##### 老版本中如何产生内存泄漏垃圾

> 环形引用

```
$a = ['one'];
$a[] = &$a;
```

##### 解决方法（GC算法）

> 对zval中的每一个元素进行一次refcount减一操作，操作完成后，如果zval的refcount=0，那么zval就是一个垃圾

