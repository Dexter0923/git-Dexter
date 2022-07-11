## 什么是组件化？

人面对复杂问题的处理方式：

任何一个人处理信息的逻辑能力都是有限的

所以，当面对一个非常复杂的问题时，我们不太可能一次性搞定一大堆的内容。

但是，我们人有一种天生的能力，就是将问题进行拆解。

如果将一个复杂的问题，拆分成很多个可以处理的小问题，再将其放在整体当中，你会发现大的问题也会迎刃而解。

组件化也是类似的思想：

如果我们将一个页面中所有的处理逻辑全部放在一起，处理起来就会变得非常复杂，而且不利于后续的管理以及扩展。

但如果，我们讲一个页面拆分成一个个小的功能块，每个功能块完成属于自己这部分独立的功能，那么之后整个页面的管理和维护就变得非常容易了。

## Vue组件化思想

组件化是Vue.js中的重要思想

它提供了一种抽象，让我们可以开发出一个个独立可复用的小组件来构造我们的应用。

任何的应用都会被抽象成一颗组件树。

组件化思想的应用：

有了组件化的思想，我们在之后的开发中就要充分的利用它。

尽可能的将页面拆分成一个个小的、可复用的组件。

这样让我们的代码更加方便组织和管理，并且扩展性也更强。所以，组件是Vue开发中，非常重要的一个篇章，要认真学习。

## 注册组件的基本步骤

组件的使用分成三个步骤：

创建组件构造器     Vue.extend()方法   

注册组件				Vue.component() 方法      component：组成部分

使用组件。  在Vue实例的作用范围内使用

```vue
<div id="app">
	<!-- 3.使用组件 在Vue实例的作用范围内使用 -->
  <my-cpn></my-cpn>
	<my-cpn></my-cpn>
	<my-cpn></my-cpn> 
</div> 
<script type="text/javascript" src="./js/vue.js"></script>
<script>
	
	  // 1.创建组件构造器  Vue.extend()方法   
		const myComponent = Vue.extend({
			// ES6
			// template:`` 这个符号 键盘Tab上面
		   template: ` 
		        <div>
		          <h2>组件标题</h2>
		          <p>我是组件中的一个段落内容</p>
		        </div>`
		});
		
	// 2.注册组件,并且定义组件标签的名称  Vue.component() 方法      component：组成部分
	Vue.component('my-cpn',myComponent)
	
	const app = new Vue({
	  el: '#app',
	  data: {
	    message: '你好啊'
	  }
	})
</script>
```

## 注册组件步骤解析

这里的步骤都代表什么含义呢？

### 1.Vue.extend()：

调用Vue.extend()创建的是一个组件构造器。 

通常在创建组件构造器时，传入template代表我们自定义组件的模板。

该模板就是在使用到组件的地方，要显示的HTML代码。

### 2.Vue.component()：

调用Vue.component()是将刚才的组件构造器注册为一个组件，并且给它起一个组件的标签名称。所以需要传递两个参数：1、注册组件的标签名 2、组件构造器

### 3.组件必须挂载在某个Vue实例下，否则它不会生效

## 全局组件和局部组件

当我们通过调用Vue.component()注册组件时，组件的注册是全局的这意味着该组件可以在任意Vue示例下使用。

```vue
<script>
    //全局组件
	Vue.component('my-cpn',myComponent)
</script>
```

如果我们注册的组件是挂载在某个实例中, 那么就是一个局部组件

```vue
<script>		
	// 局部组件
	const app2 = new Vue({
		el:'#app2',
		components:{
			'my-cpn': myComponent
		}
	})
</script>
```

## 父组件和子组件

在前面我们看到了组件树：

组件和组件之间存在层级关系

而其中一种非常重要的关系就是父子组件的关系我们来看通过代码如何组成的这种层级关系

```vue
<div id="app">
	<my-father></my-father>
</div>
<script type="text/javascript" src="./js/vue.js"></script>
<script>
	const son = Vue.extend({
		template:`
			<div>
			<h2>我是子组件标题</h2>
			<p>我是子组件的内容</p>
			</div>
		`
	})
	const father = Vue.extend({
		template:`
				<div>
				<h2>我是父组件标题</h2>
				<p>我是父组件的内容</p>
				<my-son></my-son>
				</div>
		`,
		components:{
			'my-son':son
		}
	})
	

	const app = new Vue({
		el: '#app',
		data:{
			message: 'Dexter'
		},
		components:{
			'my-father':father
		}
	})
</script>
```

## 语法糖注册全局组件和局部组件

### 全局组件

```vue
<script>
	// 创建组件构造器
	//const  myComponent= Vue.extend({
	//	template: `
	//	     <div>
	//	       <h2>组件标题</h2>
	//	       <p>我是组件中的一个段落内容</p>
	//	     </div>`
	//});
	// 注册全局组件
	// Vue.component('my-cpn',myComponent)
	// 注册全局组件的语法糖
	Vue.component('my-cpn',{
		template: `
		     <div>
		       <h2>组件标题</h2>
		       <p>我是组件中的一个段落内容</p>
		     </div>
				 `
	})
	const app = new Vue({
		el: '#app',
		data:{
			message: 'Dexter'
		}
	})
</script>
</body>
</html>

```

### 局部组件

```vue
<script>
	// 创建组件构造器
	// const  myComponent= Vue.extend({
	// 	template: `
	// 	     <div>
	// 	       <h2>组件标题</h2>
	// 	       <p>我是组件中的一个段落内容</p>
	// 	     </div>`
	// })
	// const app = new Vue({
	// 	el: '#app',
	// 	data:{
	// 		message: 'Dexter'
	// 	},
		//局部组件
		// components:{
		// 	'my-cpn':myComponent
		// }
		
	// })
	//局部组件语法糖
	const app = new Vue({
		el: '#app',
		data:{
			message: 'Dexter'
		},
		components:{
			'my-cpn':{
				template: `
				     <div>
				       <h2>组件标题</h2>
				       <p>我是组件中的一个段落内容</p>
				     </div>`
			}
		}
	})
	
</script>
```

## 模板的分离写法

刚才，我们通过语法糖简化了Vue组件的注册过程，另外还有一个地方的写法比较麻烦，就是template模块写法。

如果我们能将其中的HTML分离出来写，然后挂载到对应的组件上，必然结构会变得非常清晰。

Vue提供了两种方案来定义HTML模块内容：

### 使用script标签

```vue
<div id="app">
	<my-cpn></my-cpn>
</div>
<script type="text/x-template" id="myCpn">
	<div>
	  <h2>组件标题</h2>
	  <p>我是组件中的一个段落内容</p>
	</div>
</script>
<script type="text/javascript" src="./js/vue.js"></script>
<script>
	const app = new Vue({
		el: '#app',
		data:{
			message: 'Dexter'
		},
		components:{
			'my-cpn':{
				template:'#myCpn'
			}
		}
	})
	
</script>
```

### 使用template标签

```vue
<div id="app">
	<my-cpn></my-cpn>
</div>
<template id="myCpn">
	<div>
		<h2>组件标题</h2>
		<p>我是组件中的一个段落内容</p>
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
			'my-cpn':{
				template:'#myCpn'
			}
		}
	})
</script>
```

## 组件数据存放

### 组件可以访问Vue实例数据吗?

不能  

组件是一个单独功能模块的封装：

这个模块有属于自己的HTML模板，也应该有属性自己的数据data。

```vue
<div id="app">
	<my-cpn></my-cpn>
</div>
<template id="myCpn">
		<!-- 解析: -->
		<!-- 组件去访问message message定义在Vue我们发现最终并没有显示结果。 -->
		<!-- 结论: -->
		<!-- 组件是不能直接访问Vue实例中的data数据 -->
	<div>
		名字:{{message}}
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
			'my-cpn':{
				template:'myCpn'
			}
		}
	})
	
</script>
```

### 组件数据的存放

组件自己的数据存放在哪里呢?

组件对象也有一个data属性(也可以有methods等属性，下面我们有用到)

只是这个data属性必须是一个函数

而且这个函数返回一个对象，对象内部保存着数据

```vue
<div id="app">
	<my-cpn></my-cpn>
</div>
<template id="myCpn">
	<div>
		<p>名字:{{message}}</p>
	</div>
</template>
<script type="text/javascript" src="./js/vue.js"></script>
<script>
    // 解析:
	// Dexter可以显示
	// 原因:
	// 这是因为目前组件访问的是自己当中的data
	const app = new Vue({
		el: '#app',
		components:{
			'my-cpn':{
				template:'#myCpn',
				data(){
					return {
						message: 'Dexter'
					}
				}
			}
		}
	})
	
</script>
```

### 为什么是一个函数呢?

为什么data在组件中必须是一个函数呢?

首先，如果不是一个函数，Vue直接就会报错。

其次，原因是在于Vue让每个组件对象都返回一个新的对象，

因为如果是同一个对象的，组件在多次使用后会相互影响。

## 父子组件的通信

在上一个小节中，我们提到了子组件是不能引用父组件或者Vue实例的数据的。

但是，在开发中，往往一些数据确实需要从上层传递到下层：

比如在一个页面中，我们从服务器请求到了很多的数据。

其中一部分数据，并非是我们整个页面的大组件来展示的，而是需要下面的子组件进行展示。

这个时候，并不会让子组件再次发送一个网络请求，而是直接让大组件(父组件)将数据传递给小组件(子组件)。

如何进行父子组件间的通信呢？Vue官方提到

通过props向子组件传递数据

通过事件向父组件发送消息

<a href="https://imgtu.com/i/jat9fO"><img src="https://s1.ax1x.com/2022/07/06/jat9fO.png" alt="jat9fO.png" border="0" /></a>

在下面的代码中，我直接将Vue实例当做父组件，并且其中包含子组件来简化代码。真实的开发中，Vue实例和子组件的通信和父组件和子组件的通信过程是一样的。

### props基本用法

在组件中，使用选项props来声明需要从父级接收到的数据。

props的值有两种方式：

方式一：字符串数组，数组中的字符串就是传递时的名称。

方式二：对象，对象可以设置传递时的类型，也可以设置默认值等。

我们先来看一个最简单的props传递：

<a href="https://imgtu.com/i/jatptK"><img src="https://s1.ax1x.com/2022/07/06/jatptK.png" alt="jatptK.png" border="0" /></a>

### props数据验证

在前面，我们的props选项是使用一个数组。

我们说过，除了数组之外，我们也可以使用对象，当需要对props进行类型等验证时，就需要对象写法了。

验证都支持哪些数据类型呢？

String、Number、Boolean、Array、Object、Date、Function、Symbol

<a href="https://imgtu.com/i/jaBcsU"><img src="https://s1.ax1x.com/2022/07/06/jaBcsU.png" alt="jaBcsU.png" border="0" /></a>

当我们有自定义构造函数时，验证也支持自定义的类型

<a href="https://imgtu.com/i/jaBrR0"><img src="https://s1.ax1x.com/2022/07/06/jaBrR0.png" alt="jaBrR0.png" border="0" /></a>

### props驼峰标识

```vue
<div id="app">
	<!-- <cpn :cInfo='info'></cpn>  因为有cInfo有大写 -props驼峰标识 只是显示{} 会错误-->
	<!-- 下面这样 cInfo换成 c-info 才是正确的-->
	<cpn :c-info='info'></cpn>  
</div>
<template id="cpn">
	<div>
		<h2>{{cInfo}}</h2>
	</div>
</template>
<script type="text/javascript" src="./js/vue.js"></script>
<script>
// 这个是子
  const cpn = {
		template:'#cpn',
		props:{
			cInfo:{
				type:Object,
				default() {
					return {}
				}
			}
		}
	}
	
	// Vue实例是父
	const app = new Vue({  
		el: '#app',
		data:{
			info:{
				name:'Dexter',
				age:18,
				height:1.88
			}
		},
		components:{
			cpn
		}
	})
</script>
```

## 子级向父级传递

props用于父组件向子组件传递数据，还有一种比较常见的是子组件传递数据或事件到父组件中。我们应该如何处理呢？这个时候，我们需要使用自定义事件来完成。

什么时候需要自定义事件呢？

当子组件需要向父组件传递数据时，就要用到自定义事件了。

我们之前学习的v-on不仅仅可以用于监听DOM事件，也可以用于组件间的自定义事件。

自定义事件的流程：

在子组件中，通过$emit()来触发事件。

在父组件中，通过v-on来监听子组件事件。

```vue
<!--父组件模板-->
<div id="app">
	<cpn @item-click='cpnClick'></cpn>
</div>

<!--子组件模板-->
<template id="cpn">
	<div>
		<button v-for="item in categories"
				@click="btnClick(item)">
			{{item.name}}
		</button>
	</div>
</template>
<script type="text/javascript" src="./js/vue.js"></script>
<script>
	// 1.子组件
  const cpn ={
		template: '#cpn',
		data(){
			return{
				// category的复数 类别
				categories:[
					{id:'01', name:'热门推荐'},
					{id:'02', name:'手机数码'},
					{id:'03', name:'家用家电'},
					{id:'04', name:'电脑办公'},
				]
			}
		},
		methods:{
			btnClick(item){
				 // 发射事件: 自定义事件
				this.$emit('item-click',item)
			}
		}
		
	}
	
	// 父组件
	const app = new Vue({
		el: '#app',
		data:{
			message: 'Dexter'
		},
		components:{
			cpn
		},
		methods:{
			cpnClick(item){
				console.log('cpnClick',item)
			}
		}
	})
	
</script>
```

## 自定义事件代码

<a href="https://imgtu.com/i/jBtzVJ"><img src="https://s1.ax1x.com/2022/07/08/jBtzVJ.png" alt="jBtzVJ.png" border="0" /></a>

## 父子组件双向绑定案例

```vue
<div id="app">
  <cpn :number1="num1"
       :number2="num2"
       @num1change="num1change"
       @num2change="num2change"/>
</div>

<template id="cpn">
  <div>
    <h2>props:{{number1}}</h2>
    <h2>data:{{dnumber1}}</h2>
    <!--<input type="text" v-model="dnumber1">-->
    <input type="text" :value="dnumber1" @input="num1Input">
    <h2>props:{{number2}}</h2>
    <h2>data:{{dnumber2}}</h2>
    <!--<input type="text" v-model="dnumber2">-->
    <input type="text" :value="dnumber2" @input="num2Input">
  </div>
</template>

<script src="./js/vue.js"></script>
<script>
  const app = new Vue({
    el: '#app',
    data: {
      num1: 1,
      num2: 0
    },
    methods: {
      num1change(value) {
        this.num1 = parseFloat(value)
      },
      num2change(value) {
        this.num2 = parseFloat(value)
      }
    },
    components: {
      cpn: {
        template: '#cpn',
        props: {
          number1: Number,
          number2: Number
        },
        data() {
          return {
            dnumber1: this.number1,
            dnumber2: this.number2
          }
        },
        methods: {
          num1Input(event) {
            // 1.将input中的value赋值到dnumber中
            this.dnumber1 = event.target.value;

            // 2.为了让父组件可以修改值, 发出一个事件
            this.$emit('num1change', this.dnumber1)
           

            // 3.同时修饰dnumber2的值
            this.dnumber2 = this.dnumber1 * 100;
            this.$emit('num2change', this.dnumber2);
          },
          num2Input(event) {
            this.dnumber2 = event.target.value;
            this.$emit('num2change', this.dnumber2)

            // 同时修饰dnumber2的值
            this.dnumber1 = this.dnumber2 / 100;
            this.$emit('num1change', this.dnumber1);
          }
        }
      }
    }
  })
</script>

```

## 画图分析

<a href="https://imgtu.com/i/jrZvE8"><img src="https://s1.ax1x.com/2022/07/09/jrZvE8.png" alt="jrZvE8.png" border="0" /></a>
