---
title: 基础-数据类型
date: 2019-03-31 11:41:44
tag: php
catalog: true
---

#### 一次声明多个变量
句尾；不能省略，js狗比中指
```php
$a=$b=$c = 123;
```

#### 一次打印多个变量
```php
echo $a,$b,$c;
var_dump($a,$b,$c);
```

#### 字符串类型
- 单引号不解析变量，也不解析转义字符
- 双引号中的变量用{}包裹（不会出错的写法）
- 双引号解析转义字符
- 单引号和双引号都可以解析html标签
```php
echo '\$a<br/>';
echo "\$a<br/>";
echo "{\$a}</br>";
```
- 定界符
```php
$str=<<<dog
 'I'm a coding dog~~'
dog;
```
#### 验证变量
变量不存在的时候不会报错
```php
if(isset($_GET['loving'])){
  echo $_GET['loving'];
}else {
  echo 0;
}
```

####  验证数组
-  数组必须存在
- 不需要验证count（$loving）
```php
$loving = [];
if(!empty($loving)){
  echo '1';
}else {
  echo '0';
}
```
#### list函数对变量解构赋值
- 只能对索引数组解构
- 不会影响原数组
- 对标js的语法弱爆了
```php
$loving = ['sleep','foot ball','movie'];
list($ty1,$ty2,$ty3) = $loving;
echo $ty1."<br/>".$ty2."<br/>".$ty3."<br/>";
dump($loving);
```

####  临时类型转换
不会对原变量产生影响
```php
$var_int = (int)$var;
$var_float = (float)$var;
$var_str = (string)$var;
$var_bool = (bool)$var;
$var_null = (unset)$var;
```

#### 永久类型转换
```php
settype($var,'bool');
// 可以设置的类型
// int,float,string,int,bool,array,object,null
```

#### 判断变量的类型
```php
is_int($var);
is_float($var);
is_string($var);
is_bool($var);
is_scalar($var); // 标量
is_null($var);
is_array($var);
is_object($var);
is_resource($var);
is_numeric($var); // 是否为数值型或字符串型的数字
```
