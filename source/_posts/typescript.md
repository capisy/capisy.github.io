---
title: TS小记
tag: typescript
date: 2018-01-21 08:23:12
catalog: true
---

#### 数组的两种声明方式

```typescript
const list1:number[] = [1,2,3]

const list2:Array<string> = ['mf'] // 泛型数组
```

##### 二维数组

```
const list3:number[][] = [
	[1,2,3],
	[4,5,6]
]
```

##### 元组类型

> 元组和数组类似，只不过元组是一种**固定长度的数组，每个元素有自己的类型**。

```
const x:[string,number] = ['yjy',16]
```

#### 接口

##### 声明和实例化

```typescript
interface IPerson {
  name: string,
  age: number,
  loving: string[],
  sex?: 'male' | 'famale'
}

const mf: IPerson = {
  name: 'mf',
  age: 27,
  loving: ['zcj', 'yjy'],
  sex: 'male'
}
```

##### 接口允许有任意属性

一旦定义了任意属性，那么确定属性和可选属性的类型必须是它类型的子集

```
interface IPerson{
	[propName:string]:any
}
```

##### 接口的只读属性定义

```
interface IPerson{
	readonly name:string
}
```

#### 函数的定义

##### 普通函数

```
const func = (a:number,b:string):string => a+b
```

##### 可选参数

```
const func2 = (a: string, b?: string): string => {
  return b ? `${a} ${b}` : a
}
```

##### 参数的默认值

```
const func4 = (a:string,b:string='yjy') => `${a} ${b}`
```

##### 用接口定义函数

```
interface IFunc {
  (a: string, b: string): string
}

const func3: IFunc = (a, b) => a + b
```

#### 泛型

> 解决类、接口、函数的复用性，以及对不特定数据类型的支持
>
> 说人话，数据的类型跟根据调用时的数据类型决定的，表示的参数是什么就返回什么类型

##### 泛型定义函数

```
function getValue<T>(value:T):T{
	return value
}

getValue<string>('yjy')
getValue<boolean>(true)
```

##### 泛型定义接口的两种方式

```
interface IFunc{
	<T>(value:T):T
}

const getVal2:IFunc = val => val
getValue<string>('yjy')
```

```
interface IFunc2<T>{
	(value:T):T
}

const getVal3:IFunc<string> = val => val
getVal3('yjy')
```

##### 泛型定义类

```
class Identity<T1, T2>{
  public p1: T1  // public可省略
  private p2: T2
  constructor(p1: T1, p2: T2) {
    this.p1 = p1
    this.p2 = p2
  }
  getP1(): T1 {
    return this.p1
  }
  showP2(): void {
    console.log(this.p2)
  }
}

let mf = new Identity<string, number>('yjy', 16)
```

##### 泛型约束

问题: 不能确定T是否具有length属性，会报错

```
const getLength = <T>(arg: T): number => arg.length 
```

解决方法:

```
interface IWithLenght{
  length:number
}

const getLength = <T extends IWithLenght>(arg: T): number => arg.length
```

###### 泛型约束设置默认类型

问题：如果大多数情况下T的类型都为string，每次都要声明会很麻烦

```
interface IMyTyp<T>{
	value:T
}

let a:IMyTyp<string>{
	value:'hello world'
}
```

解决：

```
interface IMyTyp2<T=string>{
	value:T
}

let b:IMyTyp2={
	value:'hello !'
}

let c:IMyTyp2<number>={
	value:123
}
```

#### 命名空间

##### 语法

> 命名空间引入了新的作用域，大括号可以包含任意合法的代码
>
> 要在命名空间之外访问命名空间内的成员，必须使用export关键字

```
namespace ns{
	export const a:string = 'hello world'
	const b:string = 'hi world'
}

const c:string = ns.a // √
const d:string = ns.b // ×
```

实现原理：立即执行函数

```
var ns;
(function (ns) {
    ns.a = 'hello world';
    var b = 'hi ts';
})(ns || (ns = {}));
```

##### 空间拆分

>命名空间可以拆分，代码量大时可以提高可维护性
>
>拆分的代码块之间的成员无法互相访问
>
>访问需要export 导出

```
namespace ns{
	const a:number
	export function showA():void{
		console.log(a)
	}
}

namespace ns{
	const b:number = a + 1 // ×
	showA() // √
}
```

##### 空间嵌套

> 作用域为{}，作用域外不可访问

```
namespace ns {
  namespace subNs{
    const a:string = 'yjy'
    export const b:string = 'yjy'
  }
  const c:string = subNs.a // ×
  const d:string = subNs.b // √
}
```

> 访问嵌套空间内的属性，嵌套空间也需要export

```
namespace ns {
  export namespace subNs{
    export const e:string = 'yjy'
  }
}

const f:string = ns.subNs.e
```

#### 类类型

##### 类类型中可以把实例属性和静态属性直接定义在类内部

```
// es6
class Foo{
	constructor(){
		this.test = 'hi'
	}
}
Foo.test1 = 'world'
```

```
// ts
// 推荐写法
class Foo{
	test:string = 'hi' // test属性必须先声明再使用
	constructor(test:string){
		this.test = test 
	}
}

// 另外一种写法
class Foo{
	constructor(public test:string){
		this.test = test
	}
}
```

```
// ts
// 静态属性：只能由构造器调用的属性
class Foo{
	static test1:string = 'world'
}
```

##### 抽象类和抽象方法

> 抽象类和抽象方法都由关键字abstract声明
>
> 抽象方法没有函数体，由继承的子类(派生类)实现
>
> 一个类包含抽象方法，这个类必须是抽象类
>
> 抽象类可以没有抽象方法

```
abstract class Father{
	abstract showMsg(name:string):string
}

class Son extends Father{
	showMsg(name:string){
		return `My name is ${name}`
	}
}
```

