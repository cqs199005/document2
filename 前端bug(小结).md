# 小bug处理方法

### 1.google浏览器下滑动事件无效解决方法

1、改body加上css样式：

```
body {
	touch-action:none;
	}
```



2.js加上如下代码：

    document.addEventListener(
      'touchmove',
      function (event) {
        event.preventDefault()
      },
      {passive: false}
    )

### 2.获取页面滚动出去的距离为0 

正常情况下 var scrollTOp = document.body.scrollTop  
解决办法: scrollTOp = document.documentElement.scrollTopd

原因:兼容性问题和HTML页面第一行已经声明了doucment

### 3.页面水平方向超出出现滚动条

添加CSS样式,水平超出隐藏

```html
body {
  overflow-x:hidden;
}
```

 ### 4.用盒子画半圆扇形

1. 半圈:只设上面两个点的圆角
2. 扇形:高度宽度等于零,只设一个点
3. 参考资料:网页jqhtml里有css图形制作

### 5.touchend获取对象问题

touchend事件里因为手指已经松开,所以用e.targetTouches获取不到对象,要用changedTouches来获取对象,记得对象是一个数组changedTouches[0];

### 6.行内块行间隔处理

+ 行内块受父元素字体大小影响,中间会有4px左右的距离

  ```
  //给父盒子添加样式
  body {
    font-size:0;
  }
  ```

### 7.移动端1px的边框线

+ 在vue /scss语法下写法:新建文件`mixin.scss`

  ```scss
  @mixin border-1px($color) {
      position: relative;
      &::after {
          content: '';
          display: block;
          position: absolute;
          left: 0;
          bottom: 0;
          width: 100%;
          border-top: 1px solid $color;
      }
  }
  ```

+ 在使用的组件页面

  ```
  @import './common/css/mixin.scss';
  ```

  ```scss
  //在使用者样式的便签内导入自定义的样式
   .naver {
      display: flex;
      height: 40px;
      line-height: 40px;
      @include border-1px(rgba(7,17,27,0.1));
  ```

+ 在全局样式配对媒体查询,设置缩放比例

  ``` scss
  @media (-webkit-min-device-pixel-ratio:1.5),(min-device-pixel-ratio:1.5) {
      .border-1px {   //这边的.border-1px 不确定要不要加点
          &::after {
              -webkit-transform: scaleY(0.7);
              transform: scaleY(0.7);
          }
      }
  }
  @media (-webkit-min-device-pixel-ratio:2),(min-device-pixel-ratio:2) {
      .border-1px {
          &::after {
              -webkit-transform: scaleY(0.5);
              transform: scaleY(0.5);
          }
      }
  }
  ```


### 8. 移动端2倍图3倍图

+ 在`mixin.scss` 文件定义样式函数

  ```scss
  /* 设置2倍图,3倍图媒体查询 */
  /* 默认使用2倍图,如果发现设备像素比是3,就使用3倍图 */
  @mixin bg-img($url) {
      background-image: url($url+"@2x.png");
      @media (-webkit-min-device-pixel-ratio:3),(min-device-pixel-ratio:3) {
          background-image: url($url+"@3x.png")
      }
  }
  ```

+ 在使用样式函数的地方导入引用

  ```scss
  /* 导入样式函数 */
  @import '../../common/css/mixin.scss';
   .header{
       .news {
           display: inline-block;
           width: 30px;
           height: 18px;
           @include bg-img('brand');  // brand为图片路径+名字
           background-size: cover;
           background-repeat: no-repeat;
       }
   }
  ```

# 小知识

### 1.jquery下获取宽度

1. width( )  当前元素的内容宽度
2. innerWidth( ) 当前元素的内容宽度 + padding
3. outerWidth( ) 当前元素的内容宽度 + padding + border
4. outerWidth(true) 当前元素的内容宽度 + padding + border + margin



### 2.伪元素添加小图标或字体图标

```html
 ::before {
            content: "\e915";
            font-family: 'wjs';
            position: absolute;
            top: -4px;
            left: 0;
            font-size: 25px;
          }
 <span>#$xe900</span> /*在标签内直接写*/
```

注:伪元素设置宽高百分比时,要求父元素要有具体的样式宽高值(被子元素撑开不算);

### 3.背景图定位

让背景只显示在盒子的中间一部分

```html
	background: url("../images/jd-sprites.png");
    background-origin: content-box;   /*背景中心在padding范围内*/
    background-clip: content-box;  /*超出内容裁剪*/
	background-size: 200px 200px;   /*两倍图缩放*/
    background-position: -21px 0;   /*精灵图位置*/
    padding: 15px;  /*限制盒子内容*/
```

### 4.less样式使用

- 引入less文件

  ```
  <link rel="stylesheet/less" href="css/fledgling.less"/>  
  注意:rel里要加"/less"
  ```


- 引入解析less文件的js文件

  ```
  <script src="js/less.js"></script>
  ```

### 5.flex布局及盒子居中

文本多列布局:

1. column-count: 3  设置列数
2. column-gap: 50px 设置间隔
3. column-rule: solide 设置间隔线
4. column-width: 200px 设置每列宽度 取最大值
5. column-span: all ; 设置占一整行
6. justify-content:主轴对齐方式
7. align-items:交叉轴对齐方式
8. 默认主轴方式是水平方向

伸缩盒子

1.盒子居中

```
display :flex;
justify-content:center;   /*水平居中*/
align-items:center    /*垂直居中*/
```

2.伸缩盒子定位

```
display:flex;
x居中:justify-content: center;
y居中:align-items:center;
```

3.使用行内块特性居中

```
1.父元素设置:	line-height:盒子的高度
				text-aglin:center
2.居中的盒子设置 display:inline-block; 
                vertical-algin:middle;
```

4. 定位实现居中

   ```
   div {
     position:absolute;
     right:0;
     left:0;
     top:0;
     bottom:0;
     margin:auto;
   }
   ```

5.让一行的两段文本左右两段对齐

```
//给父盒子添加如下样式
diaplay:flex;
justify-content:space-between;  //设置水平的对齐方式为两端对齐
```

6.改变布局方向

```
flex-direction: column;  //垂直布局
```

### 6.不用bootstrap的响应式网页

可以使用媒体查询判断屏幕大小,根据大小选择要引用的CSS文件

```
<link rel="stylesheet" media="screen and (min-width:992px) and (max-width:1200px)" href="b.css">
/*当屏幕大小在992-1200之间时,引用这个CSS样式*/
```
### 7.使用bootstrap布局问题

1. 号航或栅格没有靠最左边排列,可能是由于上面的元素超出了,因为是浮动元素,所以出现位置偏移;解决方案:可以把上面超出的1~2像素减掉或者使用clearboth清除浮动


### 7.ajax接收跨域json数据

在html用ajax访问本地php,php再去跨域访问接口得到json数据

```
PHP中访问一个url接口的方式：
<?php
	//在php中,获取一个链接中的数据
	//设置编码
	header("Content-Type: text/plain; charset=utf-8");
	//使用curl进行网络数据访问
	$ch = curl_init();
	//网络访问的url地址
	$url = "http://xxx";  
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1); 
	// 执行HTTP请求
	curl_setopt($ch , CURLOPT_URL , $url);
	//得到数据
	$res = curl_exec($ch);
	echo $res;
?>
```

### 8.运行npm环境

1. window键+R调出系统运行窗口
2. 输入cmd
3. 输入npm-v进入npm环境
4. 下载vue最新文件: npm install vue


### 9.数组的测试函数

+ `**findIndex()**`方法返回数组中满足提供的测试函数的第一个元素的**索引**。否则返回-1。

  ```
  list:[
                  {id:1,name:'奔驰',time:new Date},
                  {id:2,name:'宝马',time:new Date}
              ];
  var index = this.list.findIndex(function (item) {
                      return item.id == 2;
                  });
  //返回结果为index = 1;
  //item代表数组里的每一项,相当于执行callback函数,ruturn 后面表达式结果为true时,则返回对应的索引
  ```


### 10.让文本框只读(不显示光标)

```
<input type="text" readonly >  
```

### 11.获取表单提交数据,并且转为对象形式

```
$("form").serialize();   //得到的是对象形式,可以直接放在Ajax请求的data里,固定写法
```

### 12.获取URL地址栏传参的数据

```
 //从url中获取ID信息
        const search =new URLSearchParams(location.search);
        const id = search.get("id");
        console.log(location.search);  //?id=2
        console.log(id);  //2
```

### 13.表单注册提交事件

```
 $('#form').on('submit',function(e){    //点击submit按钮会触发这个事件
            e.preventDefault();   //取消表单的默认提交行为
            })
```

### 14.表单checkbox注册点击事件

+ 如果多个同时注册事件,最好用change 来注册,这样才能准备的监测到事件
+ 获取checkbox的选中状态用prop("checked")

### 15.replace替换使用

- 正常情况下只替换符合条件的第一个arr.replace('替换1','替换结果');
- replace查找内容可以写正则表达式,replace(/替换值/g,"替换结果") 
- 正则表达式的g表示全局,这样才能把所以的都替换掉

### 16.nrm的使用

+ 作用：提供了一些最常用的NPM包镜像地址，能够让我们快速的切换安装包时候的服务器地址；
+ 运行`npm i nrm -g`全局安装`nrm`包；
+ 使用`nrm ls`查看当前所有可用的镜像源地址以及当前所使用的镜像源地址；
+ 使用`nrm use npm`或`nrm use taobao`切换不同的镜像源地址；

### 17.在手机上模拟运行项目

+ 手机和电脑连接同一个wifi;

+ 按下window键,输入cmd,在输入ipconfig查看wifi的ip地址ipv4:...

+ 在package.json配置文件修改默认host地址添加--host 192.168.43.137

+ ```
  "dev": "webpack-dev-server --open --port 3000 --hot --host 192.168.43.137"
  ```

+ 当手机调试时,点击没效果的话,可以把点击事件换成tap

### 18.getBoundingClientRect()的用法

+ getBoundingClientRect用于获取某个元素相对于视窗的位置集合。集合中有top, right, bottom, left等属性。

### 19.开启Apache的gzip压缩

要让apache支持gzip功能，要用到deflate_Module和headers_Module。打开apache的配置文件httpd.conf，大约在105行左右，找到以下两行内容：（这两行不是连续在一起的）

```
#LoadModule deflate_module modules/mod_deflate.so
#LoadModule headers_module modules/mod_headers.so
```

然后将其前面的“#”注释删掉，表示开启gzip压缩功能。开启以后还需要进行相关配置。在httpd.conf文件的最后添加以下内容即可：

```
<IfModule deflate_module>
    #必须的，就像一个开关一样，告诉apache对传输到浏览器的内容进行压缩
    SetOutputFilter DEFLATE
    DeflateCompressionLevel 9
</IfModule>
```

最少需要加上以上内容，才可以生gzip功能生效。由于没有做其它的额外配置，所以其它相关的配置均使用Apache的默认设置。这里说一下参数“DeflateCompressionLevel”，它表示压缩级别，值从1到9，值越大表示压缩的越厉害。

### 20.url地址发生改变触发事件

```
//当地址栏上路径发生改变时触发
window.onhashchange=function(){
  
}
```

+ 获取hash值

  ```
   const params = location.hash.substr(1);
  ```

### 21.数组快速去重

+ `Set`对象是值的集合，你可以按照插入的顺序迭代它的元素。 Set中的元素只会出现一次，即 Set 中的元素是唯一的。 

  ```
  var arr2 = [1,2,5,3,5,1,5,23,1];
          var set1 = new Set(arr2)
          console.log(set1);   //{1, 2, 5, 3, 23}
          console.log([...set1]);  //[1, 2, 5, 3, 23]
  //Set是一个对象,对象的每个属性是唯一的,得到对象后用ES6对象展开获得值,再用数组包裹就成为数组了
  ```


###22. bind call apply的区别

+ 为前面的函数,修改内部的this指向

+ 区别

  + `bind` 只修改指向,不调用,`call`和`apply` 修改指向并且立即调用函数

  + bind 有返回值,返回前面函数修改this后的拷贝函数,原函数this不会没被修改

    ```
     //可以通过这用给下面定义的函数重新赋值
     this.handleMsg2 = this.handleMsg2.bind(this, '🚗', '🚚');
    ```


### 23.浏览器性能优化

+ 使用做动画时,使用transform比用定位top/left更节省浏览器性能

### 24.背景模糊

```
<div class="background">
    <img :src="seller.avatar" width="100%" height="100%" >
</div>

.background {
	filter: blur(10px); //使用过滤
}
```

### 25.有左右横线的居中标题

```html
   <div class="title">
                <div class="line"></div>
                <div class="text">优惠信息</div>
                <div class="line"></div>
   </div>
```

```scss
/* 有线标题 */
            .title {
                display: flex;
                width: 85%;
                margin: 30px auto 24px auto;
                .line {
                    flex: 1;
                    border-bottom: 1px solid rgba(255,255,255,0.2);
                    position: relative;
                    top: -6px;
                }
                .text {
                    padding: 0 12px;
                    font-size: 14px;
                    font-weight: 700;

                }
            }
```

### 26.容器内让多行文字垂直居中

可以使用display:table 表格样式

+ 给容器设置

  ```css
  display: table;
  ```

+ 给文本标签设置

  ```css
  display: table-cell;
  vertical-align: middle;
  ```


### 27 设置图片宽高相等

```html
 <div class="img-header">
    <img :src="food.image" alt="">
</div>
```

```css
.img-header {
    position: relative;
    width: 100%;
    height: 0;
    padding-top: 100%;  
    img {
        position: absolute;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
    }
}
```



# 插件使用

### 1.iscroll

1. 搭建结构,wrapper样式要定位

   ```
   <div id="wrapper">  
       <ul>
           <li>...</li>
           <li>...</li>
           ...
       </ul>
   </div>
   ```

2. 引用iscroll.js文件

3. ```
   <script type="text/javascript">  
   var myScroll = new IScroll('#wrapper');
   </script>               
   ```

4. 属性

   ```
   mouseWheel: true,  //是否可以使用滚轮
   scrollbars: true   //是否显示滚动条
   scrollX:true,  //水平滑动允许
   scrollY:false  //垂直禁止
   ```


### 2.文件上传插件

1.jquery-fileupload插件使用的步骤：
A.引入需要的文件：

```
<script src="assets/jquery-fileupload/jquery.ui.widget.js"></script>
<script src="assets/jquery-fileupload/jquery.iframe-transport.js"></script>
<script src="assets/jquery-fileupload/jquery.fileupload.js"></script>
```

B.找到文件上传表单元素,设置id,name属性：

```
<input type="file" id="fileUpload" name="file">
```

注意：name="file"不是每次都是写file，应该参考接口文档设置该属性的值

C.找到文件上传表单元素，设置data-url属性用来设置图片上传的接口路径：

```
<input type="file" id="fileUpload" name="file" data-url="/category/addSecondCategoryPic">
```

D.编写js代码，实现文件上传：
	$("#fileUpload").fileupload({
	dataType:"json",
	//文件上传之后的回调函数done
	done:function(e,data){
		//data参数中包含了上传图片的地址(即上传到服务器之后的图片地址)
		//预览上传的图片
		$("#preview").attr("src",data.result.picAddr);
	}
	})
# 网络知识

### 1.HTTP协议

- HTTP协议：HyperText Transfer Protocol 超文本传输协议。Web服务器上存储有HTML文件及图像资源等，在请求资源时，客户端（浏览器端）与WEB服务器端之间的交互协议。

- HTTP协议工作在TCP/IP协议之上（应用层），所有的web页面文件必须遵循的标准.

- HTTP协议保证计算机可以正确快速的传输文本文档数据，以及指定数据的正确显示格式等。

  ![](e:/笔记/报文详情.png)