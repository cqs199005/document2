# Webpack高级语法

## 1.配置基本打包编译热加载环境

1. 创建一个项目文件夹`webpack-senior`

2. 创建子文件夹`dist`/`src`

3. 在src里面创建`index.html`文件,和`main.js`文件

4. 在项目根目录运行`npm init -y`进行项目初始化

5. 根据需要安装`jquery`,为了提高装包速度,使用`yarn add jquery `装包

6. 开始进行`webpack`配置,在根目录创建`webpack.config.js`文件

7. 按照所需`webpack`打包文件,还有热加载包,使用`html-webpack-plugin`插件配置启动页面

   ```
   yarn add webpack@3.8.1 webpack-dev-server@2.9.4 html-webpack-plugin@2.30.1 --dev
   ```

8. 配置`webpack.config.js`的js入口,打包出口,模板渲染出口

   ```javascript
   const path = require('path')
   const htmlWebpackPlugin = require('html-webpack-plugin')  //引入插件
   
   module.exports ={
       entry:path.join(__dirname,"./src/main.js"),   //入口主JS文件
       output:{  //打包好出口文件
           path:path.join(__dirname,"./dist"),  //出口路径
           filename:"bundle.js"  //文件名
       },
       plugins:[  
           new htmlWebpackPlugin({  //插件.指定生成的渲染模板文件
               template:path.join(__dirname,"./src/index.html"),  //要渲染的模板位置
               filename:'index.html'           //渲染后的出口文件名
           })
       ]
   }
   ```

9. 在package.json配置热加载

   ```json
     "scripts": {
       "test": "echo \"Error: no test specified\" && exit 1",
       "dev":"webpack-dev-server --open --port 3000 --hot"   //添加这条指令,表示运行在本地3000端,并且自动打开浏览器运行,保存自动刷新
     },
   ```

10. 到此,运行`npm run dev`命令, 已经初步可以运行项目了

## 2.实现完整编辑环境配置

##### 1.配置解析css.scss文件的loader

+ 在`src`下创建`CSS`文件夹,并创建`scss`样式文件,写入样式

+ 在`main.js`文件导入样式文件,这时会报错,无法编译样式文件,需安装相应的loader

  ```javascript
  import './css/index.scss'
  ```

+ 安装相应的`loader`

  ```
  yarn add style-loader@0.19.0 css-loader@0.28.7 sass-loader@6.0.6 node-sass@4.6.0 --dev
  ```

+ 在`webpack.config.js`文件配置css,scss文件,注意：`use`表示使用哪些模块来处理`test`所匹配到的文件；`use`中相关`loader`模块的调用顺序是从后向前调用的

  ```javascript
  module.exports ={
  	module:{
          rules:[
              { test:/\.css$/, use:['style-loader','css-loader'] }, //解析css文件
              { test:/\.scss$/, use:['style-loader','css-loader','sass-loader'] }, //解析scss文件
          ]
     	}
  }
  ```

  

##### 2.配置路径识别loader,用于识别图片路径,文件路径

+ 在src下创建images文件夹,放入图片,并试着在样式中加入背景图片

+ 此时运行时会报错

+ 安装相应的路径解析loader

  ```
   yarn add url-loader@0.6.2 file-loader@1.1.5 --dev
  ```

+ 在`webpack.config.js`文件配置图片路径编译,可以通过`limit`指定进行base64编码的图片大小；只有小于指定字节（byte）的图片才会进行base64编码：

  ```javascript
  module.exports ={
  	module:{
          rules:[
  			{ test:/\.(png|jpg|gif|bmp)$/,use:'url-loader?limit=5000&' }, //图片路径解析,大于5K的图片不会被转成base64位图
          ]
     	}
  }
  ```

##### 3.解析ES6高级语法

+ 安装依赖包及语法

  ```
  yarn add babel-core@6.26.0 babel-loader@7.1.2 babel-plugin-transform-runtime@6.23.0 babel-preset-env@1.6.1  babel-preset-stage-0@6.24.1 --dev
  ```

+ 在`webpack.config.js`文件配置JS文件解析

  ```javascript
  module.exports ={
  	module:{
          rules:[
  			{ test:/\.js$/, use:'babel-loader', exclude:/node_modules/ }  //配置JS文件解析,exclude:/node_modules/表示排除项,node_modules里面的js文件不需要转换语法
          ]
     	}
  }
  ```

+ 在根目录创建`.babelrc`文件,并且配置ES6语法及插件

  ```javascript
  {
      "presets":["env","stage-0"],
      "plugins":["transform-runtime"]
  }
  ```

## 3.配置发布阶段的配置文件

1. 在根目录创建上线打包用的配置文件`webpack.pub.config.js `

2. 在package.json配置pub打包命令

   ```
    "pub":"webpack --config webpack.pub.config.js"
   ```

## 4.webpack打包优化

##### 1. 优化打包后图片存放路径,修改`webpack.pub.config.js`

```javascript
//在配置图片路径解析时,在图片名字前加上images/,这样最后打包的图片就全部放在images这个文件夹里了
{ test:/\.(png|jpg|gif|bmp)$/,use:'url-loader?limit=5000&name=images/[hash:8]-[name].[ext]' },
```

##### 2. 每次打包自动清除旧版本

- 安装清理插件

  ```
   yarn add clean-webpack-plugin --dev
  ```

- 在`webpack.pub.config.js`配置文件导入

  ```javascript
  //导入每次删除文件夹的插件
  const cleanWebpackPlugin = require('clean-webpack-plugin')
  ```

- 在module.exports里注册插件

  ```javascript
  plugins:[
      new cleanWebpackPlugin(['dist'])  //创建删除插件,[]里面为要删除文件的路径
  ]
  ```

##### 3. 优化bundle.js文件,抽离第三方包

+ 在`webpack.pub.config.js`导入`webpack`

  ```javascript
  const webpack = require('webpack')
  ```

+ 修改入口文件的配置

  ```javascript
  module.exports ={
      entry:{
          app:path.join(__dirname,"./src/main.js"),   //入口主JS文件
          vendors:['jquery']  //要抽离的第三方的包名放在这个数组里,数组名自定义
      },
  ```

+ 修改出口bundle.js文件的路径,让js文件统一放在一个js文件夹下

  ```javascript
  filename:"js/bundle.js"  //文件名
  ```

+ 注册webpack优化插件

  ```javascript
  plugins:[
  	 new webpack.optimize.CommonsChunkPlugin({
              name:'vendors',  //指定抽离的入口名称,对应上面的数组名
              filename:'js/vendors.js'  //把抽离出来的所有第三方包都放在这个js文件里
          })
  ]
  ```

##### 4. 优化压缩JS代码

+ 优化`webpack.pub.config.js`配置文件

```javascript
plugins:[
	 new webpack.optimize.CommonsChunkPlugin({
            name:'vendors',  //指定抽离的入口名称,对应上面的数组名
            filename:'js/vendors.js'  //把抽离出来的所有第三方包都放在这个js文件里
        }),
        new webpack.optimize.UglifyJsPlugin({
            compress:{  //配置压缩选项
                warnings:false  //移除警告
            }
        })
]
```

##### 5. 压缩html代码

```javascript
 plugins:[  
        new htmlWebpackPlugin({  //插件.指定生成的渲染模板文件
            template:path.join(__dirname,"./src/index.html"),  //要渲染的模板位置
            filename:'index.html',           //渲染后的出口文件名
            minify:{  //这段是压缩html优化的代码
                collapseWhitespace:true, //合并多余的空格
                removeComments:true,//移除注释
                removeAttributeQuotes:true //移除属性上的引号
            }
        })
```

##### 6. 抽离CSS,scss文件

+ 安装抽离样式插件

  ```
  yarn add extract-text-webpack-plugin@3.0.2 --dev
  ```

+ 在配置文件导入插件

  ```javascript
  //导入抽离CSS插件
  const ExtractTextPlugin = require("extract-text-webpack-plugin");
  ```

+ 修改之前配置的匹配css,scss文件

  ```javascript
      module:{
          rules:[
              { test:/\.css$/, use: ExtractTextPlugin.extract({  //抽离css文件
                  fallback: "style-loader",
                  use: "css-loader",
                  publicPath: '../' // 设置图片路径
                }) }, //解析css文件
              { test:/\.scss$/, use: ExtractTextPlugin.extract({
                  fallback: 'style-loader',
                  use: ['css-loader', 'sass-loader'],
                  publicPath: '../' // 设置图片路径
                }) }, //解析scss文件
  ```

+ 定义抽离的样式文件路径

  ```javascript
  plugins:[
      new ExtractTextPlugin("css/styles.css")  //抽离的样式文件存放路径
  ]
  ```

##### 7. 压缩CSS文件

+ 安装压缩CSS插件

  ```
  yarn add optimize-css-assets-webpack-plugin@3.2.0 --dev
  ```

+ 导入插件

  ```javascript
  const OptimizeCssAssetsPlugin = require('optimize-css-assets-webpack-plugin');
  ```

+ 启用插件

  ```javascript
  plugins:[
  	new OptimizeCssAssetsPlugin() //启用压缩css插件
  ]
  ```

  

