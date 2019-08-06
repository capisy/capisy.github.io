---
title: 面向对象-类的静态属性和静态方法
date: 2019-08-06 19:47:00
tag: php
catalog: true
---

####  静态属性

> 定义：静态属性是该类所有对象共享的变量，任何一个该类的对象去访问它时，取到的都是相同的值，同样任何一个该类的对象去修改它时，修改的也都是同一个变量。

##### 声明一个含有静态属性的类

```php
class Player{
    
  public static $player_count = 0; // 静态属性（public访问修饰符可以省略）
    
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

echo '参加游戏人数：' . Player::$player_count; // 2 
echo "当前游戏人数：{$p1->getTotalPlayer()}"; // 2
```

 ##### self 和 $this 的区别

- self 是类的范畴（指向类）
- $this 是对象实例 （指向对象实例）

#### 静态方法

##### 声明和调用

```
class Person{
  public static function sayHello(){
    echo 'Hello!';
  }
}

Person::sayHello();
```

```
class Worker{

  static $productions =0;

  static function product($num){
    self::$productions += $num;
  }
  
  static function getProductions(){
    return self::$productions;
  }
}

$w1 = new Worker;
$w1->product(5.5);

$w2 = new Worker;
$w2->product(6);

echo 'Productions:'. Worker::getProductions(); // Productions:11.5
```

##### 注意事项

>  成员方法可以使用静态属性，但是静态方法不能使用成员属性

```
class Person{
    public $name='shiyuan';
    
    static function showName(){
        echo $name;
    }
}

Person::showName(); // Undefined variable: name
```

##### 使用静态方法实现单例模式

```
class DBHelper {
  private static $instance = null;

  private function _construct(){
    $this->conn = '数据库连接';
  }

  private function __clone(){}

  public static function getSingleton(){
    if(!(self::$instance instanceof self)){
      self::$instance = new self();
    }
    return self::$instance;
  }

  public function execQuery($sql){
    echo '数据库操作';
  }
}

$db = DBHelper::getSingleton();
```

