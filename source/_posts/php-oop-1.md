---
title: php面向对象（1）
date: 2019-08-03 09:57:01
tag: php
catalog: true
---

#### 对象传递机制

```php
class Person {
  public $name;
}
```

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

- 特点：（1）必须有访问修饰符 （2）不可直接调用

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

#### 构造函数 （可选）

- 作用：完成新对象实例的初始化
- 特点：（1）没有返回值 （2）在创建新实例时，系统会自动调用

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
  public function __construct($name){
    $this->name = $name;
  }
  public function showName(){
    return $this->name;
  }
};

$p6 = new Person('十元');
echo($p6->showName()); // 十元
```

