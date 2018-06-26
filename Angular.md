# Angular

### 1.Angular 简介

+ 是一套用于构建用户界面的JavaScript框架,有Google开发维护,主要被用来开发单页面应用程序

+ 类似于Vue.js
  + MVVM(数据驱动视图思想)
  + 组件化
  + 模块化
  + 指令

### 3.安装环境

+ 安装node.js
+ 安装npm
+ 安装Python
+ 安装C++编译工具

### 4.创建组件

```
ng generate component foo   //foo为组件名,会帮我们创建4个文件
```

### 5.TypeScript 

+ 文件名,以.ts为后缀
+ 语法兼容ES6
+ 是一种强类型语言,一旦定义了就不能修改类型
+ 凡是可以写JS的地方,都可以写TypeScript
+ 一般用于大型团队开发

### 6.ngFor

```
<div *ngFor="let hero of heroes">{{hero.name}}</div>
```

+ 带索引的

```
<div *ngFor="let hero of heroes; let i=index">{{i + 1}} - {{hero.name}}</div>
```

### 7.ngIf

 ```
  <app-hero-detail *ngIf="isActive"></app-hero-detail>
 ```

 ```
   <ng-template [ngIf]="todos.length"> </ng-template>  //使用模板来控制多个便签的同时显示隐藏
 ```

### 8.注册事件(事件名)

+ 注册键盘回车键按下事件

  ```
   <input  (keyup.enter)="addTodos($event)"  />   
   //.enter是事件修饰符,表示回车键
   //$event是事件源参数,方法调用时,可以通过e.target.value获得输入的值
  ```

+ (事件名)="要执行的表达式" ,也可以调用方法,也可以赋值语句

### 9.数据双向绑定

+ 在app.module.ts引入模块

  ```
  import { FormsModule} from '@angular/forms'
  ```

+ 注册模块

  ```
    imports: [
      BrowserModule,
      FormsModule
    ],
  ```

+ 使用

  ```
  <input  [(ngModel)]="item.done">
  ```

### 10.类名绑定

```
[ngClass]="{completed:item.done}"   //跟Vue类似{类名:布尔值}
```

### 11.get响应式属性

+ get 属性必须要有返回值,每当方法内的数据改变时,就会动态响应

```
  get getTodoALL() {
    return this.todos.every(item=>item.done)
  }
  //getTodoALL() 属性名
```

+ 在标签内容区使用这个属性   {{属性名}}

### 12.set 属性

```
(change)="setTodoAll=$event.target.checked" >  
//复选框发生改变时,触发事件,set属性设置必须要有参数传入$event.target.checked
```

### 13.路由使用

+ 初始化路由组件

  ```
  ng generate module app-routing --flat --module=app
  ```

+ 引入路由模块

  ```javascript
  import { NgModule } from '@angular/core';
  
  //引入路由模块
  import { RouterModule, Routes } from '@angular/router';
  
  //引入组件
  import { SigninComponent } from './signin/signin.component';
  import { SignupComponent} from './signup/signup.component'
  const routes: Routes = [
    { path: 'signin', component: SigninComponent },
    { path: 'signup', component: SignupComponent }
  ];
  @NgModule({
    exports: [ RouterModule ],
    imports: [ RouterModule.forRoot(routes) ],
  })
  export class AppRoutingModule { }
  ```

+ 设置路由出口

  ```
  <router-outlet></router-outlet>
  ```

  ####路由实现跳转事件

  + 在需要跳转的模板页面

  ```
  //导入路由模块
  import { Router } from '@angular/router'
  ```

  + 实例化路由

    ```
     constructor(
        private http: HttpClient,
        private router:Router
      ) { }
    ```

  + 使用方法跳转

    ```
    this.router.navigate(['/']) //跳转到首页
    ```

  #### 路由守卫

  + 在应用的根目录下创建一个 `auth-guard.ts` 文件。 

    ```javascript
    import { Injectable }     from '@angular/core';
    import { CanActivate,Router }    from '@angular/router';
    
    @Injectable()
    export class AuthGuard implements CanActivate {
        constructor(
            private router:Router
        ){}
      canActivate() {
          const userToken = window.localStorage.getItem('user-token')
        if(!userToken) {
            //当本地存储没有token时,说明用户没有登录,跳转到登录页面
            this.router.navigate(['/signin'])
            return false;
        }else {
             return true;  //返回true,则继续导航
        }
      }
    }
    ```

  + 在打开 `app-routing.module.ts`，导入 `AuthGuard` 类，修改管理路由并通过 `CanActivate()` 守卫来引用 `AuthGuard`： 

    ```javascript
    //导入路由守卫
    import {AuthGuard} from '../../auth-guard.service'
    ```

    ```javascript
      {
        path: 'admin',
        component: AdminComponent,
        canActivate: [AuthGuard],
      }
    ```

    ```javascript
    @NgModule({
    	providers:[AuthGuard]
    })
    ```

    

###14.input文本框报错问题

+ 在使用数据双向绑定时,记得设置name属性

### 15.Angular提供了丰富的表单验证功能

### 16.Angular发送http请求

+ 打开根模块 `AppModule`

  ```javascript
  //导入http模块
  import { HttpClientModule } from '@angular/common/http'
  //挂载到import上
    imports: [
      HttpClientModule
    ],
  ```

+ 在要使用http的模板上

  ```javascript
  //导入http模块
  import { HttpClient } from '@angular/common/http'
  ```

+ 在export 暴露的类中声明实例化http

  ```javascript
  //声明私有的http,系统会自动帮我们实例化
    constructor(
      private http: HttpClient
    ) { }
  ```

+ 在方法中就可以直接使用了

  ```javascript
  this.http.post('http://localhost:3000/users',data).toPromise().then(res => {
        console.log(res) //成功回调函数
      }).catch(err=>{    //错误回调函数
     	  console.log(err)  
      })
  ```


### 17.Angular中a标签

+ 路由跳转

```
<a routerLink="signin.html">Already have an account? Login here.</a>
```

+ 禁止a链接的默认跳转事件

```javascript
<a href="signin.html" (click)="signOut($event)">退出</a>

//事件处理函数
  signOut(e) {
    e.preventDefault();
  }
```

### 18.设置请求头

+ 引入请求头模块

  ```
  import { HttpClient,HttpHeaders } from '@angular/common/http';
  ```

+ 在发起请求中的options属性里设置,例添加'X-Access-Token'这个请求头

  ```javascript
  this.http.get('http://localhost:3000/contacts',{
        headers: new HttpHeaders().set('X-Access-Token', token)
      })
  ```

### 19.设置拦截请求和响应

+ 在App根组件创建一个拦截文件 `global.interceptor.ts`

  ```javascript
  import { Injectable } from '@angular/core';
  import {
    HttpEvent, HttpInterceptor, HttpHandler, HttpRequest
  } from '@angular/common/http';
  
  import { Observable } from 'rxjs';
  
  /** Pass untouched request through to the next request handler. */
  @Injectable()
  export class NoopInterceptor implements HttpInterceptor {
  
    intercept(req: HttpRequest<any>, next: HttpHandler):
      Observable<HttpEvent<any>> {
          //设置拦截内容,修改请求头
          const token = window.localStorage.getItem('user-token');
          //添加请求头
          const restoken = req.clone({headers:req.headers.set('X-Access-Token', token)})
      return next.handle(restoken);
    }
  }
  ```

+ 在App.module.ts注册这个文件,并设置拦截生效

  + 引入文件

    ```javascript
    //导入设置响应拦截模块
    import { NoopInterceptor } from './global.interceptor';
    import { HTTP_INTERCEPTORS } from '@angular/common/http';
    ```

  + 设置参数

    ```javascript
    @NgModule({
         providers: [
        { provide: HTTP_INTERCEPTORS, useClass: NoopInterceptor, multi: true },
      ],
    ```

### 20.页面路由跳转传参

+ 在路由导航模板的路径上添加参数匹配

  ```
  {path:'edit/:id',component:ContactEditComponent},  //匹配id
  ```

+ 在页面路由传递参数设置

  ```
   <a [routerLink]="['/contact/edit',item.id]" >编辑</a>&nbsp;&nbsp;  //传递id
  ```

+ 获取参数

  ```javascript
  //引入获取参数的模块
  import { ActivatedRoute } from '@angular/router';
  //声明
  constructor(
      private route:ActivatedRoute,
    ) { }
  //使用
  const id = this.route.snapshot.params.id;
  ```

  