---
title: php面向对象（1）
date: 2019-08-03 09:57:01
tag: php
catalog: true
---

#### 对象传递机制

```php+HTML
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

![](/images/blog/php/01.jpg)

##### 如果是引用传递

```
$p3 = &$p1;
$p3 = 'shiyuan';
echo $p1; // shiyuan
```

![](/images/blog/php/02.jpg)

f1g1ns1.dnspod.net

f1g1ns2.dnspod.net