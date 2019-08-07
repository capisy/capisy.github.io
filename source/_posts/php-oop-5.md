---
title: 面向对象-封装、继承和多态
date: 2019-08-07 19:16:00
tag: php
catalog: true
---

#### 封装

##### php提供三种访问控制符来控制方法和属性的访问权限

|        | public | protected | private |
| :----: | :----- | :-------- | :------ |
| 类内部 | true   | true      | true    |
| 继承类 | true   | true      | false   |
| 类外部 | true   | false     | false   |

##### 访问和修改受保护和私有属性的方法

###### 使用魔术方法 

```php
class Person{
	protected $age;
	private $salary;
  function __construct($age,$salary){
      $this->age = $age;
      $this->salary = $salary;
  }
  function __get($prop){
      if(isset($this->$prop)){
          return $this->$prop;
      }
  }
  function __set($prop,$val){
      if(isset($prop)){
          $this->$prop = $val;
      }
  }
}

$p1 = new Person(16,99999);
echo $p1->age; // 16
$p1->salary = 100000;
echo $p1->salary; // 100000
```

###### 通过 getX 和 setX 方法

```php
class Book {
  private $amount; // 销量

  function __construct($amount){
    $this->amount = $amount;
  }

  function setAmount($num){
    if(is_int($num)){
      $this->amount = $num;
    }else{
      echo '请输入一个数字';
    }
  }

  function getAmount($validate){
    if($validate){
      return $this->amount;
    }else{
      echo '没有访问权限';
    }
  }
}
```

#### 对象运算符的链式调用

```php
class Student {
  private $school;

  function setSchool($school){
    $this->school = $school;
  }

  function getSchool(){
    return $this->school;
  }
}

class School{
  private $myClass;

  function setClass($class){
    $this->myClass= $class;
  }

  function getClass(){
    return $this->myClass;
  }
}

class MyClass{
  public $className;
}

$myClass = new MyClass;
$myClass->className ='ub2010';

$school = new School;
$school->setClass($myClass);

$zcj = new Student;
$zcj->setSchool($school);

$className = $zcj->getSchool()->getClass()->className;
echo $className; // ub2010
```

