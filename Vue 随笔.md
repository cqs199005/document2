# Vue 随笔

### 1.路由路径传值

+ 路由路径定义

  ```javascript
  {path:'/api/:id',component:app}  //这里的:id用来匹配路径 "/api/14" 这里14这个id
  ```

+ 获取路由上的值的方法

  ```javascript
  //在路由页面上
  1.可以通过 {{$route.params.id}} 直接渲染
  2.可以在实例对象data上取值id:this.$route.params.id(这样可以方便数据调用)
  ```

+ 路由传参第二种方式

  ```javascript
  <router-link to="/login?id=10" >登录</router-link>
  
  //获取参数
  this.$route.query.id
  ```

  

### 2. 获取到图片HTML片段渲染问题

+ 当给<style scoped> 设置scoped属性时,有时会导致图片渲染时过大,这是把这个属性去掉,就可以设置图片大小

### 3.Vue动画过渡

+ 切换动画效果时,要实现平滑动画,可以添加如下样式

  ```
  .v-leave-to { position:absolute}
  ```

### 4.路由重定向

+ 给根目录'/'设置默认显示路由

  ```
  {path:"/",redirect:"/home"}  //设置默认显示home这个首页
  ```

### 5.router-link

+ router-link 默认渲染为span便签

+ 可以设置属性tag来修改渲染成的标签

  ```javascript
  <router-link to="ddd" tag="li"></router-link>   //这里设置默认渲染成li标签
  ```

### 6.Vue缩略图插件使用vue-preview

+ 安装插件

  ```
  npm i vue-preview -S
  ```

+ 引入模块并注册

  ```javascript
  //缩略图模块
  import VuePreview from 'vue-preview'
  Vue.use(VuePreview)
  ```

+ 在template放置显示标签

  ```
   <vue-preview :slides="slide1" @close="handleClose"></vue-preview>
  ```

+ 注意slide1数据的格式

  ```javascript
   slide1: [
            {   //一种缩略图的数据格式
              src: 'https://farm6.staticflickr.com/5591/15008867125_68a8ed88cc_b.jpg',
              msrc: 'https://farm6.staticflickr.com/5591/15008867125_68a8ed88cc_m.jpg',
              alt: 'picture1',
              title: 'Image Caption 1',
              w: 600,
              h: 400
            },
  ```

+ 在Vue 实例注册事件

  ```javascript
  methods: {
        handleClose () { //关闭时触发的事件
          console.log('close event')
        }
      }
  ```

+ 定义样式(要在全局样式下才会生效,所以style不要定义scoped属性)

  ```scss
  .my-gallery {
              display: flex;
              flex-wrap: wrap;
              figure {
                  margin: 10px;
                  box-shadow: 0 0 10px #999;
                  img {
                      width: 100px;
                      vertical-align: middle;
                  }
              }
          }
  ```

### 7.this.$route 和this.router的区别

+ this.$route 是路由参数对象,路由中所有的参数都属于它,如params,query

  ```
  this.$route.params.id  //可以获取url传参
  ```

+ this.$router是路由导航对象,可以实现路由的跳转,前进,后退

  ```javascript
  this.$router.push('/home/googs;ist')  //跳转到指定路径
  this.$router.push({ path: '/home/googs;ist' }) //跳转到指定路径
  this.$router.push({name:"goodsdesc",params:{id:id}});  //在配置路由规则时需设置name属性
  this.$router.go(1)  //在浏览记录前进一步
  this.$router.go(-1)  //在浏览记录后退一步
  ```


### 8.半场动画

+ 标签添加transition标签

  ```html
  <transition
          @before-enter="beforeEnter"
          @enter="enter"
          @after-enter="afterEnter"
          >
              <div class="ball" v-show="isshow"></div>
  </transition>
  ```

+ 添加动画函数

  ```javascript
  beforeEnter(el){
                  el.style.transform = 'translate(0,0)';//动画初始位置
              },
  enter(el,done){
                  el.offsetWidth;   //刷新每一帧,固定写法
                  el.style.transform = 'translate(87px,202px)';
                  el.style.transition = "all 1s cubic-bezier(0,-0.4,.92,.39)";
                  done() //调用结束动画
              },
  afterEnter(el){
                  this.isshow = !this.isshow
              },
  showball(){
                  this.isshow = !this.isshow
              }
  ```

+ 使用animate 动画

  - 先安装包 `yarn add animate.css -S`
  - 在`main.js`导入样式文件 `import 'animate.css/animate.css'`

  ```vue
  <transition appear enter-active-class="animated fadeInRight" >  //这是实现进场动画
        <router-view ></router-view>
  </transition>
  ```

### 9.Vuex

+ vuex 是 Vue 配套的 公共数据管理工具，它可以把一些共享的数据，保存到 vuex 中，方便 整个程序中的任何组件直接获取或修改我们的公共数据；

+ 安装包 

  ```
  运行 cnpm i vuex -S 
  ```

+ 导入包

  ```javascript
  import Vue from 'vue'
  import Vuex from 'vuex'
  ```

+ 注册包

  ```javascript
  Vue.use(Vuex)
  ```

+ 实例化

  ```javascript
  var store = new Vuex.Store({
    state: {
      // 大家可以把 state 想象成 组件中的 data ,专门用来存储数据的
      // 如果在 组件中，想要访问，store 中的数据，只能通过 this.$store.state.*** 来访问
      count: 0
    },
    mutations: {
      // 注意： 如果要操作 store 中的 state 值，只能通过 调用 mutations 提供的方法，才能操作对应的数据，不推荐直接操作 state 中的数据，因为 万一导致了数据的紊乱，不能快速定位到错误的原因，因为，每个组件都可能有操作数据的方法；
      increment(state) {
        state.count++
      },
      // 注意： 如果组件想要调用 mutations 中的方法，只能使用 this.$store.commit('方法名')
      // 这种 调用 mutations 方法的格式，和 this.$emit('父组件中方法名')
      subtract(state, obj) {
        // 注意： mutations 的 函数参数列表中，最多支持两个参数，其中，参数1： 是 state 状态； 参数2： 通过 commit 提交过来的参数；
        console.log(obj)
        state.count -= (obj.c + obj.d)
      }
    },
    getters: {
      // 注意：这里的 getters，只负责对外提供数据，不负责修改数据,如果想要修改state中的数据,请去找 mutations
      optCount: function (state) {
        return '当前最新的count值是：' + state.count
      }
      // 经过咱们回顾对比，发现 getters 中的方法， 和组件中的过滤器比较类似，因为 过滤器和 getters 都没有修改原数据， 都是把原数据做了一层包装，提供给了 调用者；
      // 其次， getters 也和 computed 比较像， 只要 state 中的数据发生变化了，那么，如果 getters 正好也引用了这个数据，那么 就会立即触发 getters 的重新求值；
    }
  })
  ```

+ 在main.js父组件Vm上挂载

  ```javascript
  store:store 
  // 5. 将 vuex 创建的 store 挂载到 VM 实例上， 只要挂载到了 vm 上，任何组件都能使用 store 来存取数据
  ```

+ 子组件取值

  ```javascript
  $store.state.count
  ```

+ 操作store里面的数据

  + 这个不推荐

  ```javascript
  add() {
        // 千万不要这么用，不符合 vuex 的设计理念
        this.$store.state.count++;
      },
  ```

  + 推荐这样使用,让store帮我们操作

  ```javascript
  this.$store.commit("increment");  
  ```

+ 调用getters里的方法获取数据

  ```javascript
  {{$store.getters.badgeCount}}  //badgeCount 为方法名
  ```

+ 总结

  +  state中的数据，不能直接修改，如果想要修改，必须通过 mutations
  +  如果组件想要直接 从 state 上获取数据： 需要 this.$store.state.xxx
  +  如果 组件，想要修改数据，必须使用 mutations 提供的方法，需要通过 this.$store.commit('方法的名称'， 唯一的一个参数)
  +  如果 store 中 state 上的数据， 在对外提供的时候，需要做一层包装，那么 ，推荐使用 getters, 如果需要使用 getters ,则用 this.$store.getters.xxx


### 10.解决改变数组是,页面无法及时渲染更新问题

+ 使用Vue.set()方法更改数组值

  ```
  Vue.set(vm.数组名,要更改的下标,值)
  ```

###11.MVC和MVVM区别

+ MVVM
  + MVVM是站在前端的角度去分层
  + M(model)后台操作数据
  + V(view) 视图层
  + VM(view model) 控制.管理view 和 model
+ MVC
  + 站在整个项目的角度去分层
  + M(model) 数据连接
  + V(view) 前端页面
  + C(controller) 业务逻辑(包含路由分发)

### 12.Vue常用指令

+ v-cloak 为防止Vue文件没加载完出现{{xx}}的数据,给这个属性设置隐藏样式,当vue文件加载完,v-cloak会自动被移除掉,所以数据也就显示出来了

+ v-text 不解析html元素

+ v-html 解析html

+ v-bind(简写:) 属性绑定(单项数据绑定)  model改变,view跟着改变

+ v-model 双向数据绑定

+ v-on(简写@) 
  + 事件修饰符:stop(阻止冒泡),prevent(阻止默认行为)
  + 键盘事件绑定:@keydown 修饰符:@keydown.enter (按下回车键)

+ v-for 遍历 
  + 数组 `v-for="(item,index) in arr" :key="index"`
  + 对象`v-for="(value,key,index) in arr" :key="index"`

+ filter过滤器 {{  item | 过滤器名(arg2)}}

  + 全局过滤器Vue.filter(过滤器名称,function(arg1,arg2){ })  arg1:|左边的参数 arg2:传入的参数

  + 局部过滤器,定义在vm实例中

    ```javascript
    new Vue({
        filters:{
            dateFmt(arg1,arg2){  //过滤器
                
            }
        }
    })
    ```

+ 自定义指令

  + 全局指令

    ```javascript
    Vue.directive("过滤器名",{
        bind(){
           //内存当中做好处理 
        },
        instered(){
            //页面已经存在DOM的时候使用
        }
    })
    
    //简写:默认绑定bind和updata
    Vue.directive("指令名",()=>{
        
    })
    ```

  + 私有过滤器

    ```javascript
    new Vue({
        directives:{
            指令名(el,binding){ //el绑定的dom元素  //binding.value是指令后面传入的值
            
        }
    })
    ```

+ 自定义按键修饰符

  + @keydown.enter  

  + 自定义按键修饰符 

    ```javascript
    Vue.config,keyCodes.f2=113
    ```

+ vue-resource 数据交互

  + this.$http.get( ).then( )
  + this.$http.post(url,{emulateJSON:true}).then( )
  + this.$http.jsonp( ).then( )

### 13.生命周期

+ beforeCreate：实例刚在内存中被创建出来，此时，还没有初始化好 data 和 methods 属性

- created：实例已经在内存中创建OK，此时 data 和 methods 已经创建OK，此时还没有开始 编译模板
- beforeMount：此时已经完成了模板的编译，但是还没有挂载到页面中
- mounted：此时，已经将编译好的模板，挂载到了页面指定的容器中显示

### 14.props以对象形式接收

```javascript
props:{
        msg:{
            type:String  //数据类型记得大写
        }

				}
```

### 15. 路由

+ 后端路由
  + 每个url都对应服务器上的一个资源,这种对应关系就是后端路由,通过请求路径去分发相应的资源
+ 前端路由
  + 通过url中的hash值,实现页面间的跳转

### 16. 父子组件传值

+ 父组件向子组件传值时,技能属性绑定命名不能大写

  ```
   <News :message="message"></News>
   
    props:["message"],  //接收
  ```

+ 子向父

  ```vue
  <News :message="message" @getSubMsg="getSubMsg"></News>
   methods:{
         getSubMsg(data){
             console.log(data);
             this.submsg=data
         }
     },
     
  //子组件
  this.$emit('getSubMsg',传递的数据)
  ```

### 16.1 父组件调用子组件方法

```
<food :food="selFood" ref="food"></food>  //这是子组件

 this.$refs.food.show();
 //通过$refs获取子组件对象,然后就可以调用这个对象里的方法了
```



### 17. `watch`,`computd`,`methods`

+ `watch`,用来监听虚拟的,如路由改变
+ `computed`属性的结果会被缓存，除非依赖的响应式属性变化才会重新计算。主要当作属性来使用,用于计算属性；
+ `methods`方法表示一个具体的操作，主要书写业务逻辑；
+ `watch`一个对象，键是需要观察的表达式，值是对应回调函数。主要用来监听某些特定数据的变化，从而进行某些具体的业务逻辑操作；可以看作是`computed`和`methods`的结合体；

### 18.Vue-cli 使用

+ npm install vue-cli -g  (只需要装一次)
+ vue -V 检查版本信息,有则安装成功
+ 创建项目 `vue init webpack 项目名 `
+ 初始化选项后3项可以选择no

### 19. render 渲染

+ 用render渲染,指定一个组件替换vm实例el所指向的容器#app

  ```javascript
  new Vue({
      el:"#app",
      render:function(c){
          return c(组件名)
      }
      //可以简写
      render:c => c(组件名)
  })
  
  
  ```

### 20.在webpack使用Vue

##### 1.安装Vue模板

+ 正常使用`import Vue from 'vue' ` 导入的包是(vue.runtime.common.js)阉割版的,不完整

+ 使用路径导入真正的vue完整版js

  ```javascript
  import Vue from '../node_modules/vue/dist/vue.js'
  ```

+ 或者通过修改vue模块中的package.json文件的main指向,修改指向vue.js

+ 或者在`webpack.config.js`中添加`resolve`属性：

  ```javascript
  resolve: {
      alias: {
        'vue$': 'vue/dist/vue.js'
      }
    }
  ```

##### 2 .less样式使用

+ 在.vue文件中,设置style样式

  ```
  <style lang="less" scoped>  </style>  //scoped 设置这样式为私有样式
  ```

+ 安装依赖的包

  ```
  yarn add less less-loader -D
  ```

  ```
  yarn add node-sass sass-loader -D
  ```

### 21. element-ui

+ Vue2.0 的pc端开发ui框架

  ```
  yarn add element-ui -S
  ```

+ ```
  //导入element-ui 包
  import ElementUI from 'element-ui';
  import 'element-ui/lib/theme-chalk/index.css';
  Vue.use(ElementUI);
  
  //导入自定义全局样式文件
  import './styles/index.scss'
  ```

### 22. 路由编程式导航

+ 要使用编程式导航,在定义路由规则时要定义name值,值必须唯一

  ```
  { path: '/login', name:"login" component: login }
  ```

+ 使用,先导入路由组件

  ```
  this.$router.push({name:"login"})
  ```


### 23.给ui框架的组件绑定事件

+ 要在事件后面添加.native事件修饰符

  ```
  //使用element-ui开发,给输入框注册回车事件
  <el-input placeholder="请输入内容" @keydown.native.enter="getuserlist">
           ...
  </el-input>
  ```

### 24.插槽作用域

1. 先在组件中使用slot标签占坑  

2. 给slot标签添加一个name="slot" 这个name是给渲染组件的时候使用的
3. 给slot 绑定传递过去的属性
4. 在渲染后的组件之间添加一个 template 这个template 需要指定2个属性

​       slot="slot"  slot-scope = "scope" 插槽作用域  scope 是随便取的   

   5. 在{{{}}} 使用数据 {{scope.传递过来的属性}}

      ```vue
      组件模板
      <template id="template">
      		<div>
      			<!-- 先占坑 name是给插槽使用  :title 是给插槽绑定一个属性-->
      			{{name}}
      			<slot name="slot" v-for="item in list" :title="item.title" :user="name"></slot>
      		</div>
      	</template>
      ```

      ``` vue
      <login :list="list" :name="name">
      			<!-- slot="slot"  slot-scope   vue2.5 以上代替以前的 scope-->
      			<template slot="slot" slot-scope="scope">
      				<p>{{scope.title}}--{{scope.user}}</p>
      			</template>
      </login>
      ```

      

### 25.常用UI框架

+ PC端

  + elementUI
  + zan UI

+ 移动端

  + Mint UI

  + Mui 

  + Vant 

    + 图片预览功能
    + 

    

  + VUX

### 26 . webpack 打包

+ 打包修改,在`config/index,js`,修改第53行

  ```javascript
   assetsPublicPath: '/',
   改成
    assetsPublicPath: './',
  ```

+ 运行npm run build 打包

### 27.axios使用

+ 安装

  ```
  yarn add axios -S
  ```

+ 导入

  ```
  import axios from 'axios'
  ```

+ 设置 基准路径

  ```javascript
  const url ="http://127.0.0.1:8888/api/private/v1/"
  axios.defaults.baseURL=url
  ```

+ 设置请求头

  ```javascript
  //设置路由拦截,添加请求头信息token
  axios.interceptors.request.use(function (config) {
      // 将token给到一个前后台约定好的key中，作为请求发送
      let token = localStorage.getItem('mytoken')
      if (token) {
        config.headers['Authorization'] = token
      }
      return config
    }, function (error) {
      // Do something with request error
      return Promise.reject(error)
    })
  ```


### 28.Vue项目搭建

+ 运行`vue init webpack 文件名`

+ 运行yarn 安装依赖包

+ 安装less语法

  ```
  yarn add less less-loader -D
  ```

+ 如果要使用动画,要引动画文件

  ```
  yarn add animate.css -S
  import 'animate.css/animate.css'
  ```

### 29. Vue拓展

+ 博客 张鑫旭
+ 搜索项目,在掘金上找vue-demo
+ 知乎:黄轶
+ 前端里网站

### 30.获取点击事件的元素

```
//绑定事件,添加$event
@click="showDom($event)"   

showDom(e){
    // e.target 是你当前点击的元素
    // e.currentTarget 是你绑定事件的元素
}
```

### 31.`Vue.nextTick(callback)`

为了在数据变化之后等待 Vue 完成更新 DOM ，可以在数据变化之后立即使用`Vue.nextTick(callback)` 。这样回调函数在 DOM 更新完成后就会调用。 

### 32.better-scroll 滚动插件

```
yarn add better-scroll
```

```vue
import betterScroll from 'better-scroll'

//定义初始化
initScroll() {
    const menuScroll = new betterScroll(this.$refs.menu,{})
    const foodsScroll = new betterScroll(this.$refs.foods,{probeType:3}) 
	//probeType:3 表示监听滚动,获取滚动出去的距离
}

//在渲染完成后调用初始化事件
mounted(){
	 this.initScroll();
}
```

+ 滚动事件

  ```
  //当foods滚动时,触发事件
  this.foodsScroll.on('scroll',(pos)=>{
      //获取Y方向滚动出去的距离
      this.scrollY = Math.abs(Math.round(pos.y)); 
  })
  ```

+ 相关属性配置

  ```
  this.orderScroll = new BScroll(this.$refs.wrapper, {
            probeType: 3,    
            click:true,  //点击事件有效(默认被插件禁止)
            pullUpLoad: {   // 配置上啦加载
              threshold: -80   //上啦80px的时候加载
            },
            mouseWheel: {    // pc端同样能滑动
              speed: 20,
              invert: false
            },
            useTransition:false,  // 防止iphone微信滑动卡顿
          });
          // 上拉加载数据
          this.orderScroll.on('pullingUp',()=>{
            this.scrollFinish = false;
            // 防止一次上拉触发两次事件,不要在ajax的请求数据完成事件中调用下面的finish方法,否则有可能一次上拉触发两次上拉事件
            this.orderScroll.finishPullUp();
            // 加载数据
            this.getIncomeDetail(this.nextPage);
          });
        }
  ```

+ 跳转到指定元素位置

  ```
   //使用better-scroll的方法进行滚到到指定元素位置
  this.foodsScroll.scrollToElement(el,300);  //el为指定的元素,300为动画时间
  ```

### 33. 给数据添加新的属性

vue中给某对象添加新的属性数据时,vue是不会对齐进行数据驱动视图改变的,要使用vue.set方法才会

```
在组件先导入Vue 对象
 Vue.set(this.food,'count',1)
 //设置this.food.count=1
```



# 饿了么商家App项目

## 1. 需求分析

## 2. 项目资源准备

+ 小图片/图标使用2-3倍图
+ 小图标不建议使用精灵图,容易失真,而且webpack会自动帮我们打包图片,编译成base64位图,也不用发起请求
+ 自己制作字体图标
  + 在icomoom网站上,上传设计师设计的svg图片,可以帮我们生成字体图标
+ 在static静态文件夹下新建`css`文件夹,引入`reset.css`文件

## 3. 组件设置



