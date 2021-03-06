# 一. nrm的安装和使用
## 1.nrm的作用
提供了一些最常用的npm包的镜像地址，能够让我们快速的切换安装包的时候的服务器的地址。

## 2.nrm的安装
使用npm i nrm -g 全局安装nrm包

## 3.nrm的使用
（1）使用nrm ls 查看当前所有可用的镜像源地址以及当前所使用的镜像源地址； 
（2）使用 nrm use npm 或者nrm use taobao切换不同的镜像源地址 
**温馨提示：** 
nrm 只是单纯的提供了几个常用的下载包的url地址，并能够让我们在这几个地址之间，很方便的进行切换，但是，我们每次安装包的时候，使用装包的工具都是npm


# 二.webpack
## 1.webpack的概念
webpack是前端的一个项目构建工具，基于node.js开发出来。

## 2.webpack的作用
网页静态资源过多产生的问题： 
（1）网页加载速度慢，因为要发起很多二次请求。 
（2）静态资源过多就要处理错综复杂的依赖关系

**使用webpack可以解决静态资源之间复杂的依赖关系。**

## 3.webpack安装的两种方式 
（1）运行npm i webpack -g全局安装webpack 
（2）在项目根目录中运行npm i webpack --save-dev安装到项目依赖中

## 4.webpack基本配置文件使用
在项目文件夹里面新建一个webpack的配置文件，命名为webpack.config.js。 
这个配置其实就是一个js文件，通过node中的模块操作，向外暴露了一个配置对象exports。 
在配置文件中，需要手动指定入口（要打包的文件）和出口（文件打包后的输出路径）。
```javascript
const path = require('path');
module.exports = {
  entry: path.join(__dirname, 'src/main.js'),             //入口，表示要使用webpack打包哪个文件
  output: {                                               //出口相关配置
    path: path.join(__dirname, 'dist'),                   //指定打包好的文件存在哪个路径下
    filename: 'bundle.js'                                 //指定输出文件的名称
  }
}
```
设置好了配置文件之后，在cmd中输入webpack即可运行该配置文件，把本地的js代码打包编译到内存中

## 5.webpack-dev-server的使用
使用webpack每次修改代码都需要重新手动打包编译，很麻烦，webpack-dev-server这个插件可以实现自动打包编译的功能。 
运行npm i webpack-dev-server -D把这个插件安装到项目本地开发依赖。
**注意：**
+ （1）由于我们是在项目本地安装的webpack-dev-server，所以无法把它当作脚本命令在cmd终端中直接运行（只有安装到全局-g的工具，才能在cmd正常执行）所以，
安装完毕后，需要在package.json配置文件中配置这条命令，配置方法如下：
在scripts属性中新增一条属性："dev": "webpack-dev-server"
+ （2）webpack-dev-server这个工具，如果想要正常运行，要求在本地项目中也必须安装webpack（npm i webpack -D即使全局已经安装了本地也要安装）3
+ （3）把页面中引入的bundle.js变成从根路径引入：
···javascript
<script src="/bundle.js"></script>
```
+ （4）webpack-dev-server帮我们打包生成的bundle.js文件，并没有存放到实际的物理磁盘上，而是直接托管到了电脑的内存中，所以我们在项目的根目录中是找不到
打包好的bundle.js的。
然后，我们在终端中直接输入npm run dev即可运行该插件，运行后，每次我们配置的需要打包编译的文件变化的时候这个插件都会为我们自动打包编译。

## 6.webpack-dev-server的常用命令
在package.json配置文件中的script属性配置："dev": "webpack-dev-server"的时候，可以在后面增加一些参数来改变一些设置：
```
--open                //打包完毕后自动打开浏览器
--port 3000           //修改打开浏览器的端口号为3000
--contentBase src     //修改打开浏览器默认打开的目录为src
--hot                 //启用热更新功能（每次打包只打包修改的数据）

"dev": "webpack-dev-server --open --port 3000 --contentBase src --hot "
```

## 7.webpack-plugin的使用
 （1）插件作用 
 根据本地路径下的index模板页面在内存中生成对应的页面，这样我们在浏览器中访问的页面就是内存中的html页面了
 
 （2）安装流程 
 ①在终端输入安装指令：npm i html-webpack-plugin -D 
 ②在webpack.config.js配置文件中，导入安装的插件：
 ```javascript
 const htmlWebpackPlugin = require('html-webpack-plugin');
 ```
 ③只要是插件，都一定要放到module.exports中的plugins属性中去：
 ```javascript
 module.exports = {
  entry: path.join(__dirname, 'src/main.js'),             //入口，表示要使用webpack打包哪个文件
  output: {                                               //出口相关配置
    path: path.join(__dirname, 'dist'),                   //指定打包好的文件存在哪个路径下
    filename: 'bundle.js'                                 //指定输出文件的名称
  }
  //plugins用于配置插件
  plugins: [
    //创建一个在内存中生成html页面的插件
    new htmlWebpackPlugin({
      template: path.join(__dirname, 'src/index.html'),    //指定模板页面的路径
      filename: 'index.html',                              //指定在内存中生成页面的名称
    }),
  ]
}
```
## 8.loader配置和使用
直接引入项目路径下的css文件会向服务器发起二次ajax请求，所以css样式文件最好也从内存中读取。 
由于webpack默认只能打包处理js类型的文件，而无法处理其他的非js类型的文件。这时候就需要再安装一些插件，这里需要安装loader。 
（1）loader的安装和配置 
①如果要打包处理css文件，需要安装npm i style-loader css-loader -D 
②在webpack.config.js这个配置文件中，新增一个配置节点对象，叫做module，这个对象中，有一个rules属性，是数组类型，里面存放了所有第三方文件的匹配和处理规则。
```javascript
 module.exports = {
  entry: path.join(__dirname, 'src/main.js'),             //入口，表示要使用webpack打包哪个文件
  output: {                                               //出口相关配置
    path: path.join(__dirname, 'dist'),                   //指定打包好的文件存在哪个路径下
    filename: 'bundle.js'                                 //指定输出文件的名称
  }
  //plugins用于配置插件
  plugins: [
    //创建一个在内存中生成html页面的插件
    new htmlWebpackPlugin({
      template: path.join(__dirname, 'src/index.html'),    //指定模板页面的路径
      filename: 'index.html',                              //指定在内存中生成页面的名称
    }),
  ],
  //module用于配置所有第三方模块加载器
  module: {
    rules: [
      { test: /\.css$/, use: ['style-loader', 'css-loader'] }   //配置处理.css文件的第三方loader规则
    ],
  }
}
```
（2）安装处理less文件的loader
```javascript
npm i less-loader -D
```

（3）url-loader  
默认情况下，webpack无法处理css文件中的地址（不管是图片还是字体库，只要是url地址，都处理不了），所以又需要导入url-loader插件了。 
安装：npm i url-loader file-loader -D
 

## 9.webpack处理第三方文件类型的过程 
（1）发现这个要处理的文件不是js文件，然后去配置文件中，查找有没有对应的第三方loader规则。 
（2）如果能找到对应的规则，就会调用对应的loader处理这种文件类型。 
（3）在调用loader的时候，是从后面往前调用的。 
（4）当最后的一个loader调用完毕，会把处理的结果，直接交给webpack进行打包合并，最终输出到bundle.js中去。


## 10.babel的使用
在webpack中，默认只能处理一部分ES6的新语法，一些更高级的ES6语法或者ES7语法webpack是处理不了的。这时候，就需要借助于第三方的loader，来帮助webpack
处理这些高级的语法，当第三方loader把高级语法转为低级的语法之后，会把结果交给webpack去打包到bundle.js中。
通过Babel，可以帮我们将高级的语法转换为低级的语法。 
（1）配置 
①在webpack中，可以运行如下两套命令，安装两套包，去安装Babel相关的loader功能： 
第一套包：npm i babel-core babel-loader babel-plugin-transform-runtime -D 
第二套包：npm i babel-preset-env babel-preset-stage-0 -D 

②打开webpack的配置文件webpack.config.js，在module节点下的rules数组中，添加一个新的匹配规则： 
```javascript
{ test:/\.js$/, use: 'babel-loader', exclude:/node_modules/ }            
//这个配置规则中的exclude:/node_modules/表示排除掉node_modules目录，因为如果不排除这个文件夹，则babel会把node_modules中的所有第三方js文件全都打包编译，这样很消耗cpu，打包速度很慢。并且这些js文件打包后项目无法正常运行。
```

③在项目的根目录中，新建一个叫做.babelrc的babel配置文件，这个配置文件为json格式。

④在.babelrc文件中写如下的配置：
```javascript
{
  "presets": ["env", "stage-0"],
  "plugins": ["transform-runtime"]
}
```

# 三.使用webpack导入vue
## 1.安装vue包
npm i vue -S

## 2.导入包
import Vue from 'Vue';      //这里导入的包是功能不完整的包，只提供了runtime-only的方式，并没有提供像网页中那样的使用方式。所以不能用这种方式导入。
正确的导入包的姿势：
import Vue form '../node_modules/vue/dist/vue.js';

## 3.创建组件
在webpack中，推荐额外创建一个.vue的文件来存放组件模板，所以需要安装能解析这种文件的loader：
npm i vue-loader vue-template-complier -D
 
 
 # 四.在webpack中使用路由
 ## 1.下载包
 ```javascript
 npm i vue-router -S
 ```
 
 ## 2.导入包
 ```javascript
 import vueRouter from 'vue-router';
 ```
 
 ## 3.手动安装
 ```javascript
 Vue.use(vueRouter);
 ```
 
 ## 4.创建router对象
 ```javascript
 var router = new vueRouter({
  routes: [
    { path: '/account', component: account }                 //component后面的account是我们定义的一个组件，使用前需要先用import导入
  ]
 });
 ```

