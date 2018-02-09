---
title: TypeScript语法
date: 2018-01-23 09:01:02
tags: js
---

![typescript](/images/blog/ts/ts.jpg)

<!-- more -->

## ts的安装和使用
```
    cnpm i gulp -g
    cnpm i typescript gulp-typescript --save-dev  //安装  
```
```
    //安装插件
    var gulp = require('gulp'),
        ts = require('gulp-typescript');
    //新建任务
    gulp.task('_ts',function(){
        return gulp.src('./src/ts/**/*.ts') //ts文件目录
        .pipe(ts())
        .pipe(gulp.dest('./src/js')) //编译成的js文件目录
    });
```

## ts语法

##  字符串新特性 （提高开发效率和代码的辨识度）

-  多行字符串

** 语法： 用``作为多行字符串的定界符，用${}作为变量的边界 **

```
var tmpData = {
       title : 'tmp_title',
       content : 'tmp_content',
       content_num : 99,
};

var tmpFoo = function(){
    return 'tmp_content_two';
};

var tmp =`
      <section id="box-wrap">
          <h2>${tmpData.title}</h2>
        <div>
                  <p>${ tmpData.content }</p>  //解析变量
                  <p>${ tmpData.content+ '随便加点内容' }</p> //操作变量
                  <p>${ ++tmpData.content_num }</p>
                  <p>${ tmpFoo() }</p>  //函数调用
       </div>
      </section>
`;
alert(tmp);
```
- 自动拆分字符串
```
    var foo = function(template,name,age){
        console.log(template);
        console.log(name);
        console.log(age);       
    }

    const myName = "MF";
     var getAge = function(){
        return 99;
    };

    foo `My name is ${ myName }, I'm ${ getAge() }`;
```

## 参数类型 
 如果一个变量被指定了类型，再给变量赋值的时候，编辑器会根据类型做一些检查，减少开发时犯错误的几率。
这里的检查是静态类型检查，只是在ts的编译器中提示错误，并不意味着编译后的js代码中的变量变成了强类型。
```
var myName:string = "MF";
myName = 13;
```
以下的情况，编译器同样是会报错。因为编译器的类型推断机制。在声明一个新变量时会做类型检查，确定变量的类型。
```
var myName = "MF";
myName = 13;
```
如果不想限制新声明的变量的类型，可以用any关键字声明变量。
```
var myName:any = "MF";
myName = 13;
```

#### ts 支持的基本类型包括
- any
- string
- number
- boolean
- void (用于声明函数的返回值)
```
  var foo1 = function(): void{
        return 'aaa';
  }

 var foo2 = function(): number{
        return 'aaa';
  }

var foo3 = function(arg:string): number{
    return 111;
}
foo3(12);
```
####  自定义类型
```
class Person{
    name: string;
    age:number;
}
var mf :Person =  new Person();
mf.name = 'mf';
mf.age = 99;
```

#### 默认参数（在参数声明的后面用等号指定参数的默认值）
```
var foo1 = function(a:string,b:string,c:string='ccc'){
    console.log(a);
    console.log(b);
    console.log(c);
}
foo1('aaa','bbb');

var foo2 = function(a:string,b?:string,c:string='ccc'){
    //可选参数不可以声明在必选参数的前面
    console.log(a);
    console.log(b);
    console.log(c);
}
foo2('aaa');
```

## 函数新特性

#### Rest and Spread 操作符（用来声明任意数量的方法参数）
```
function foo(...args){
    //可以传入任意数量的参数  
    args.forEach( (arg) => console.log(arg) );
  }
  foo(1,2,3);
  //foo(4,5,6,7);
```
#### 析构表达式（通过表达式将对象或数组拆解成任意数量的变量）
```
function getStock(){
    return {
        code: 'google',
        price:'100',
    }
}
var {code:codeX,price} = getStock();
console.log(codeX);
console.log(price);
```
```
function getStock(){
    return{
        code:'google',
        price: {
            price1:200,
            price2:400,
        }
    }
}
var {price:{ price2 } } = getStock();

console.log(price2);
```
```
  //用析构表达式从数组中取值
   var array = [0,1,2,3,4];
   var [num0,,num2] = array;
   console.log(num0);
   console.log(num2);

  //var [, , num2, , ...others] = array;
  //console.log(num2);
  //console.log(others);
```
```
  //用析构表达式拆分数组作为函数的参数
   var array = [0,1,2,3,4];
    function fn([num1,num2,...others]){
        console.log(num1);
        console.log(num2);
        console.log(others);
    }
    fn(array);
```

## for of 循环
```
  var array = [0,1,2,3,4];

  //for Each
  array.forEach(i => console.log(i));

  //for of
  for(var i of array){
      if(i> 2) break;
      console.log(i)
  }

  //用for...of 拆分字符串
  for(var n of 'uniplaza'){
      console.log(n);
  }
```

## 外部库的引用
把下载好的对应外部库的*.d.ts 文件，放入*.ts 文件目录下即可。
```
  cnpm i --save-dev @types/jquery
```