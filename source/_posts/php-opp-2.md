---
title: 面向对象-访问修饰符
date: 2019-08-04 20:32:00
tag: php
catalog: true
---

#### 访问修饰符定义

> 对属性或方法的访问控制，通过在前面添加关键字来是实现

- public-公有：可以在任何地方被访问

- protected-受保护: 可以被自身及其子类或父类访问

- private-私有：只能被其定义所在的类访问

```php
class Clerk {
  public $name;
  protected $job;
  private $salary;

  function __construct($name,$job,$salary){
    $this->name = $name;
    $this->job= $job;
    $this->salary=$salary;
  }
}

$c1 = new Clerk('shiyuan','actor','99999');
echo($c1->name);
echo($c1->job); // Cannot access protected property
echo($c1->salary); // Cannot access private property
```

#### 通过公有的成员方法,访问受保护的和私有的属性

```php
class Clerk {
  protected $job;
  private $salary;

  function __construct($job,$salary){
    $this->job= $job;
    $this->salary=$salary;
  }
    
  function showJob(){
    return $this->job;
  }

  function showSalary(){
    return $this->salary;
  }
}

$c2 = new Clerk('shiyuan','actor','99999');
echo($c2->showJob()); // actor
echo($c2->showSalary()); // 99999
```



