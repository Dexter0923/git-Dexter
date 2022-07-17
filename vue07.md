## 父子组件的访问方式： $children

有时候我们需要父组件直接访问子组件，子组件直接访问父组件，或者是子组件访问跟组件。

父组件访问子组件：使用$children或$refs

子组件访问父组件：使用$parent

我们先来看下$children的访问

this.$children是一个数组类型，它包含所有子组件对象。

我们这里通过一个遍历，取出所有子组件的message状态。

## 父子组件的访问方式： $refs

$children的缺陷：

通过$children访问子组件时，是一个数组类型，访问其中的子组件必须通过索引值。

但是当子组件过多，我们需要拿到其中一个时，往往不能确定它的索引值，甚至还可能会发生变化。有时候，我们想明确获取其中一个特定的组件，这个时候就可以使用$refs

$refs的使用：

$refs和ref指令通常是一起使用的。

首先，我们通过ref给某一个子组件绑定一个特定的ID。

其次，通过this.$refs.ID就可以访问到该组件了。

```vue
<div id="app">
  <cpn></cpn>
  <cpn></cpn>

  <!-- <my-cpn></my-cpn>
  <y-cpn></y-cpn> -->

  <cpn ref="aaa"></cpn>
  <button @click="btnClick">按钮</button>
</div>

<template id="cpn">
  <div>我是子组件</div>
</template>
<script src="./js/vue.js"></script>
<script>
  const app = new Vue({
    el: '#app',
    data: {
      message: 'Dexter'
    },
    methods: {
      btnClick() {
        // 1.$children
        // console.log(this.$children);
        // for (let c of this.$children) {
        //   console.log(c.name);
        //   c.showMessage();
        // }
        // console.log(this.$children[2].name);

        // 2.$refs => 对象类型, 默认是一个空的对象 ref='bbb'
        console.log(this.$refs.aaa.name);
      }
    },
    components: {
      cpn: {
        template: '#cpn',
        data() {
          return {
            name: '我是子组件的name'
          }
        },
        methods: {
          showMessage() {
            console.log('showMessage');
          }
        }
      },
    }
  })
</script>
```

## 父子组件的访问方式： $parent

如果我们想在子组件中直接访问父组件，可以通过$parent

注意事项：

尽管在Vue开发中，我们允许通过$parent来访问父组件，但是在真实开发中尽量不要这样做。

子组件应该尽量避免直接访问父组件的数据，因为这样耦合度太高了。

如果我们将子组件放在另外一个组件之内，很可能该父组件没有对应的属性，往往会引起问题。

另外，更不好做的是通过$parent直接修改父组件的状态，那么父组件中的状态将变得飘忽不定，很不利于我的调试和维护。

<a href="https://imgtu.com/i/j6HXh8"><img src="https://s1.ax1x.com/2022/07/11/j6HXh8.png" alt="j6HXh8.png" border="0" /></a>

## 为什么使用slot

slot翻译为插槽：

在生活中很多地方都有插槽，电脑的USB插槽，插板当中的电源插槽。

插槽的目的是让我们原来的设备具备更多的扩展性。

比如电脑的USB我们可以插入U盘、硬盘、手机、音响、键盘、鼠标等等。

组件的插槽：

组件的插槽也是为了让我们封装的组件更加具有扩展性。

让使用者可以决定组件内部的一些内容到底展示什么。

栗子：

移动网站中的导航栏。

移动开发中，几乎每个页面都有导航栏。

导航栏我们必然会封装成一个插件，比如nav-bar组件。一旦有了这个组件，我们就可以在多个页面中复用了。

### 如何封装这类组件呢？slot

如何去封装这类的组件呢？

它们也很多区别，但是也有很多共性。

如果，我们每一个单独去封装一个组件，显然不合适：

比如每个页面都返回，这部分内容我们就要重复去封装。

但是，如果我们封装成一个，好像也不合理：有些左侧是菜单，有些是返回，有些中间是搜索，有些是文字，等等。

如何封装合适呢？

抽取共性，保留不同。

最好的封装方式就是将共性抽取到组件中，将不同暴露为插槽。

一旦我们预留了插槽，就可以让使用者根据自己的需求，决定插槽中插入什么内容。

是搜索框，还是文字，还是菜单。由调用者自己来决定。

### slot基本使用

了解了为什么用slot，我们再来谈谈如何使用slot？

在子组件中，使用特殊的元素<slot>就可以为子组件开启一个插槽。

该插槽插入什么内容取决于父组件如何使用。

我们通过一个简单的例子，来给子组件定义一个插槽：

有了这个插槽后，父组件如何使用呢？

```vue
<div id="app">
	<cpn></cpn> <!-- 显示 我是插槽的默认内容，-->
	<cpn>
		<h2>哈哈哈哈哈</h2>       <!-- 显示 哈哈哈哈哈-->
		<P>Dexter</P>			<!-- 显示 Dexter -->
	</cpn>
</div>
<template id="cpn">
	<div>
      <!-- slot中的内容表示，如果没有在该组件中插入任何其他内容，就默认显示该内容 -->
		<slot>我是插槽的默认内容</slot>
	</div>
</template>
<script type="text/javascript" src="./js/vue.js"></script>
<script>
	const app = new Vue({
		el: '#app',
		data:{
			message: 'Dexter'
		},
		components:{
			cpn:{
				template:'#cpn'
			}
		}
	})
</script>
```

### 具名插槽slot

当子组件的功能复杂时，子组件的插槽可能并非是一个。

比如我们封装一个导航栏的子组件，可能就需要三个插槽，分别代表左边、中间、右边。

那么，外面在给插槽插入内容时，如何区分插入的是哪一个呢？

这个时候，我们就需要给插槽起一个名字如何使用具名插槽呢？

非常简单，只要给slot元素一个name属性即可<slot name='myslot'></slot>

```vue
<div id="app">
	<cpn></cpn> <!-- 显示 左边 中间 右边-->
	<cpn>
		<button slot="left">返回</button>  <!-- 显示 返回 中间 右边-->
	</cpn>
</div>
<template id="cpn">
	<div>
		<slot name="left">左边</slot>
		<slot name="center">中间</slot>
		<slot name="right">右边</slot>
	</div>
</template>
<script type="text/javascript" src="./js/vue.js"></script>
<script>
	const app = new Vue({
		el: '#app',
		data:{
			message: 'Dexter'
		},
		components:{
			cpn:{
				template:'#cpn'
			}
		}
	})
	
</script>
```

### 作用域插槽：准备

用域插槽是slot一个比较难理解的点，而且官方文档说的又有点不清晰。

这里，我们用一句话对其做一个总结，然后我们在后续的案例中来体会：

父组件替换插槽的标签，但是内容由子组件来提供。

我们先提一个需求：

子组件中包括一组数据，比如：pLanguages: ['JavaScript', 'Python', 'Swift', 'Go', 'C++']

需要在多个界面进行展示：某些界面是以水平方向一一展示的，某些界面是以列表形式展示的，某些界面直接展示一个数组

内容在子组件，希望父组件告诉我们如何展示，怎么办呢？

利用slot作用域插槽就可以了我们来看看子组件的定义：

<a href="https://imgtu.com/i/j2udZ4"><img src="https://s1.ax1x.com/2022/07/12/j2udZ4.png" alt="j2udZ4.png" border="0" /></a>

### 作用域插槽：使用

在父组件使用我们的子组件时，从子组件中拿到数据：

我们通过<template slot-scope="slotProps">获取到slotProps属性在通过slotProps.data就可以获取到刚才我们传入的data了

<a href="https://imgtu.com/i/j2u0o9"><img src="https://s1.ax1x.com/2022/07/12/j2u0o9.png" alt="j2u0o9.png" border="0" /></a>

## 模块化开发

### 匿名函数的解决方案

我们可以使用匿名函数来解决方面的重名问题

```javascript
(function(){
    var flag = true
})()
```

但是如果我们希望在main.js文件中，用到flag，应该如何处理呢？

显然，另外一个文件中不容易使用，因为flag是一个局部变量。

### 使用模块作为出口

我们可以使用将需要暴露到外面的变量，使用一个模块作为出口，什么意思呢？

来看下对应的代码：

```javascript
var ModuleA = (function(){
    // 1.定义一个对象
    var obj ={}
    // 2.在对象内部添加变量和方法
    obj.flag = true
    obj.myFunc = function(info){
        console.log(info);
    }
    // 3.将对象返回
    return obj
})()
//我们做了什么事情呢？
//非常简单，在匿名函数内部，定义一个对象。
//给对象添加各种需要暴露到外面的属性和方法(不需要暴露的直接定义即可)。
//最后将这个对象返回，并且在外面使用了一个MoudleA接受。
```

接下来，我们在man.js中怎么使用呢？

我们只需要使用属于自己模块的属性和方法即可

```javascript
if(ModuleA.flag){
    console.log('Dexter');
}
ModuleA.myFunc('彭于晏');
console.log(ModuleA);
```

这就是模块最基础的封装，事实上模块的封装还有很多高级的话题：

但是我们这里就是要认识一下为什么需要模块，以及模块的原始雏形。

幸运的是，前端模块化开发已经有了很多既有的规范，以及对应的实现方案。

常见的模块化规范：CommonJS、AMD、CMD，也有ES6的Modules

### CommonJS（了解）

模块化有两个核心：导出和导入

CommonJS的导出：

```javascript
module.exports ={
	flag = true,
	test(a,b){
		return a + b
	}
	demo(a,b){
		return a * b
	}
}
```

CommonJS的导入

```javascript
let{test, demo,flag} = require('01.js');
```

### export基本使用

export指令用于导出变量，比如下面的代码：

```javascript
//01.js
export let name = 'Dexter'
export let age = 18
export let height = 1.88
```

上面的代码还有另外一种写法：

```javascript
//01.js
let name = 'Dexter'
let age = 18
let height = 1.88
export(name,age, height)
```

### 导出函数或类

上面我们主要是输出变量，也可以输出函数或者输出类

上面的代码也可以写成这种形式：

```javascript
export function test(aaa){
	console.log(aaa);
}
export class person {
	constructor(name,age){
		this.name = name;
		this.age = age;
	}
	run(){
		console.log(this.name + '在努力');
	}
}
```

或者这种

```javascript
function test(aaa){
	console.log(aaa);
}
class person {
	constructor(name,age){
		this.name = name;
		this.age = age;
	}
	run(){
		console.log(this.name + '在努力');
	}
}
export {test,person}
```

### export default

某些情况下，一个模块中包含某个的功能，我们并不希望给这个功能命名，而且让导入者可以自己来命名

这个时候就可以使用export default

```javascript
//01.js
export default function(){
	console.log('Dexter加油');
}
```

我们来到main.js中，这样使用就可以了

这里的Dexter是我自己命名的，你可以根据需要命名它对应的名字

```javascript
import Dexter from '01.js'
Dexter()
```

另外，需要注意：

export default在同一个模块中，不允许同时存在多个。

### import使用

我们使用export指令导出了模块对外提供的接口，下面我们就可以通过import命令来加载对应的这个模块了

先，我们需要在HTML代码中引入两个js文件，并且类型需要设置为module

```html
<script src="01.js" type="module"></script>
<script src="main.js" type="module"></script>
```

import指令用于导入模块中的内容，比如main.js的代码

```javascript
import {name, age, height} from '01.js'
console.log(name, age, height);
```

如果我们希望某个模块中所有的信息都导入，一个个导入显然有些麻烦：

通过"  *  "可以导入模块中所有的export变量*

但是通常情况下我们需要给 * 起一个别名，方便后续的使用

```javascript
import * as Dexter from '01.js'
console(Dexter.name, Dexter.age, Dexter.height);
```

## 什么是Webpack？

我们先看看官方的解释：At its core, webpack is a static module bundler for modern JavaScript applications. 从本质上来讲，webpack是一个现代的JavaScript应用的静态模块打包工具。

但是它是什么呢？用概念解释概念，还是不清晰。

我们从两个点来解释上面这句话：模块 和 打包

## 和grunt/gulp的对比

grunt/gulp的核心是Task

我们可以配置一系列的task，并且定义task要处理的事务（例如ES6、ts转化，图片压缩，scss转成css）

之后让grunt/gulp来依次执行这些task，而且让整个流程自动化。

所以grunt/gulp也被称为前端自动化任务管理工具。

所以，grunt/gulp和webpack有什么不同呢？

grunt/gulp更加强调的是前端流程的自动化，模块化不是它的核心。

webpack更加强调模块化开发管理，而文件压缩合并、预处理等功能，是他附带的功能。

## webpack安装

安装webpack首先需要安装Node.js，Node.js自带了软件包管理工具npm

查看自己的node版本：

```c++
node -v
```

全局安装webpack(这里我先指定版本号3.6.0，因为vue cli2依赖该版本)

```c++
npm install webpack@3.6.0 -g
```

局部安装webpack（后续才需要）

--save-dev`是开发时依赖，项目打包后不需要继续使用的。

```c#
cd 对应目录
npm install webpack@3.6.0 --save-dev
```

为什么全局安装后，还需要局部安装呢？

在终端直接执行webpack命令，使用的全局安装的webpack

当在package.json中定义了scripts时，其中包含了webpack命令，那么使用的是局部webpack(本地文件)

### 准备工作

**文件和文件夹解析：**

dist文件夹：用于存放之后打包的文件

src文件夹：用于存放我们写的源文件

main.js：项目的入口文件。

mathUtils.js：定义了一些数学工具函数，可以在其他地方引用，并且使用。

index.html：浏览器打开展示的首页html

package.json：通过npm init生成的，npm包管理的文件

### js文件的打包

现在的js文件中使用了模块化的方式进行开发，他们可以直接使用吗？不可以。

因为如果直接在index.html引入这两个js文件，浏览器并不识别其中的模块化代码。

另外，在真实项目中当有许多这样的js文件时，我们一个个引用非常麻烦，并且后期非常不方便对它们进行管理。

我们应该怎么做呢？使用webpack工具，对多个js文件进行打包。

我们知道，webpack就是一个模块化的打包工具，所以它支持我们代码中写模块化，可以对模块化的代码进行处理。（如何处理的，待会儿在原理中，我会讲解）

另外，如果在处理完所有模块之间的关系后，将多个js打包到一个js文件中，引入时就变得非常方便了。

OK，如何打包呢？使用webpack的指令即可

```c
webpack ./src/main.js ./dist/bundle.js
```

### 使用打包后的文件

打包后会在dist文件下，生成一个bundle.js

文件文件内容有些复杂，这里暂时先不看，后续再进行分析。

bundle.js文件，是webpack处理了项目直接文件依赖后生成的一个js文件，我们只需要将这个js文件在index.html中引入即可

### 入口和出口

我们考虑一下，如果每次使用webpack的命令都需要写上入口和出口作为参数，就非常麻烦，有没有一种方法可以将这两个参数写到配置中，在运行时，直接读取呢？

当然可以，就是创建一个webpack.config.js文件

```javascript
//webpack.config.js
const path = require('path')
module.exports = {
	entry: './src/main.js',
	output:{
		path: path.resolve(__dirname,'dist'),//动态获取路径
		filename:'bundle.js'
	}
}
```

### package.json中定义启动

是，每次执行都敲这么一长串有没有觉得不方便呢？

OK，我们可以在package.json的scripts中定义自己的执行脚本。

package.json中的scripts的脚本在执行时，会按照一定的顺序寻找命令对应的位置。

首先，会寻找本地的node_modules/.bin路径中对应的命令。

```javascript
// package.json
{
    ...
    "scripts":{
        //自己添加 比如
        "build": "webpack"
    }
    ...
}
```

如果没有找到，会去全局的环境变量中寻找。

如何执行我们的build指令呢？

```c++
npm run build
```

### 什么是loader？

loader是webpack中一个非常核心的概念。

**webpack用来做什么呢？**

在我们之前的实例中，我们主要是用webpack来处理我们写的js代码，并且webpack会自动处理js之间相关的依赖。

但是，在开发中我们不仅仅有基本的js代码处理，我们也需要加载css、图片，也包括一些高级的将ES6转成ES5代码，将TypeScript转成ES5代码，将scss、less转成css，将.jsx、.vue文件转成js文件等等。对于webpack本身的能力来说，对于这些转化是不支持的。那怎么办呢？

给webpack扩展对应的loader就可以啦。

**loader使用过程：**

步骤一：通过npm安装需要使用的loade

步骤二：在webpack.config.js中的modules关键字下进行配置

大部分loader我们都可以在webpack的官网中找到，并且学习对应的用法。

https://www.webpackjs.com/concepts/ //中文网 文档

### css文件处理 - 准备工作

在入口文件中引用：

```javascript
// main.js
require('./css/Dexter.css')
```

### css文件处理

**安装css-loader**

```c++
npm install --save-dev css-loader
```

**安装style-loader**

```c++
npm install --save-dev style-loader
```

**并且在webpack.config.js的配置如下**

```JavaScript
//webpack.config.js
const path = require('path')
module.exports = {
	entry: './src/main.js',
	output:{
		path: path.resolve(__dirname,'dist'),//动态获取路径
		filename:'bundle.js'
	},
    module: {
    rules: [
      {
        test: /\.css$/i,
        use: ["style-loader", "css-loader"],
      },
    ],
}
```

### less文件处理 – 准备工作

我们还是先创建一个less文件，依然放在css文件夹中

**在入口文件中引用：**

```javascript
// main.js
require('./css/Dexter01.less')
```

**安装less**

```c++
npm install less less-loader --save-dev
```

其次，修改对应的配置文件

添加一个rules选项，用于处理.less文件

```javascript
//webpack.config.js
const path = require('path')
module.exports = {
	entry: './src/main.js',
	output:{
		path: path.resolve(__dirname,'dist'),//动态获取路径
		filename:'bundle.js'
	},
    module: {
    rules: [
      {
        test: /\.css$/i,
        use: ["style-loader", "css-loader"],
      },
      {
        test: /\.less$/i,
        use: [
          // compiles Less to CSS
          "style-loader",
          "css-loader",
          "less-loader",
        ],
      },
    ],
}
```

### 图片文件处理 – 资源准备阶段

**安装url-loader**

```c++
npm install --save-dev url-loader
```

**修改webpack.config.js配置文件：**

```javascript
//webpack.config.js
const path = require('path')
module.exports = {
	entry: './src/main.js',
	output:{
		path: path.resolve(__dirname,'dist'),//动态获取路径
		filename:'bundle.js'
	},
    module: {
    rules: [
      {
        test: /\.css$/i,
        use: ["style-loader", "css-loader"],
      },
      {
        test: /\.less$/i,
        use: [
          // compiles Less to CSS
          "style-loader",
          "css-loader",
          "less-loader",
        ],
      },
        {
        test: /\.(png|jpg|gif|jpeg)$/i,
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 8192,
            },
          },
        ],
      },
    ],
}
```

**安装file-loader**

```c++
npm install --save-dev file-loader
```

**图片文件处理 – 修改文件名称**

我们发现webpack自动帮助我们生成一个非常长的名字

这是一个32位hash值，目的是防止名字重复

但是，真实开发中，我们可能对打包的图片名字有一定的要求比如，将所有的图片放在一个文件夹中，跟上图片原来的名称，同时也要防止重复

所以，我们可以在options中添加上如下选项：

img：文件要打包到的文件夹name：获取图片原来的名字，放在该位置

hash:8：为了防止图片名称冲突，依然使用hash，但是我们只保留8位

ext：使用图片原来的扩展名

但是，我们发现图片并没有显示出来，这是因为图片使用的路径不正确默认情况下，webpack会将生成的路径直接返回给使用者

```javascript
//webpack.config.js
const path = require('path')
module.exports = {
	entry: './src/main.js',
	output:{
		path: path.resolve(__dirname,'dist'),//动态获取路径
		filename:'bundle.js'
        publicPath:'dist/'   //我们整个程序是打包在dist文件夹下的，所以这里我们需要在路径下再添加一个dist/
	},
    module: {
    rules: [
      {
        test: /\.css$/i,
        use: ["style-loader", "css-loader"],
      },
      {
        test: /\.less$/i,
        use: [
          // compiles Less to CSS
          "style-loader",
          "css-loader",
          "less-loader",
        ],
      },
        {
        test: /\.(png|jpg|gif|jpeg)$/i,
        use: [
          {
            loader: 'url-loader',
            options: {
              limit: 8192,
              name:'img/[name].[hash:8].[ext]' //options中添加
            },
          },
        ],
      },
    ],
}
```

### ES6语法处理

如果希望将ES6的语法转成ES5，那么就需要使用babel。

而在webpack中，我们直接使用babel对应的loader就可以了。

**安装babel**

```c++
npm install --save-dev babel-loader@7 babel-core babel-preset-es2015
```

**配置webpack.config.js文件**

```javascript
{
      test: /\.m?js$/,
      exclude: /(node_modules|bower_components)/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: ['@babel/preset-env']
        }
      }
 }
```

### 引入vue.js

**安装vue依赖**

```javascript
npm install vue --save  //因为我们后续是在实际项目中也会使用vue的，所以并不是开发时依赖
```

改完成后，重新打包，运行程序：

打包过程没有任何错误(因为只是多打包了一个vue的js文件而已)

但是运行程序，没有出现想要的效果，而且浏览器中有报错

这个错误说的是我们使用的是runtime-only版本的Vue，什么意思呢？

这里我只说解决方案：Vue不同版本构建，后续我具体讲解runtime-only和runtime-compiler的区别。

**修改webpack.config.js文件**

```javascript
//webpack.config.js
const path = require('path')
module.exports = {
	entry: './src/main.js',
	output:{
		path: path.resolve(__dirname,'dist'),//动态获取路径
		filename:'bundle.js'
        publicPath:'dist/'   //我们整个程序是打包在dist文件夹下的，所以这里我们需要在路径下再添加一个dist/
	},
    module: {....},
    resolve:{
        alias:{
            'vue$':'vue/dist/vue.esm.js'
        }
    },
}   
```

### Vue组件化开发引入

```javascript
//main.js

// 1.使用commonjs的模块化规范
const {add, mul} = require('./js/mathUtils.js')

console.log(add(20, 30));
console.log(mul(20, 30));

// 2.使用ES6的模块化的规范
import {name, age, height} from "./js/info";

console.log(name);
console.log(age);
console.log(height);

// 3.依赖css文件
require('./css/normal.css')

// 4.依赖less文件
require('./css/special.less')

// 5.使用Vue进行开发
import Vue from 'vue'
import App from './vue/App.vue'

new Vue({
  el: '#app',
  template: '<App/>',
  components: {
    App
  }
})
```

### .vue文件封装处理

```javascript
// App.vue
<template>
  <div>
    <h2 class="title">{{message}}</h2>
    <button @click="btnClick">按钮</button>
    <h2>{{name}}</h2>
    <Cpn/> //实现Cpn组件
  </div>
</template>

<script>
  import Cpn from './Cpn' //引入Cpn组件

  export default {
    name: "App",
    components: {
      Cpn
    },
    data() {
      return {
        message: 'Hello Webpack',
        name: 'Dexter'
      }
    },
    methods: {
      btnClick() {

      }
    }
  }
</script>

<style scoped> 
  .title { //样式
    color: green;
  }
</style>
```

**安装vue-loader和vue-template-compiler**

```c++
npm install vue-loader vue-template-compiler --save-dev
```

**修改webpack.config.js的配置文件：**

```javascript
{
        test: /\.vue$/,
        use: ['vue-loader']
}
```

### 认识plugin

**plugin是什么？**

plugin是插件的意思，通常是用于对某个现有的架构进行扩展。

webpack中的插件，就是对webpack现有功能的各种扩展，比如打包优化，文件压缩等等。

**loader和plugin区别**

loader主要用于转换某些类型的模块，它是一个转换器。

plugin是插件，它是对webpack本身的扩展，是一个扩展器。

**plugin的使用过程：**

步骤一：通过npm安装需要使用的plugins(某些webpack已经内置的插件不需要安装)

步骤二：在webpack.config.js中的plugins中配置插件。

下面，我们就来看看可以通过哪些插件对现有的webpack打包过程进行扩容，让我们的webpack变得更加好用。

我们先来使用一个最简单的插件，为打包的文件添加版权声明

该插件名字叫BannerPlugin，属于webpack自带的插件。

按照下面的方式来修改webpack.config.js的文件：

```javascript
//webpack.config.js
const path = require('path')
const webpack = require('webpack')
module.exports = {
    ...
    plugins:[
        new webpack.BannerPlugin('最终版权归Dexter所有')
    ]
}   
```

重新打包程序：查看bundle.js文件的头部，

```javascript
/*! 最终版权归Dexter所有 */
```

### 打包html的plugin

目前，我们的index.html文件是存放在项目的根目录下的。

我们知道，在真实发布项目时，发布的是dist文件夹中的内容，但是dist文件夹中如果没有index.html文件，那么打包的js等文件也就没有意义了。

所以，我们需要将index.html文件打包到dist文件夹中，这个时候就可以使用

HtmlWebpackPlugin插件HtmlWebpackPlugin插件可以为我们做这些事情：

自动生成一个index.html文件(可以指定模板来生成)

将打包的js文件，自动通过script标签插入到body中

**安装HtmlWebpackPlugin插件**

```javascript
npm install html-webpack-plugin --save-dev
```

使用插件，修改webpack.config.js文件中plugins部分的内容如下：

```javascript
//webpack.config.js
const path = require('path')
const webpack = require('webpack')
module.exports = {
    ...
    plugins:[
        new webpack.BannerPlugin('最终版权归Dexter所有')
    	new htmlWebpackPlugin({
          template:'index.html'
        }),
    ]
}   
//这里的template表示根据什么模板来生成index.html
//另外，我们需要删除之前在output中添加的publicPath属性
//否则插入的script标签中的src可能会有问题
```

### js压缩的Plugin

在项目发布之前，我们必然需要对js等文件进行压缩处理

这里，我们就对打包的js文件进行压缩

我们使用一个第三方的插件uglifyjs-webpack-plugin，并且版本号指定1.1.1，和CLI2保持一致

```javascript
npm install uglifyjs-webpack-plugin@1.1.1 --save-dev
```

修改webpack.config.js文件，使用插件：

```javascript
//webpack.config.js
const path = require('path')
const webpack = require('webpack')
const uglifyJsPlugin = require('uglifyjs-webpack-plugin')
module.exports = {
    ...
    plugins:[
        new webpack.BannerPlugin('最终版权归Dexter所有')
    	new htmlWebpackPlugin({
          template:'index.html'
        }),
        new uglifyJsPlugin()
    ]
}   
```

### 搭建本地服务器

webpack提供了一个可选的本地开发服务器，这个本地服务器基于node.js搭建，内部使用express框架，可以实现我们想要的让浏览器自动刷新显示我们修改后的结果。

不过它是一个单独的模块，在webpack中使用之前需要先安装它

```javascript
npm install --save-dev webpack-dev-server@2.9.1
```

webpack.config.js文件配置修改如下：

```javascript
//webpack.config.js
  devServer: {
    contentBase:'./dist'
    inline: true
  }
```

再配置package.json

```javascript
//package.json
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack",
    "dev":"webpack-dev-server --open" //--open参数表示直接打开浏览器
  },
```

### 配置文件的分离

```javascript
npm install webpack-merge
```

## Vue CLI的使用

### 安装Vue脚手架

```javascript
npm install -g @vue/cli
```

**再安装一个全局加载项**

```javascript
npm install -g @vue/cli-init
```

