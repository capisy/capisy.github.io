---
title: 基础-函数篇
date: 2019-03-31 21:35:02
tag: php
catalog: true
---

#### 设置文件头信息
```php
header('content-type:text/html;charset=utf-8');
```
#### 日期、时间函数
设置当前时区为中国
```php
date_default_timezone_set('PRC');
```
时间戳
```php
time();
```
获取日期时间
```php
$cur_date = date('Y-m-d H:i:s');
// 根据时间戳获取时间
$special_date = data('Y-m-d H:i:s',time());
```
获取星期信息
```php
$arr_days = ['星期日','星期一','星期二','星期三','星期四','星期五','星期六'];
$today_day = date('w'); // 获取当前是一个星期的第几天
echo "今天是{$arr_day[$today_day]}";
```

#### 数学函数
```php
// 随机数函数
mt_rand(min,max);
echo mt_rand(0,255);
```
```php
// 取整函数
$num_ceil = ceil(3.14) // 4
$num_floor = floor(3.14) // 3

// @ param1:需要四色五入的数
// @ param2:保留到小数点后几位
$num_round = round(3.1419526,2);  // 3.14 
```
```php
//最大值和最小值
$max = max(1,2,3); // 3 
$min = min(1,2,3); // 1
```
```php
// 数字格式化，以千位分隔符形式转换
// @ params1: 格式化的数
// @ params2：保留到小数点后几位
$format_num = number_format(111222333.444555,2); // 111,222,333.44
```
#### 字符串相关的函数
```php
// 获取字符串长度
$str = 'mfyj';
$len = strlen($str) 
```
```php
// 字符串大小写转换
$str = 'wish you have a good ending ';
$toUpper = strToUpper($str);
$toLower = strToLower($str);
$ucFirst = ucFirst($str); // 句子的首字母转换为大写
$ucWords = ucWords($str); // 句子的每个单词首字母转换为大写
```
```php
// 字符串替换
$str = 'mdZz';
$str2 = str_replace('z',' hello',$str); // 区分大小写
$str3 = str_ireplace('z',' hello',$str); // 不区分大小写
echo $str2; // mdZ hello
echo $str3; // md hello hello
echo $str; // mdZz
```
```php
$html = htmlSpecialChars($str); // 转换html中的标签
```
```php
// 删除收尾空格,不会改变原来的字符串
$str2 = lTrim($str);
$str3 = rTrim($str);
$str4 = trim($str);
```
```php
// 获取当前字符在字符串中第一次出现的位置,类似js中的indexof
$pos = strPos('javascript','t'); // 区分大小写
$posIgnore = striPos('javascript','T'); // 不区分大小写
```
```php
// 字符串截取
// @ param $end：可选参数，缺省为到字符串末尾
$subStr = subStr($str,$start,$end);
```
```php
// 字符串反转
$str2 = strRev('javascript');
```
```php
// 字符串md5
$str_md5 = md5($str);
```
```php
// 字符串打乱
$str_shuffle = str_shuffle('abcdef');
```
```php
// 字符串分割
// @ param1: 分隔符
// @ param2: 被分割的字符串
// @ ret: []
$str_arr = explode(' ',$str);
// 数组的拼接
$str = implode('',$str_arr);
```
```php

```

#### 局部静态变量
```php
function foo(){
	static $var = 0;
	$var++;
}
```
> 用js代码表示
```js
const foo = (function(){
	let _var = 0
	return function(){
		_var++
	}
}())
```
#### 全局变量
```php
$a = 1;
$b = 2;
function global_var(){
	global $a,$b; // 函数内部使用全局变量需要先声明
	$a = 3; // 函数内部可以改变全局变量的值
}
global_var();
echo $a; // 3
```
引用传递,可以理解为变量的浅拷贝（地址引用）
```php
$a = 1;
functon foo(&$a){
	$a = 2;
}
foo($a);
echo $a; // 2
```
```php
$obj = new StdClass()
$obj->a = 3;
function fun($o){
 $o->b = 4;
}
fun($obj);
print_r($obj); // a=3,b=4

```
#### 函数参数可以设置默认值
> 设置默认值的变量必须放到最后
```php
function foo($a=3,$b=4){
 echo $a,$b;
}
```
#### 可变参数
```php
function foo(){
  print_r(func_get_args()); // [1,2,3]
  echo func_num_args; // 参数数量
}
foo(1,2,3);
```
> 用js代码表示
```js
function foo(){
	let args = arguments
	console.log(args,args.length)
}
```
#### 参数类型
> 可选参数： array、stdClass、callable（回调函数）
> 在指定了参数类型后，如果参数类型不匹配程序会报错
```php
function foo(array $arr){
	print_r($arr);
}
foo([1,2,3]) 
```
#### 函数嵌套
> php中所有函数都是全局作用域
> 如果要在全局调用函数内部嵌套的函数，需要先执行外部的函数
```php
function foo(){
	function bar(){
		echo 'MDZZ,傻逼才会这么用函数';
	}
}
foo();
bar();
```
> 嵌套函数如果想使用局部变量，需要使用匿名函数use传值的方式
```php
function foo(){
  $var = 1;
  $bar = function() use($var){
    echo $var;  // 1
  };
  $bar();
};

foo();
```
```php
function foo(){
  $var = '局部变量';
  return function($out) use($var){
    echo $out; // 外部传入的参数 
    echo $var; // 局部变量
  };
};

$res_fn = foo();
$res_fn('外部传入的参数');
```