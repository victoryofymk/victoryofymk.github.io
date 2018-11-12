---
title: VUE(一)：VUE简介
date: 2018-10-23 16:12:48
categories: "vue" #文章分類目錄 可以省略
comments: true
tags:
- vue #文章標籤 可以省略
- web
---

## 简介
[Vue.js](https://github.com/vuejs/vue)是当下很火的一个JavaScript MVVM库，它是以数据驱动和组件化的思想构建的。相比于Angular.js，Vue.js提供了更加简洁、更易于理解的API，使得我们能够快速地上手并使用Vue.js。

## MVVM模式
下图不仅概括了MVVM模式（Model-View-ViewModel），还描述了在Vue.js中ViewModel是如何和View以及Model进行交互的
![](https://raw.githubusercontent.com/victoryofymk/victoryofymk.github.io/images/20181025103023.png)
ViewModel是Vue.js的核心，它是一个Vue实例。Vue实例是作用于某一个HTML元素上的，这个元素可以是HTML的body元素，也可以是指定了id的某个元素。

当创建了ViewModel后，双向绑定是如何达成的呢？

首先，我们将上图中的DOM Listeners和Data Bindings看作两个工具，它们是实现双向绑定的关键。
从View侧看，ViewModel中的DOM Listeners工具会帮我们监测页面上DOM元素的变化，如果有变化，则更改Model中的数据；
从Model侧看，当我们更新Model中的数据时，Data Bindings工具会帮我们更新页面中的DOM元素。

## 直接引用
在工程中直接引用vue，后续介绍使用vue-cli搭建vue工程化
### Hello World示例
```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>

	<body>
		<!--这是我们的View-->
		<div id="app">
			{{ message }}
		</div>
	</body>
	<script src="js/vue.js"></script>
	<script>
		// 这是我们的Model
		var exampleData = {
			message: 'Hello World!'
		}

		// 创建一个 Vue 实例或 "ViewModel"
		// 它连接 View 与 Model
		new Vue({
			el: '#app',
			data: exampleData
		})
	</script>
</html>
```
使用Vue的过程就是定义MVVM各个组成部分的过程的过程。

定义View
定义Model
创建一个Vue实例或"ViewModel"，它用于连接View和Model
在创建Vue实例时，需要传入一个选项对象，选项对象可以包含数据、挂载元素、方法、模生命周期钩子等等。

```
在这个示例中，选项对象的el属性指向View，el: '#app'表示该Vue实例将挂载到<div id="app">...</div>这个元素；data属性指向Model，data: exampleData表示我们的Model是exampleData对象。
Vue.js有多种数据绑定的语法，最基础的形式是文本插值，使用一对大括号语法，在运行时{{ message }}会被数据对象的message属性替换，所以页面上会输出"Hello World!"。
```

### 双向绑定示例
MVVM模式本身是实现了双向绑定的，在Vue.js中可以使用v-model指令在表单元素上创建双向数据绑定。
```
<!--这是我们的View-->
<div id="app">
	<p>{{ message }}</p>
	<input type="text" v-model="message"/>
</div>
将message绑定到文本框，当更改文本框的值时，<p>{{ message }}</p> 中的内容也会被更新。反过来，如果改变message的值，文本框的值也会被更新，我们可以在Chrome控制台进行尝试。
```
### Vue.js的常用指令
Vue.js的指令是以v-开头的，它们作用于HTML元素，指令提供了一些特殊的特性，将指令绑定在元素上时，指令会为绑定的目标元素添加一些特殊的行为，我们可以将指令看作特殊的HTML特性（attribute）。
```
v-if指令
v-show指令
v-else指令
v-for指令
v-bind指令
v-on指令
```
#### v-if指令
v-if是条件渲染指令，它根据表达式的真假来删除和插入元素，它的基本语法如下：v-if="expression"
expression是一个返回bool值的表达式，表达式可以是一个bool属性，也可以是一个返回bool的运算式。例如：

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<h1>Hello, Vue.js!</h1>
			<h1 v-if="yes">Yes!</h1>
			<h1 v-if="no">No!</h1>
			<h1 v-if="age >= 25">Age: {{ age }}</h1>
			<h1 v-if="name.indexOf('jack') >= 0">Name: {{ name }}</h1>
		</div>
	</body>
	<script src="js/vue.js"></script>
	<script>
		
		var vm = new Vue({
			el: '#app',
			data: {
				yes: true,
				no: false,
				age: 28,
				name: 'keepfool'
			}
		})
	</script>
</html>
```
注意：yes, no, age, name这4个变量都来源于Vue实例选项对象的data属性。
这段代码使用了4个表达式：

* 数据的yes属性为true，所以"Yes!"会被输出；
* 数据的no属性为false，所以"No!"不会被输出；
* 运算式age >= 25返回true，所以"Age: 28"会被输出；
* 运算式name.indexOf('jack') >= 0返回false，所以"Name: keepfool"不会被输出。
注意：v-if指令是根据条件表达式的值来执行元素的插入或者删除行为。

#### v-show指令
v-show也是条件渲染指令，和v-if指令不同的是，使用v-show指令的元素始终会被渲染到HTML，它只是简单地为元素设置CSS的style属性。

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<h1>Hello, Vue.js!</h1>
			<h1 v-show="yes">Yes!</h1>
			<h1 v-show="no">No!</h1>
			<h1 v-show="age >= 25">Age: {{ age }}</h1>
			<h1 v-show="name.indexOf('jack') >= 0">Name: {{ name }}</h1>
		</div>
	</body>
	<script src="js/vue.js"></script>
	<script>
		
		var vm = new Vue({
			el: '#app',
			data: {
				yes: true,
				no: false,
				age: 28,
				name: 'keepfool'
			}
		})
	</script>
</html>
```
在Chrome控制台更改age属性，使得表达式age >= 25的值为false，可以看到<h1>Age: 24</h1>元素被设置了style="display:none"样式。
#### v-else指令
可以用v-else指令为v-if或v-show添加一个“else块”。v-else元素必须立即跟在v-if或v-show元素的后面——否则它不能被识别。

```
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<h1 v-if="age >= 25">Age: {{ age }}</h1>
			<h1 v-else>Name: {{ name }}</h1>
			<h1>---------------------分割线---------------------</h1>
			<h1 v-show="name.indexOf('keep') >= 0">Name: {{ name }}</h1>
			<h1 v-else>Sex: {{ sex }}</h1>
		</div>
	</body>
	<script src="js/vue.js"></script>
	<script>
		var vm = new Vue({
			el: '#app',
			data: {
				age: 28,
				name: 'keepfool',
				sex: 'Male'
			}
		})
	</script>
</html>
```
v-else元素是否渲染在HTML中，取决于前面使用的是v-if还是v-show指令。
这段代码中v-if为true，后面的v-else不会渲染到HTML；v-show为tue，但是后面的v-else仍然渲染到HTML了。
#### v-for指令
v-for指令基于一个数组渲染一个列表，它和JavaScript的遍历语法相似：

```
v-for="item in items"
```

items是一个数组，item是当前被遍历的数组元素。

```
<!DOCTYPE html>
<html>

	<head>
		<meta charset="UTF-8">
		<title></title>
		<link rel="stylesheet" href="styles/demo.css" />
	</head>

	<body>
		<div id="app">
			<table>
				<thead>
					<tr>
						<th>Name</th>
						<th>Age</th>
						<th>Sex</th>
					</tr>
				</thead>
				<tbody>
					<tr v-for="person in people">
						<td>{{ person.name  }}</td>
						<td>{{ person.age  }}</td>
						<td>{{ person.sex  }}</td>
					</tr>
				</tbody>
			</table>
		</div>
	</body>
	<script src="js/vue.js"></script>
	<script>
		var vm = new Vue({
			el: '#app',
			data: {
				people: [{
					name: 'Jack',
					age: 30,
					sex: 'Male'
				}, {
					name: 'Bill',
					age: 26,
					sex: 'Male'
				}, {
					name: 'Tracy',
					age: 22,
					sex: 'Female'
				}, {
					name: 'Chris',
					age: 36,
					sex: 'Male'
				}]
			}
		})
	</script>

</html>

```
#### v-bind指
v-bind指令可以在其名称后面带一个参数，中间放一个冒号隔开，这个参数通常是HTML元素的特性（attribute），例如：v-bind:class

```
v-bind:argument="expression"
```

下面这段代码构建了一个简单的分页条，v-bind指令作用于元素的class特性上。
这个指令包含一个表达式，表达式的含义是：高亮当前页。

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
		<link rel="stylesheet" href="styles/demo.css" />
	</head>
	<body>
		<div id="app">
			<ul class="pagination">
				<li v-for="n in pageCount">
					<a href="javascripit:void(0)" v-bind:class="activeNumber === n + 1 ? 'active' : ''">{{ n + 1 }}</a>
				</li>
			</ul>
		</div>
	</body>
	<script src="js/vue.js"></script>
	<script>
		var vm = new Vue({
			el: '#app',
			data: {
				activeNumber: 1,
				pageCount: 10
			}
		})
	</script>
</html>
```
#### v-on指令
v-on指令用于给监听DOM事件，它的用语法和v-bind是类似的，例如监听<a>元素的点击事件：

```
<a v-on:click="doSomething">
```

有两种形式调用方法：绑定一个方法（让事件指向方法的引用），或者使用内联语句。
Greet按钮将它的单击事件直接绑定到greet()方法，而Hi按钮则是调用say()方法。

```
<!DOCTYPE html>
<html>
	<head>
		<meta charset="UTF-8">
		<title></title>
	</head>
	<body>
		<div id="app">
			<p><input type="text" v-model="message"></p>
			<p>
				<!--click事件直接绑定一个方法-->
				<button v-on:click="greet">Greet</button>
			</p>
			<p>
				<!--click事件使用内联语句-->
				<button v-on:click="say('Hi')">Hi</button>
			</p>
		</div>
	</body>
	<script src="js/vue.js"></script>
	<script>
		var vm = new Vue({
			el: '#app',
			data: {
				message: 'Hello, Vue.js!'
			},
			// 在 `methods` 对象中定义方法
			methods: {
				greet: function() {
					// // 方法内 `this` 指向 vm
					alert(this.message)
				},
				say: function(msg) {
					alert(msg)
				}
			}
		})
	</script>
</html>
```
#### v-bind和v-on的缩写
Vue.js为最常用的两个指令v-bind和v-on提供了缩写方式。v-bind指令可以缩写为一个冒号，v-on指令可以缩写为@符号。

```
<!--完整语法-->
<a href="javascripit:void(0)" v-bind:class="activeNumber === n + 1 ? 'active' : ''">{{ n + 1 }}</a>
<!--缩写语法-->
<a href="javascripit:void(0)" :class="activeNumber=== n + 1 ? 'active' : ''">{{ n + 1 }}</a>

<!--完整语法-->
<button v-on:click="greet">Greet</button>
<!--缩写语法-->
<button @click="greet">Greet</button>
```

