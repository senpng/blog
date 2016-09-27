title: Webpack
date: 2016-05-26 09:51:00
tags: 
---

## WebPack是什么

> 1. 一个打包工具
2. 一个模块加载工具
3. 各种资源都可以当成模块来处理
4. 网站 http://webpack.github.io/

如今，越来越多的JavaScript代码被使用在页面上，我们添加很多的内容在浏览器里。如何去很好的组织这些代码，成为了一个必须要解决的难题。

对于模块的组织，通常有如下几种方法：

1. 通过书写在不同文件中，使用script标签进行加载
2. CommonJS进行加载（NodeJS就使用这种方式）
3. AMD进行加载（require.js使用这种方式）
4. ES6模块

**思考：为什么只有JS需要被模块化管理，前台的很多预编译内容，不需要管理吗？**

基于以上的思考，WebPack项目有如下几个目标：

* 将依赖树拆分，保证按需加载
* 保证初始加载的速度
* 所有静态资源可以被模块化
* 可以整合第三方的库和模块
* 可以构造大系统

从下图可以比较清晰的看出WebPack的功能

![示意图](/images/webpack/webpack.png)

### 特点
1. 丰富的插件，方便进行开发工作
2. 大量的加载器，包括加载各种静态资源
3. 代码分割，提供按需加载的能力
4. 发布工具

### 优势
>* 丰富的插件，方便进行开发工作
* 大量的加载器，包括加载各种静态资源
* 代码分割，提供按需加载的能力
* 发布工具

## 安装
1.安装

```bash
$ npm install webpack -g
```

2.使用webpack

```bash
$ npm init # 自动生成一个package.json文件
$ npm install webpack --save-dev # 将webpack增加到package.json文件中
```

3.可以使用不同的版本

```bash
$ npm install webpack@1.2.x --save-dev
```

4.如果想要安装开发工具

```bash
$ npm install webpack-dev-server --save-dev
```

## 配置

> 每个项目下都必须配置有一个 webpack.config.js ，它的作用如同常规的 gulpfile.js/Gruntfile.js ，就是一个配置项，告诉 webpack 它需要做什么。

下面是一个例子

```javascript
var webpack = require('webpack');
var commonsPlugin = new webpack.optimize.CommonsChunkPlugin('common.js');
module.exports = {
    //插件项
    plugins: [commonsPlugin],
    //页面入口文件配置
    entry: {
        index : './src/js/page/index.js'
    },
    //入口文件输出配置
    output: {
        path: 'dist/js/page',
        filename: '[name].js'
    },
    module: {
        //加载器配置
        loaders: [
            { test: /\.css$/, loader: 'style-loader!css-loader' },
            { test: /\.js$/, loader: 'jsx-loader?harmony' },
            { test: /\.scss$/, loader: 'style!css!sass?sourceMap'},
            { test: /\.(png|jpg)$/, loader: 'url-loader?limit=8192'}
        ]
    },
    //其它解决方案配置
    resolve: {
        root: 'E:/github/flux-example/src', //绝对路径
        extensions: ['', '.js', '.json', '.scss'],
        alias: {
            AppStore : 'js/stores/AppStores.js',
            ActionType : 'js/actions/ActionType.js',
            AppAction : 'js/actions/AppAction.js'
        }
    }
};
```

1. plugins 是插件项，这里我们使用了一个 CommonsChunkPlugin的插件，它用于提取多个入口文件的公共脚本部分，然后生成一个 common.js 来方便多页面之间进行复用。
2. entry 是页面入口文件配置，output 是对应输出项配置 （即入口文件最终要生成什么名字的文件、存放到哪里）
3. module.loaders 是最关键的一块配置。它告知 webpack 每一种文件都需要使用什么加载器来处理。 所有加载器需要使用npm来加载
4. 最后是 resolve 配置，配置查找模块的路径和扩展名和别名（方便书写）

## 使用
1. 正确安装了WebPack，方法可以参考上面
2. 书写app.js文件

	```javascript 
	document.write("看看如何让它工作！");
	```
3. 书写index.html文件

	```html
	<html>
	<head>
	<meta charset="utf-8">
	</head>
	<body>
	<script type="text/javascript" src="bundle.js" charset="utf-8"></script>
	</body>
	</html>
	```
	
4. 执行命令，生成bundle.js文件
	
	```bash
	$ webpack ./entry.js bundle.js
	```

5. 在浏览器中打开index.html文件，可以正常显示出预期

**使用加载器**

1. 增加style.css文件

	```css
	body {
		background: yellow;
	}
	```
2. 修改app.js文件
	
	```javascript
	require("!style!css!./style.css");
	document.write("看看如何让它工作！");
	```
	
3. 执行命令，安装加载器

	```javascript
	$ npm install css-loader style-loader   # 安装的时候不使用 -g
	```
	
4. 执行webpack命令，运行查看效果
5. 可以在命令行中使用loader

	```javascript
	$ webpack ./entry.js bundle.js --module-bind "css=style!css"
	```
 
 **使用配置文件**
 > 默认的配置文件为webpack.config.js

增加webpack.config.js文件

```javascript
module.exports = {
 entry: "./entry.js",
 output: {
     path: __dirname,
     filename: "bundle.js"
 },
 module: {
     loaders: [
         { test: /\.css$/, loader: "style!css" }
     ]
 }
};
```
执行程序

```bash
$ webpack
```

## Webpack-dev-server
> webpack-dev-server是一个小型的node.js Express服务器,它使用webpack-dev-middleware中间件来为通过webpack打包生成的资源文件提供Web服务。它还有一个通过Socket.IO连接着webpack-dev-server服务器的小型运行时程序。webpack-dev-server发送关于编译状态的消息到客户端，客户端根据消息作出响应。

简单来说，webpack-dev-server就是一个小型的静态文件服务器。使用它，可以为webpack打包生成的资源文件提供Web服务。那么，它能给开发带来什么便利呢？

**第一** webpack-dev-server有两种模式支持自动刷新——iframe模式和inline模式。在iframe模式下：页面是嵌套在一个iframe下的，在代码发生改动的时候，这个iframe会重新加载；在inline模式下：一个小型的webpack-dev-server客户端会作为入口文件打包，这个客户端会在后端代码改变的时候刷新页面。

* 使用iframe模式无需额外的配置，只需在浏览器输入
http://localhost:8080/webpack-dev-server/index.html
* 使用inline模式有两种方式：命令行方式和Node.js API。
	
	1) 命令行方式比较简单，只需加入--line选项即可。例如：

	```bash
	webpack-dev-server --inline
	```
	使用--inline选项会自动把webpack-dev-server客户端加到webpack的入口文件配置中。
	
	**注意：** 使用webpack-dev-server命令行的时候，会自动查找名为webpack.config.js的配置文件。如果你的配置文件名称不是webpack.config.js，需要在命令行中指明配置文件。例如，我的配置文件是webpack.config.dev.js：`webpack-dev-server --inline --config webpack.config.dev.js`。
	
	2) Node.js API方式需要手动把`webpack-dev-server/client?http://localhost:8080`加到配置文件的入口文件配置处，因为webpack-dev-server没有`inline:true`这个配置项。
	
**第二** webpac-dev-server支持Hot Module Replacement，即模块热替换，在前端代码变动的时候无需整个刷新页面，只把变化的部分替换掉。使用HMR功能也有两种方式：命令行方式和Node.js API。

* 命令行方式同样比较简单，只需加入--line --hot选项。--hot这个选项干了一件事情，它把`webpack/hot/dev-server`入口点加入到了webpack配置文件中。这时访问浏览器，你会看见控制台的log信息：

	```
	[HMR] Waiting for update signal from WDS...
	[WDS] Hot Module Replacement enabled.
	```
	HMR前缀的信息由`webpack/hot/dev-server`模块产生，WDS前缀的信息由`webpack-dev-server`客户端产生。

* Node.js API方式需要做三个配置：
	1. 把`webpack/hot/dev-server`加入到webpack配置文件的entry项；
	2. 把`new webpack.HotModuleReplacementPlugin()`加入到webpack配置文件的plugins项；
	3. 把`hot:true`加入到webpack-dev-server的配置项里面。

**注意：**要使HMR功能生效，还需要做一件事情，就是要在应用热替换的模块或者根模块里面加入允许热替换的代码。否则，热替换不会生效，还是会重刷整个页面。下面是摘自webpack在github上docs的原话：

> A module can only be updated if you "accept" it. So you need to module.hot.accept the module in the parents or the parents of the parents... I. e. a Router is a good place or a subview.

具体的代码是：

```javascript
if(module.hot)
    module.hot.accept();

```

也可以使用一些插件去完成这个工作，例如`webpack-module-hot-accept`插件。不过，webpack-dev-server HMR结合`react-hot-loader`使用的时候，`react-hot-loader`会去做这个工作。

综合上述两点，使用`wepack-dev-server`辅助开发，使得开发者在开发前端代码的过程中无需频繁手动刷新页面，使用HMR甚至不用等待页面刷新，确实可以给开发者带来很好的体验。

但是，问题又来了。我要进行前后端联调的时候怎么办呢？毕竟`webpack-dev-server`只是一个静态文件服务器，不具备动态处理的能力。这个时候就需要将后端服务器与`webpack-dev-server`结合使用了。`webpack-dev-server`只用来为webpack打包生成的资源文件提供服务，比如js文件、图片文件、css文件等；后端服务器除提供API接口外，还提供入口HTML。

要将`webpack-dev-server`与后端服务器结合使用，需要做三件事情。

**第一** 首页HTML文件是从后端服务器发出的，这时页面的根地址变成了后端服务器地址，怎么使得webpack产生的资源文件在请求资源的时候是向`web-dev-server`请求而不是后端服务器请求？只需在webpack配置文件中的`output.publicPath`配置项写上绝对URL地址，例如`output.publicPath = "http://localhost:8080/assets/"`。这时，webpack打包产生的资源文件里面的url地址都会是绝对地址，而不是相对地址。

**第二** 后端服务器产生的入口HTML文件要向`webpack-dev-server`请求资源文件，这个简单，只需在HTML文件中加入资源文件的绝对地址，例如：`<script src="http://localhost:8080/assets/bundle.js">`
第三 要使`webpack-dev-server`和它的运行时程序连接起来。这个简单，只需要使用iline模式即可。

OK，things done!

最后再插一句，我在将`HMR`和`react-hot-loader`结合使用的时候遇到了热替换不起作用的问题，即改变前端代码之后页面不是热替换而是冲刷。我在这里找到了答案。
react-hot-loader/docs/troubleshooting.md

![](/images/webpack/troubleshooting.png)



