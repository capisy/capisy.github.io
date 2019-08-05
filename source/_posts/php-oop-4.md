---
title: 面向对象-类的静态属性和静态方法
date: 2019-08-05 21:47:00
tag: php
catalog: true
---

####  静态属性

##### 声明一个含有静态属性的类

```php
class Player{
    
  public static $player_count; // 静态属性（public访问修饰符可以省略）
    
  function __construct($name){
    $this->name = $name;
  }
    
  function addPlayer(){
    echo("{$this->name}加入游戏<br/>");
    self::$player_count++;
  }
    
  function getTotalPlayer(){
    return self::$player_count;
  }
}
```

##### 实例化类，获取静态属性的值

```php
$p1 = new Player('李狗蛋');
$p1->addPlayer(); // 李狗蛋加入游戏

$p2 = new Player('王小花');
$p2->addPlayer(); // 王小花加入游戏 

echo("<br/>当前游戏人数：{$p1->getTotalPlayer()}"); // 当前游戏人数：2 
```



 