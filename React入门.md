# React 入门

##  1.设置props的默认值

```javascript
    //设置props默认值
    static defaultProps = { 
        count:0  
    }
```

设置完就算没有传值过来,也会显示默认值0

```javascript
 render(){
        return <div>
            <h3>当前的数量是:{this.props.count}</h3>
        </div>
    }
```

## 2.设置props的值的类型校验

+ 在react 15.+版本后,校验类型的功能被单独抽离出来成为一个包,所以需安装包

  ```
  cnpm i prop-types --save
  ```

+ 引包

  ```javascript
  //引入React类型校验包,从react15.5以后开始单独抽离出来的
  import ReactType from "prop-types"
  ```

+ 设置类型校验

  ```javascript
      //设置props的变量类型
      static propTypes = {
          count:ReactType.number
      }
  ```

+ 如果用户传递的值类型错误,还是显示,但是会在控制台报错提示

  ```javascript
  Warning: Failed prop type: Invalid prop `count` of type `string` supplied to `CountsItem`, expected `number`.
      in CountsItem
  ```


## 3.修改state私有数据

+ 使用setState方法

```javascript
 this.setState({
       count:this.state.count+1
            })
```

+ 设置完会有回调函数

  ```javascript
  this.setState({
         msg:"新数据"
  },function(){
      //这是设置完成后的回调函数
  })
  ```

## 4.注册事件

+ 给标签注册事件

  ```javascript
   <button id="add" onClick={this.addCount}>+1</button>
  //在render里,这里的this指向组件实例
  ```

+ 在组件内部定义方法

  ```javascript
   //注册点击事件,使用箭头函数,这里的this指向组件实例
      addCount=()=>{
          this.setState({
              count:this.state.count+1
          })
      }
  ```

## 5.生命周期函数

### 创建阶段的生命周期函数

**组件创建阶段**：组件创建阶段的生命周期函数，有一个显著的特点：创建阶段的生命周期函数，在组件的一辈子中，只执行一次；

+ componentWillMount: 组件将要被挂载，此时还没有开始渲染虚拟DOM(可以用来发送数据请求)
+ render：第一次开始渲染真正的虚拟DOM，当render执行完，内存中就有了完整的虚拟DOM了
+ componentDidMount: 组件完成了挂载，此时，组件已经显示到了页面上，当这个方法执行完，组件就进入都了 运行中 的状态

### 运行中的生命周期函数

**组件运行阶段**：也有一个显著的特点，根据组件的state和props的改变，有选择性的触发0次或多次；

+ 在这`nextProps`,`nextState`表示最新的数据,修改后的

+ componentWillReceiveProps:组件将要接收新属性，此时，只要这个方法被触发，就证明父组件为当前子组件传递了新的属性值；这里可以用来监听路由改变进而重新发起请求改变页面

+ shouldComponentUpdate 

  ```javascript
   //当数据发生改变触发,这函数要返回true或false,看是否要把改变的数据重新渲染页面
      shouldComponentUpdate(nextProps,nextState){
          console.log(nextProps);   //最新的props数据
          console.log(nextState.count)  //最新的state数据,count=4
          console.log(this.state.count); //上一次修改的数据,count=3
          return nextState.count % 2===0 ? true : false;
      }
  ```

+ componentWillUpdate() //组件将要更新,但是还没更新

+ `componentDidUpdate`组件已经完成更新,此时获取的数据都是最新的 

  + 有两个参数(prevProps,prevState),获取修改前的数据

+ 注意:在运行中的生命周期函数不能修改State或props中的值,不然会陷入死循环

## 6.快速获取组件里的DOM元素

+ 在标签内通过ref绑定

  ``` html
  <h3 ref="content">当前的数量是:{this.state.count}</h3>
  ```

+ 使用

  ```javascript
  console.log(this.refs.content.innerHTML);  //打印标签的内容
  ```


## 7. 事件源参数e

+ e.target 表示触发事件的DOM对象

```javascript
<input type="text"  onChange={this.txtchange}/>
    txtchange=(e)=>{
        this.setState({
            msg:e.target.value  
        })
    }
```

## 8.React中组件间传值

+ React中任何数据和方法在组件中传值,都可以通过this.props拿到

## 9.context组件间传值 

1. 定义父组件共享数据 

   ```javascript
      getChildContext(){
           return {
               color:this.state.color
           }
       }
   ```

2. 在要共享出去的组件设置共享出去的数据校验 

   ```javascript
   static childContextTypes = {
           color:ReactType.string  //校验共享出去的数据类型为字符型
       }
   ```

3. 接收数据的组件也要进行数据校验 

   ```javascript
    static contextTypes = {
           color:ReactType.string //校验接收的数据类型为字符型
       }
   ```

4. 使用

   ```javascript
    render(){
           return <div>
               <h6 style={{color:this.context.color}}>这是孙组件</h6>
           </div>
       }
   ```

5. 口诀

   ```
   记住一串单词组合getChildContextTypes
   
   前3个、后3个、后两个
   
   一个方法、两个静态属性
   ```

## 10.路由导航使用

1. 安装`react-router`包

   ```
   yarn add react-router-dom
   ```

2. 在页面导入包

   ```javascript
   import { HashRouter, Route, Link } from 'react-router-dom'
   ```

3. 定义hash,启动路由导航

   ```javascript
   // 启动路由导航,一个网站只要开启一次即可
   //<HashRouter> 标签下只能有一个根标签包裹
   render(){
       <HashRouter>
       	<div>
       		...
       	</div>
       </HashRouter>
   }
   ```

4. 定义路由导航标签,相当于vue中的 `<router-link>`

   ```javascript
    <Link to="/home">首页</Link>&nbsp;&nbsp;
   ```

5. 定义路由规则,默认路由匹配为模糊匹配

   ```javascript
   <Route path="/home" component={Home}></Route>
   //这个路由规则,标签所在的位置也是一个占位标签,相当于Vue中的<router-view>想要精确匹配可以给路由加上exact
   ```

6. 精确匹配,添加`exact`属性

   ```javascript
   <Route path="/movie" component={Movie} exact></Route>
   ```

## 11.路由参数匹配

+ 路由传参

```javascript
 <Link to="/movie/top/250">电影</Link>&nbsp;&nbsp;
```

+ 路由规则

```javascript
<Route path="/movie/:type/:id" component={Movie} exact></Route>
```

+ 接收参数

```javascript
 console.log(this.props.match.params);  //{type: "top", id: "250"}
```

## 12.Ant Design of React 组件库

### 1. 基本安装配置

1. 安装包

   ```
   yarn add antd
   ```

2. 按需导入要使用的组件

   ```javascript
   //导入日期组件
   import { DatePicker } from 'antd'
   ```

3. 记得设置`css`样式匹配,在scss启用模块化,所以自己定义样式时用scss文件来定义

   ```javascript
   { test: /\.css$/, use: ['style-loader', 'css-loader'] }, 
   { test: /\.scss$/, use: ['style-loader', 'css-loader?modules&localIdentName=[name]_[local]-[hash:5]', 'sass-loader'] },// 通过 为 css-loader 添加 modules 参数，启用 CSS 的模块化
   ```

4. 在render()进行模板渲染

   ```javascript
   renter(){
       return <div>
       	 选择日期:<DatePicker />
       </div>
   }
   ```

5. 优化,由于第3步导入的是全部样式,体积过大,可以安装插件来定义按需导入

   ```
   cnpm i babel-plugin-import -D
   ```

6. 在` .babelr`添加插件配置
   ```javascript
   {
     "plugins": [
       ["import", { "libraryName": "antd", "libraryDirectory": "es", "style": "css" }] 
     ]
   }
   ```
### 2. 样式设置

+ 想让`Content`内容充满中间空白区域,设置flex

  ```
  <Content style={{ backgroundColor:'#fff',flex:"1"}}>
  ```

### 3.加载动画使用

+ 导入ui组件,书写代码

+ 设置一个判断变量isLoad=true

  ```javascript
   render(){
          return  <div>
              {this.isloading()}
          </div>
      }
      isloading=()=>{
          if(this.state.isLoad) {
              return <Spin tip="Loading...">
              <Alert
                message="正在请求电影列表"
                description="精彩内容,马上呈现..."
                type="info"
              />
            </Spin>
          }else {
              return <h1>电影加载好了!</h1>
          }
      }
  ```

  

## 13.获取App根目录下的hash

+ 例如想获取http://localhost:3000/#/home这里的home

```javascript
window.location.hash.split('/')[1]
```

## 14. Fetch API数据请求

Fetch API提供了一个fetch()方法，它被定义在BOM的window对象中，你可以用它来发起对远程资源的请求。 该方法返回的是一个Promise对象，让你能够对请求的返回结果进行检索。 

+ fetch只支持跨域CORS 不支持JSONP跨越

+ fetch-jsonp跨域是可以支持jsonp跨域

  + 安装npm install fetch-jsonp 

  + 导入跨域请求包

    `import fetchJSONP from 'fetch-jsonp'`

  + 像fetch一样正常使用

    ```javascript
    fetchJSONP("https://api.douban.com/v2/movie/in_theaters")
            .then(res=>{
                return res.json()   //固定写法,调用这个json方法才能获取到数据,会返回promise对象
            }).then(res=>{
                console.log(res);
            })
    ```

## 15.路由编程式导航

+ 只有通过路由规则匹配到跳过过来的页面,this.props里面才会有history属性

```javascript
        //使用reacr-router-dom 实现编程式导航
        const url = `/movie/${this.state.movieType}/${page}`;  //导航的路由地址
        this.props.history.push(url);
```

+ 在子组件使用时,没有`this.props.history.push`这路由跳转的方法,所以可以让父组件传递过来

  ```javascript
  <MovieInfo {...item} key={i} history={this.props.history}></MovieInfo>
  ```

## 16. 路由的Switch跳转

+ `import { Switch } from "react-router-dom"` 加载模块

+ 在渲染中使用

  ```javascript
  <Switch>
      <Route exact path="/movie/detail/:id" component={MovieItemInfo}></Route>
      <Route exact path="/movie/:type/:page" component={MovieList}></Route>
  </Switch>
  ```

+ 在Switch里面的路由,从上到下匹配,只有匹配到一个,就不会再进行匹配了



# React-Native

## 1.  注意事项

+ RN中只能用`.js`作为组件,不能用`.jsx`
+ 不能使用网页中的所有标签
+ 文本内容不能单独写,需用Text标签包裹
+ 连接夜神模拟器:adb connect 127.0.0.1:62001(在模拟器安装的目录的bin目录下运行)
+ 服务端口设置: 192.168.12.21:8081

## 2.RN中的布局标签

##### 1. 需要标签,需按需引入

```javascript
//按需导入需要的布局组件
import { View,Text } from 'react-native'
```

##### 2.标签说明

+ View :相当于Div,不能注册事件

+ Text: 文本标签

+ Platform: 用来提供平台检测功能

+ StyleSheet:样式相关组件

  ```javascript
  const styles = StyleSheet.create({  //创建样式对象
    container: {
      flex: 1,
      justifyContent: 'center',  //文本类型的样式必须加引号
      alignItems: 'center',
      backgroundColor: '#F5FCFF',
      fontSize:20             //长度单位px可以省略,因为RN默认是以px作为单位
    },
  })  
  ```

+ TextInput: 文本框组件

+ Image: 图片组件(请求网络资源图片标签要设置宽和高)

+ Button:按钮,在RN中,Button里title和onPress属性为必须的

+ ActivityIndicator：加载的小圆圈效果

+ ScrollView：这是一个列表滚动的组件,用来包裹页面,使页面可以滚动(RN默认超出页面高度也不能滚动的)

+ ListView：也是一个列表滚动的组件，但是，这个组件已经过时了，官方推荐使用 FlatList 来代替它

+ TouchableHighlight :用来包裹View标签,注册事件

  ```
  <TouchableHighlight onPress={} >
  	<View><View>
  </TouchableHighlight >
  ```

+ FlatList:高性能列表渲染组件

  


##### 3. tabbar 组件

github连接:`https://github.com/happypancake/react-native-tab-navigator`

+ 安装:npm install react-native-tab-navigator --save 

+ 导入组件

  ```
  import TabNavigator from 'react-native-tab-navigator';
  ```

+ 结构使用

  ```html
  <TabNavigator>
    <TabNavigator.Item
      selected={this.state.selectedTab === 'home'}   {/* 切换页面 */}
      title="Home"   {/* 文本 */}
      renderIcon={() => <Image source={...} />}   {/* 默认图标 */}
      renderSelectedIcon={() => <Image source={...} />}   {/* 被选中时图标 */}
      badgeText="1"   {/* 徽标,如购物车数量 */}
      onPress={() => this.setState({ selectedTab: 'home' })}>   {/* 点击事件 */}
      {homeView}
    </TabNavigator.Item>
    <TabNavigator.Item
      selected={this.state.selectedTab === 'profile'}
      title="Profile"
      renderIcon={() => <Image source={...} />}
      renderSelectedIcon={() => <Image source={...} />}
      renderBadge={() => <CustomBadgeView />}
      onPress={() => this.setState({ selectedTab: 'profile' })}>
      {profileView}
    </TabNavigator.Item>
  </TabNavigator>
  ```
  ##### 4.图标安装

  + npm install react-native-vector-icons --save 

  + 修改`android/app/build.gradle `配置文件,添加如下内容

    ```javascript
    project.ext.vectoricons = [
        iconFontNames: [ 'MaterialIcons.ttf', 'EvilIcons.ttf','FontAwesome.ttf' ] 
    	//上面的数组内容为你想要使用的字体库,常用FontAwesome.ttf,可以去你下载的包里的fonts文件夹里看字体库名称
    ]
    
    apply from: "../../node_modules/react-native-vector-icons/fonts.gradle"
    ```

  + 手动将下载的字体库资源全部复制到`android/app/src/main/assets/fonts ` 文件夹名称要小写,没有assets文件夹就自己创建

  + 手动给`android/settings.gradle `文件添加内容

    ```
    include ':react-native-vector-icons'
    project(':react-native-vector-icons').projectDir = new File(rootProject.projectDir, '../node_modules/react-native-vector-icons/android')
    ```

  + 编辑`android/app/build.gradle ` 添加下面`+`这条数据

    ```
    dependencies {   //找到这个属性
      compile fileTree(dir: 'libs', include: ['*.jar'])
      compile "com.android.support:appcompat-v7:23.0.1"
      compile "com.facebook.react:react-native:+"  // From node_modules
    + compile project(':react-native-vector-icons')    
    }
    ```

  + 编辑`android/app/src/main/java/MainApplication.java `,添加下面带`+`的数据

    ```javascript
    package com.myapp;
    
    + import com.oblador.vectoricons.VectorIconsPackage;
    
    ....
    
      @Override
      protected List<ReactPackage> getPackages() {
        return Arrays.<ReactPackage>asList(
          new MainReactPackage()
    +   , new VectorIconsPackage()
        );
      }
    
    }
    ```

  + 到此修改配置完毕,需运行`react-native run-android`重新打包构建,最好把之前的apk卸载了重新装

  + 注意：使用图标，需要使用 `Android SDK Manager` 安装 `Android SDK Build-tools 26.0.1` 并接收其 license;在配置安装环境的根目录运行`SDK Manager.exe`文件,选择`Android SDK Build-tools 26.0.1`和`SDK Platform 26 `并安装,安装时记得点击Accept Licens

  + 导入图标组件

    ```javascript
    import Icon from 'react-native-vector-icons/FontAwesome';
    ```

  + 使用

    ```javascript
     <TabNavigator.Item
                selected={this.state.selectedTab === 'home'}
                title="Home"
    			//这是在tab标签里使用的方法
                renderIcon={() => <Icon name="home" size={25} color="gray" />}
                renderSelectedIcon={() => <Icon name="home" size={25} color="#0079ff" />}
                onPress={() => this.setState({ selectedTab: 'home' })}
                >
    ```

  + 图标名称在该文档顶部的图标库名点击进入查看库网站


    ##### 4.轮播组件

+ 安装

  ```
  npm i react-native-swiper --save
  ```

+ 导入

  ```
  import Swiper from 'react-native-swiper';
  ```

+ 导入样式,设置图片后可以不需要这样式

  ```
  const styles = StyleSheet.create({
      wrapper: {
      },
      slide1: {
          flex: 1,
          justifyContent: 'center',
          alignItems: 'center',
          backgroundColor: '#9DD6EB',
      },
      slide2: {
          flex: 1,
          justifyContent: 'center',
          alignItems: 'center',
          backgroundColor: '#97CAE5',
      },
      slide3: {
          flex: 1,
          justifyContent: 'center',
          alignItems: 'center',
          backgroundColor: '#92BBD9',
      },
      text: {
          color: '#fff',
          fontSize: 30,
          fontWeight: 'bold',
      }
  })
  ```

+ 标签渲染

  ```
  {/* 轮播图组件 */}
      <View style={{height:220}}>  //要手动设置高度才会显示
          <Swiper style={styles.wrapper} showsButtons={true} autoplay={true} loop={true}>
              <View style={styles.slide1}>
                  <Text style={styles.text}>Hello Swiper</Text>  //自己把这标签换成Image
              </View>
              <View style={styles.slide2}>
                  <Text style={styles.text}>Beautiful</Text>
              </View>
              <View style={styles.slide3}>
                  <Text style={styles.text}>And simple</Text>
              </View>
          </Swiper>
      </View>
  ```

##### 5.RN中请求数据没有跨域限制,可以直接使用fetch请求各种资源

##### 6. RN中View默认是flex布局,默认是纵轴方向对齐

##### 7. 路由导航

1. 运行`npm i react-native-router-flux --save`

2. 路由官网：`https://github.com/aksonov/react-native-router-flux`

3. 路由相关配置：`https://github.com/aksonov/react-native-router-flux/blob/master/docs/API.md`

4. 路由简单的DEMO：`https://github.com/aksonov/react-native-router-flux/blob/v3/docs/MINI_TUTORIAL.md`

5. 导入模块

   ```
   import { Router,Scene,Stack } from 'react-native-router-flux'
   ```

6. 使用

   ```
   return <Router sceneStyle={{backgroundColor:"#fff"}}>
       <Stack key="root">
           <Scene key="app" component={App} hideNavBar={true} />
       </Stack>
   </Router>
   ```

   + 第一个Scene为默认展示的首页
   + key表示路由的规则名称,将来可以用来进行编程式导航
   + stack用来路由分组
   + Router 相当于react 路由里的`HashRouter`,启用路由导航
   + hideNavBar属性表示隐藏头部导航栏

7. 路由跳转

   + 导入组件

     ```
     import { Actions } from 'react-native-router-flux'
     ```

   + 使用

     ```javascript
     //点击跳转到电影列表事件
         getMovielist=()=>{
             Actions.movielist()  //movielist为路由规则的key值,如果要传递参数,直接写在括号里,对象形式如({id:15})
         }
         
     //取值:this.props.id
     ```

##### 8. FlatList  列表渲染

```html
<FlatList
    data={this.state.movielist}   //渲染的数据
    keyExtractor={(item, i) => i.toString()}  //设置key关键字
    renderItem={({ item }) => this.showList(item)}  //渲染的每项内容
    ItemSeparatorComponent={this.showLine}  //每项之间的分割,showLine是一个方法,方法返回分割线
    onEndReachedThreshold={0.5}   //距离底部为当前可见页面的一半触发
    onEndReached={this.getNextPage}  //触发的时间
/>
```



## 3.报错处理

+ 在index引入加入如下代码

  ```
  import { YellowBox } from 'react-native';
  YellowBox.ignoreWarnings(['Warning: isMounted(...) is deprecated', 'Module RCTImageLoader']);
  ```

+ 参考错误处理文档

  ```
  https://www.jianshu.com/p/98c8f2a970eb
  ```


## 4.调用摄像头拍照

[react-native-image-picker的github官网](https://github.com/marcshilling/react-native-image-picker)
[react native 之 react-native-image-picke的详细使用图解](http://www.cnblogs.com/shaoting/p/6148085.html)

1. 运行`npm install react-native-image-picker@latest --save`安装到项目运行依赖，此时调试**可能会报错**，如果报错，需要使用下面的步骤解决：
   - 先删除`node_modules`文件夹
   - 运行`npm i`
   - 运行`npm start --reset-cache`
2. 运行`react-native link`自动注册相关的组件到原生配置中
3. 打开项目中的`android`->`app`->`src`->`main`->`AndroidManifest.xml`文件，在第8行添加如下配置：

```
<uses-permission android:name="android.permission.CAMERA" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
```

1. 打开项目中的`android`->`app`->`src`->`main`->`java`->`com`->`当前项目名称文件夹`->`MainActivity.java`文件，修改配置如下：

   ```
   package com.native_camera;
   import com.facebook.react.ReactActivity;
   
   // 1. 添加以下两行：
   import com.imagepicker.permissions.OnImagePickerPermissionsCallback; // <- add this import
   import com.facebook.react.modules.core.PermissionListener; // <- add this import
   
   public class MainActivity extends ReactActivity {
       // 2. 添加如下一行：
       private PermissionListener listener; // <- add this attribute
   
       /**
        * Returns the name of the main component registered from JavaScript.
        * This is used to schedule rendering of the component.
        */
       @Override
       protected String getMainComponentName() {
           return "native_camera";
       }
   }
   ```

2. 在项目中添加如下代码：

   ```
   // 第1步：
   import {View, Button, Image} from 'react-native'
   import ImagePicker from 'react-native-image-picker'
   var photoOptions = {
     //底部弹出框选项
     title: '请选择',
     cancelButtonTitle: '取消',
     takePhotoButtonTitle: '拍照',
     chooseFromLibraryButtonTitle: '选择相册',
     quality: 0.75,
     allowsEditing: true,
     noData: false,
     storageOptions: {
       skipBackup: true,
       path: 'images'
     }
   }
   
   // 第2步：
   constructor(props) {
   super(props);
       this.state = {
         imgURL: ''
       }
     }
   
   // 第3步：在render(){}渲染区域添加如下
   <Image source={{ uri: this.state.imgURL }} style={{ width: 200, height: 200 }}></Image>
   <Button title="拍照" onPress={this.cameraAction}></Button>
   
   // 第4步：
   cameraAction = () => {
   ImagePicker.showImagePicker(photoOptions, (response) => {
     console.log('response' + response);
     if (response.didCancel) {
       return
     }
     this.setState({
       imgURL: response.uri
     });
   })
   }
   ```

3. 安装(Android SDK构建工具25.0.2] 许可协议

4. **一定要退出之前调试的App**，并重新运行`react-native run-android`进行打包部署；这次打包期间会下载一些jar的包，需要耐心等待！

## 5.签名打包发布Release版本的apk安装包

+ 请参考以下两篇文章：
 - [ReactNative之Android打包APK方法（趟坑过程）](http://www.jianshu.com/p/1380d4c8b596)
 - [React Native发布APP之签名打包APK](http://blog.csdn.net/fengyuzhengfan/article/details/51958848)

## 6.如何发布一个apk
1. 先保证自己正确配置了所有的 RN 环境

2. 在 cmd 命令行中，运行这一句话`keytool -genkey -v -keystore my-release-key2.keystore -alias my-key-alias2 -keyalg RSA -keysize 2048 -validity 10000`

   ```
   这里如果提示keytool 不是内部命令或可以执行命令
   就以管理员身份运行这个命令窗口,在执行
   ```
 + 其中： `my-release-key.keystore` 表示你一会儿要生成的那个 签名文件的 名称【很重要，包找个小本本记下来】
 + `-alias` 后面的东西，也很重要，需要找个小本本记下来，这个名称可以根据自己的需求改动`my-key-alias`
 + 当运行找个命令的时候，需要输入一系列的参数，找个口令的密码，【一定要找个小本本记下来】
3. 当生成了签名之后，这个签名，默认保存到了自己的用户目录下`C:\Users\liulongbin\my-release-key2.keystore`
4. 将你的签名证书copy到 android/app目录下。
5. 编辑 `android` -> `gradle.properties`文件，在最后，添加如下代码：
```
MYAPP_RELEASE_STORE_FILE=生成的签名文件名,包括后缀
MYAPP_RELEASE_KEY_ALIAS=your keystore alias
MYAPP_RELEASE_STORE_PASSWORD=设置的密码
MYAPP_RELEASE_KEY_PASSWORD=默认跟上面密码一样
```
6. 编辑 android/app/build.gradle文件添加如下代码：
```
...
android {
    ...
    defaultConfig { ... }
    + signingConfigs {
    +    release {
    +        storeFile file(MYAPP_RELEASE_STORE_FILE)
    +        storePassword MYAPP_RELEASE_STORE_PASSWORD
    +        keyAlias MYAPP_RELEASE_KEY_ALIAS
    +        keyPassword MYAPP_RELEASE_KEY_PASSWORD
    +    }
    +}
    buildTypes {
        release {
            ...
    +        signingConfig signingConfigs.release
        }
    }
}
...
```
7. 进入项目根目录下的`android`文件夹，在当前目录打开终端，然后输入`./gradlew assembleRelease`开始发布APK的Release版；
8. 当发行完毕后，进入自己项目的`android\app\build\outputs\apk`目录中，找到`app-release.apk`，这就是我们发布完毕之后的完整安装包；就可以上传到各大应用商店供用户使用啦；

>注意：请记得妥善地保管好你的密钥库文件，不要上传到版本库或者其它的地方。

