# ECMAScript 6 的新特性

##变量定义

###es5采用var来定义变量

#### var这种方式存在的弊端：

~~~
var没有块级作用域，定义后在当前闭包中都可以访问，如果变量名重复，就会覆盖前面定义的变量，并且也有可能被他人修改。
~~~

#### 新的两种变量定义方式

#####let

- 块级作用域

- 在同一个块中不能重复声明

  ~~~
  for(let i =0;i<3;i++){
   setTimeOut(function(){
      console.log(i);//0,1,2 
   },1000)
  }
  //上述let定义的方式相当于使用了闭包，因为let i = 0 就相当于一个函数了，而定时器又是在这个函数内部，那么不就实现闭包了吗
  ~~~

#####const

- 声明同时必须赋值
- 一定声明不可改变
  - 对象可以修改
- 块级作用域

#####`let` vs `const`

使用[最小特权原则](https://en.wikipedia.org/wiki/Principle_of_least_privilege)，所有变量除了你计划去修改的都应该使用`const`。 基本原则就是如果一个变量不需要对它写入，那么其它使用这些代码的人也不能够写入它们，并且要思考为什么会需要对这些变量重新赋值。 使用 `const`也可以让我们更容易的推测数据的流动。



## 箭头（Arrow）

`=>` 是function的简写形式，支持`expression` 和 `statement `两种形式。同时一点很重要的是它拥有词法作用域的this值，帮你很好的解决this的指向问题，这是一个很酷的方式，可以帮你减少一些代码的编写，先来看看它的语法。

### 基本用法

ES6允许使用“箭头”（`=>`）定义函数。

```
var f = v => v;
```

上面的箭头函数等同于：

```
var f = function(v) {
  return v;
};
```

如果箭头函数不需要参数或需要多个参数，就使用一个圆括号代表参数部分。

```
var f = () => 5;
// 等同于
var f = function () { return 5 };

var sum = (num1, num2) => num1 + num2;
// 等同于
var sum = function(num1, num2) {
  return num1 + num2;
};
```

如果箭头函数的代码块部分多于一条语句，就要使用大括号将它们括起来，并且使用`return`语句返回。

```
var sum = (num1, num2) => { return num1 + num2; }
```

由于大括号被解释为代码块，所以如果箭头函数直接返回一个对象，必须在对象外面加上括号。

```
var getTempItem = id => ({ id: id, name: "Temp" });
```

箭头函数可以与变量解构结合使用。

```
const full = ({ first, last }) => first + ' ' + last;

// 等同于
function full(person) {
  return person.first + ' ' + person.last;
}
```

## 类（class）

​	ES6 引入了class（类），让javascript的面向对象编程变得更加容易清晰和容易理解。类只是基于原型的面向对象模式的语法糖。

​	在es5中没有构造函数与普通函数没有太大的区别，只用new来区分。在es6中用class表示一个真正的构造函数。 

### 构造函数的另一张书写方式

- ES6新增 的书写方式
  传统方式：

  ```
  function Person(name: string, age: number) {
      this.name = name
      this.age = age
  }
  Person.prototype.sayHello = function(){
      console.log(this.name,this.age)
  }
  let str = new Person('小艳艳',65)
  str.sayHello()
  ```

- 类写法

  ```
  class Person {
      name: string
      age: number
         就是类的构造函数，当new Person时候，就会自动
  	   调用constructor
      constructor(name: string, age: number) {
             这里实际上是在动态的为实例添加成员
  	       TypeScript要求类的成员应该先被定义出来并确定类型
          this.name = name
          this.age = age
      }
         实例方法
      sayHello():void {
          console.log(this.name,this.age)
      }
  }
  let str = new Person('王艳',65)
  str.sayHello()
  ```

### 类的继承

```
class Person {
    name: string
    age: number
    //就是类的构造函数，当new Person时候，就会自动
	//调用constructor
    constructor(name: string,age: number){
    	//这里实际上是在动态的为实例添加成员
	   //TypeScript要求类的成员应该先被定义出来并确定类型
        this.name = name
        this.age = age
    }
    //实例方法
    eat():void {
        console.log('米西米西')
    }
}
//继承
class Student extends Person {
    constructor(name: string,age: number) {
        //这是父类构造方法
        super(name,age)
    }
}
//调用
let p1 = new Student('伍云召',18)
p1.eat()
```



### 类的简写形式

```
class info {
	name: string;
	age: number;
	constructor(name: string,age: number){
	    this.name = name
	    this.age = age
	}
}
改为：（成员比较少情况下）
class info {
	constructor(public name: string,public age: number){

	}
}
new info('阿吉',18,'男')
```

## 类成员

### 实例化成员

- 实例成员，只能通过new出来的实例访问

### 静态成员

- 也叫类本身成员，只能通过类本身访问

  ```
  // 实例成员，只能通过new出来的实例访问
  // 静态成员，也叫类本身成员，只能通过类本身访问
  class Persson{
  	static type: string = 'asdf'
  	name: string = 'asd'
  }
  Persson.type //静态成员只能类本身访问
  
  // ---实例化成员只能 new 一个实例访问
  new Persson().name 
  Persson.name //不报错是因为构造函数本身具有这个默认属性
  ```



## Module

到目前为止,javascript (ES5及以前) 还不能支持原生的模块化，大多数的解决方案都是通过引用外部的库来实现模块化。在ES6中，模块将作为重要的组成部分被添加进来。模块的功能主要由 `export` 和 `import` 组成.每一个模块都有自己单独的作用域，模块之间的相互调用关系是通过 `export` 来规定模块对外暴露的接口，通过`import`来引用其它模块提供的接口。同时还为模块创造了命名空间，防止函数的命名冲突。

### export,import 命令

```
  //test.js
  export var name = 'Rainbow'
```

ES6将一个文件视为一个模块，上面的模块通过 `export` 向外输出了一个变量。一个模块也可以同时往外面输出多个变量。

```
 //test.js
 var name = 'Rainbow';
 var age = '24';
 export {name, age};
```

定义好模块的输出以后就可以在另外一个模块通过`import`引用。

```
  //index.js
 import {name, age} from './test.js'
```

### 整体输入，module指令

```
 //test.js
  
  export function getName() {
    return name;
  }
  export function getAge(){
   return age;
  } 
```

通过 `import * as` 就完成了模块整体的导入。

```
import * as test form './test.js';
```

通过指令 `module` 也可以达到整体的输入。

```
 module test from 'test.js';
 test.getName();
```

### export default

不用关系模块输出了什么，通过 `export default` 指令就能加载到默认模块，不需要通过 花括号来指定输出的模块,`一个模块只能使用 export default 一次`

```
  // default 导出
  export default function getAge() {} 
 
  // 或者写成
  function getAge() {}
  export default getAge;

  // 导入的时候不需要花括号
  import test from './test.js';
```

一条`import` 语句可以同时导入默认方法和其它变量.

```
import defaultMethod, { otherMethod } from 'xxx.js';
```


## 解构赋值

#### 数组按照顺序解构

```
// 数组,按照顺序解构
var arr = [2, 3];
var num1 = arr[0], num2 = arr[1], num3 = arr[2];
console.log([num1, num2, num3]);
```

#### 对象按照属性解构

```
// 对象，按照属性名解构
var userinfo = {
    name: 'zsfas',
    age: 18
};
// 在浏览器环境中，window有默认属性成员name
var nickname = userinfo.name, age = userinfo.age;
console.log(nickname);
```

注意：

```
// 函数中形参不指定类型会默认any类型
function fn(x, y) {
    console.log(x + y);
}
fn(1, 5);
```

#### 剩余参数解构

```
function sum(..args: number[]){
	const ret = 0;
	args.forEach(function(item){
		ret += item
	})
	return ret
}
sum(1,2,3,4)
会动态推算出来返回值数据类型，
但前提是参数类型明确情况下
建议写上返回值类型！
```



## 展开操作符

### 数组展开操作符

```
// 数组展开操作符
let arr1 = [1,2.5,6]
let arr2 = [6,9,8,4,...arr1]//数组合并
console.log(arr2)
let arr3 = [...arr1,...arr2]//数组合并
console.log(arr3)
```

### 对象展开操作符

```
// 对象展开操作符
// 对象的展开操作符，一般用于对象拷贝
var obj1 = {
    name: '小艳艳',
    age: 28
}
var obj2 = {
    ...obj1,
    sex: '女'
}
console.log(obj2)
```

