# ES6笔记

### 1.常量

- #### ES5中的声明方法

```javascript
Object.defineProperty(window,'pi2',{
    value:3.1415926,
    writable:false,  //设置只读,不可修改
});

console.log(window.pi2);
window.pi2 = 9999;
console.log(window.pi2);
```

- #### ES6中的写法

  ```javascript
      const pi3 = 3.1415926;
      console.log(pi3);
  //    pi3 = 4;   会报错
  //常量定义完后就没办法再重新赋值
  ```



### 2.作用域

- #### ES5中var 作用域是全局

```javascript
//ES5 var 声明的i是全局变量,当调用函数时,i循环完的值是3
var callback = [];
for(var i = 0; i <= 2; i++) {
    //闭包函数
    callback[i] = function () {
        return i * 2;
    }
}
console.log(callback[0]()); //6
console.log(callback[1]()); //6
console.log(callback[2]()); //6

ES5中想用块作用域,一般是是用自调用函数;(function fn() {})()
```

- #### ES6中let 是块作用域

- #### let声明的变量不会变量提升,且具有{}块作用域

```javascript
//ES6 let 块作用域,每循环一次,就生成一个块作用域,调用函数时闭包指向对应的块作用域
var callback2 = [];
for(let i = 0; i <= 2; i++) {
    //闭包函数
    callback2[i] = function () {
        return i ;
    }
}
console.log(callback2[0]()); //0
console.log(callback2[1]()); //1
console.log(callback2[2]());//2
```

```javascript
ES6中可以用 {
			
               这里面的一个单独的块作用域
            }
            但是var在{}中不受影响 
```



### 3.箭头函数

- #### ES3/ES5 中声明函数方法

  ```javascript
  //ES3,ES5函数声明
  function aaa(a,b) {
      return a +b;
  }
  ```

  this的指向

  ```javascript
  //function传统定义的函数，this指向随着调用环境的改变而改变,谁调用,this指向谁
  function aaa(){
      console.log(this)
  }
  var obj={
      aaa:aaa
  };
  aaa();//此时输出window对象
  obj.aaa();//此时输出obj对象
  ```

- #### ES6中声明函数

- #### 如何把 function 改成 箭头函数呢：  先把 function 删掉，然后，在 ()  和 { } 之间，添加一个 `=>` 就好了

- 箭头函数的特性： 箭头函数内部的 this, 永远和 箭头函数外部的 this 保持一致；

- 箭头函数，本质上就是一个匿名函数 

- 最标准的箭头函数格式是      ( 参数列表 ) => { 函数体 }

- 变体1： 如果 箭头函数左侧的 形参列表中，只有一个 形参，那么，( ) 可以省略   ( x ) => { console.log(x) }     可以改造成     x => { console.log(x) }

- 变体2：如果 箭头函数右侧的 函数体中，只有一行代码，那么， { } 可以省略    (x, y) => {console.log(x + y)}  可以改造成    (x, y) => console.log(x + y)

- 变体3：如果箭头函数 左侧 只有一个形参，右侧只有一行代码，那么， 左侧的 () 和 右侧的 {} 都可以省略  ( x ) => { console.log(x) }   可以改造成     x => console.log(x)

- 注意： 如果我们省略了 右侧的 { }， 那么，默认就会把 右侧函数体中的代码执行结果，返回出去     (x, y) => { return  x + y }   可以简写成    (x, y) => x + y
  ```javascript
  //ES6箭头函数,把function删掉,在参数()和{}之间加入=>    function(){}变成 ()=>{}
  var bbb=(a,b)=> { return a + b;}

  // 当函数体只有一句时,可以省略{}和renturn
  var ccc=(a,b)=> a + b;

  //当参数只有一个时,可以省略括号
      var ddd = v => v;
      
  ```

- this 的指向

```javascript
    //箭头函数中,this 的指向是定义时this的指向
    var aaa=()=>{
        console.log(this)
    };
    var obj={
        aaa:aaa
    }
    aaa();//此时指向window
    obj.aaa();//此时指向window
```

+ 注意事项:

1. 由于js的内存机制，function的级别最高，而用箭头函数定义函数的时候，需要var(let const定义的时候更不必说)关键词，而var所定义的变量不能得到变量提升，故箭头函数一定要定义于调用之前！
2. 箭头函数固然好用，但是不能用于构造函数，即不能被new一下；



### 3.设置函数参数默认值

- #### ES5

```javascript
function fn(a,b,c) {
    a = a || 1;
    b = b || 2;
    c = c || 3;
    console.log(a)
    console.log(b)
    console.log(c)
}
```

- #### ES6

```javascript
function fn2(a=11,b=22,c=33) {
    console.log(a)
    console.log(b)
    console.log(c)
}	
```



### 4.ES6扩展函数及数组合并

- #### ES5合并数组

```javascript
	//ES5中合并数组
    var arr1 = [1, '你好', '世界'];
    var arr2 = ['aa', 'hello'].concat(arr1);
    console.log(arr2);
```

- #### ES6扩展运算符

```javascript
//可变参数,当不确定传入参数有几个时,可以用...a(扩展运算符)代替形参
function fn(...a) {  //在函数定义时...a成为rest参数
    var sum = 0;
    a.forEach(key => {
        sum += key;
});
    return sum;
}
console.log(fn(1,2,3,4,5))
//也可以这样调用
let arr = [1,2,3,4,5];
fn(...arr)    //在函数调用时,...arr称为扩展运算符,...arr==1,2,3,4,5(这个运算符帮我们把数组展开)
```

```javascript
	//ES6合并数组
    var arr1 = [1, '你好', '世界'];
    var arr2 = ['aa', 'hello',...arr1];
    console.log(arr2);
```



### 5.代理对象,对数据隐私保护

- #### ES3中对数据的保护,通过闭包进行保护

  ```javascript
  //设置性别属性不能更改
  var Person = function () {
      var data = {
          name: 'zs',
          age: 18,
          sex: '男'
      };
      this.get = function (v) {
          return data[v];
      }
      this.set = function (k, v) {
          if (k != 'sex') {
              return data[k] = v;
          }
      }
  }
  var person = new Person();
  console.table({
      name:person.get('name'),
      age:person.get('age'),   //男
      sex:person.get('sex')
  });
  person.set('sex','女');
  person.set('name','ls');
  console.table({
      name:person.get('name'),
      age:person.get('age'),   //男
      sex:person.get('sex')
  });
  ```


- #### ES5中通过设置常量来保护数据

  ```javascript
  var Person = {
      name:'zs',
      age:18
  };
  Object.defineProperty(Person,'sex',{
      writable:false,
      value:'男'
  });
  console.table({
      name:Person.name,
      age:Person.age,
      sex:Person.sex
  });
  Person.name = 'ls';
  Person.sex = '女';
  console.table({
      name:Person.name,
      age:Person.age, 
      sex:Person.sex
  });
  ```


- #### ES6中通过设置代理对象,让代理对象来管理数据的访问与修改

  ```javascript
  let Person = {
      name:'zs',
      age:18,
      sex:'男'
  };
  //设置代理对象
  let person = new Proxy(Person,{
      get(target,key) {
          return target[key];
      },
      set(target,key,value) {
          if(key != 'sex') {
              target[key] = value;
          }
       }
  });
  console.table({
      name:person.name,  //zs
      age:person.age,
      sex:person.sex   //男
  });
  person.name = 'ls';
  person.sex = '女';
  console.table({
      name:person.name,  //ls
      age:person.age,
      sex:person.sex    //男
  });
  ```

### 6.字符串新方法

1.判断字符串中是否包含某个字符

```javascript
str1.includes(str2);//如果包含则返回true
```
2.字符串模板

```javascript
var str="";
//字符串模板用反引号包裹字符串,用${}里面可以写变量;
for(let i=0;i<arr.length;i++) {
    str +=`<tr>
        <td>${arr[i].name}</td>
        <td>${arr[i].age}</td>
        <td>${arr[i].sex}</td>
    </tr>`
}
```

3.查找判断字符串

- startsWith() 用来判断字符串，是否以指定的字符开头，如果是，返回值是 true，否则返回 false
- endsWith() 用来判断字符串，是否以指定的字符结尾；如果是，返回值是 true，否则返回 false

4.padStart和padEnd的用法

```javascript
const str = '我是';
//padStart表示在字符串前面填充,第一个参数是填充后的长度,第二参数是用什么填充
const str2 = str.padStart(4,'0');
//padEnd()表示在字符串后面
const str3 = str.padStart(4,'0').padEnd(7,'大坏蛋');
console.log(str2);//00我是
console.log(str3);//00我是大坏蛋
```
### 7.对象中定义属性和方法(对象解构赋值)

```javascript
const name = "孙悟空";
const age = 99999;
const sex = '妖';
const attack = ()=> console.log('我会72变');

//声明一个对象,有上面的属性和方法
const monkey = {
    name,
    age,
    sex,
    attack
}
console.log(monkey);
monkey.attack();

//方法简写
var o = {
    talk:function(){
        console.log('我是大王');
    }
}
//可以简写
function talk(){
    console.log('我是大王');
}
var o2={
    talk
}
o.talk();
o2.talk();
//---------------------------
var o4 = {
    sum:function(x,y){
        return x + y;
    }
}
//简写
var o3={
    sum(x,y) {
        return x+y;
    }
}
console.log(o4.sum(5,5));
console.log(o3.sum(5,5));
```
### 8.导入模块方式和暴露方法

1. ##### 导入

   - ES5方法 var a = require('xxx')
   - ES6方法 import a  from "xxx"

2. ##### 暴露

   - ES5方法 module.exports = xx  或 exports

   - ES6方法 export default { }或export  

   - ```javascript
     //export default只能暴露一个,但是接收时可以自定义名字接收import xx from "模块名"
     //使用export 可以暴露多个成员,
     export var name = "zs"
     export var age = 18
     //但是必须使用{}来接收,但是可以按需导入,必须严格按照名称来接收
     import {name,age} from "模块名"
     //如果真的要改变接收的变量名,可以使用as起别名
     import {nams as name2,age} from "模块名"这种方式来接收
     ```


### 9.Promise 处理回调函数地狱问题

+ promise 实例是异步操作,JS主程序不会执行,只是创建这个实例,里面的内容进行异步操作,主程序继续运行下面代码

```javascript
//封装函数
function getFileByPath(fPath) {
    //Promise 是一个构造函数,一旦被new创建新的实例,会立刻执行里面的方法
    //Promise 表示一个 异步操作；每当我们 new 一个 Promise 的实例，这个实例，就表示一个具体的异步操作；
    //在 Promise 上，有两个函数，分别叫做 resolve（成功之后的回调函数） 和 reject（失败之后的回调函数）  
    var promise = new Promise(function(resolve,reject){  
        fs.readFile(fPath,"utf-8",(err,result)=>{
           if(err) return reject(err)  //失败执行失败函数
           resolve(result)  //成功执行成功的回调函数
        })
    })
    return promise //把实例对象传出去
}
```

+ .then()的使用

```javascript
//调用是,用一个变量接收实例
var p = getFileByPath('./file/02.txt')
//new 出来的Promise实例上，调用.then() 方法【预先】为这个Promise异步操作指定成（resolve）和失败（reject）回调函数；
p.then(function(data){ //第一个是成功回调
    console.log("成功了"+"---" + data);
},function(err){  //第二个是失败回调
    console.log("失败了" + "----" + err);
})
```

+ 需求,顺序读取文件1,2,3,并打印出来

```javascript
//通过.then()返回一个新的实例,新的实例又可以使用.then()进行处理
getFileByPath("./file/01.txt").then(function(data){
    console.log(data);
    return getFileByPath("./file/02.txt")
}).then(function(data){
    console.log(data);
    return getFileByPath("./file/03.txt")
}).then(function(data){
    console.log(data);
})
```

+ .catch() 一旦有一个错误,立即终止实例下的代码执行

  ```javascript
  }).then(function(data){
      console.log(data);
  }).catch(function(err){
      console.log("一旦有一个错误,以下代码立即终止" + err.message);
  })
  ```

+ 

### 10.类

+ 用于创建对象,

  ```javascript
  class person {
  	//constructor就是类的构造函数,当new一个新对象时,就会调用这个,为对象添加新成员
      constructor(name, age) {
          this.name = name,
          this.age = age
      }
      ////这是指定义原型链上的方法
      sayHai() {
          console.log(this.age,this.name)
      }
  }
  var p1 = new person('张三', 18)
  console.log(p1);   //person { name: '张三', age: 18 }
  ```

+ 类的继承,继承父元素上的所有属性及方法

  ```javascript
  class student extends person {  //extends指继承类的名称,继承所有方法
      constructor(name, age) {
          super(name,age)  //super指调用Person对象上的属性,继承所有属性
      }
  }
  
  var s1 = new student('李四',16)
  console.log(s1); //student { name: '李四', age: 16 }
  ```

+ 类成员的公开/私有/受保护/只读

  + 默认类成员都是公开的`public`
  + 我们可以把不希望被外部修改的成员定义为私有的`private`,私有成员无法被继承
  + `protected`  和`private`类似,但是可以被继承,也无法被外部访问,内部自己可以访问
  + `readonly` 只读属性,数据可以被读取,但是不能重新赋值

+ 类的静态属性只能被类使用,new出来的无法调用

  ```javascript
  class Pre{
      static info = 'aaaa'
  }
  var p1 = new Pre
  console.log(p1.info);  //undefind
  console.log(Pre.info);  //aaaa
  ```

  

### 11.for-of数组循环

```javascript
let arr = [1,2,3,4,5,6];
for(let val of arr) {
	if(val === 3) {
      break;
	}
    console.log(val);
}
//1 2 
//val 代表数组的每一项,并且可以使用break打断
```

