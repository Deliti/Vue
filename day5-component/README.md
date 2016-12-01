#Vue学习之路 -- 组件开发
----
因为上次开发字典下拉组件，基于jQuery开发，在页面展示那一块很麻烦，所以就想着使用Vue开发相同功能的组件，解放一下自己，毕竟我很懒。。<br>
下面就是我学习Vue的组件开发之路
#[起手式](step1-regi.html)
组件在Vue中是一个非常重要的模块，就如同jQuery
一样，每一个功能模块其实就是Vue的扩展，然后新建的Vue实例。对于Vue来说，它的组件不仅仅只有js，还有HTML和CSS，就相当于一个完整的部分页面。<br>
###`how to`
+ 1.创建一个构造器
```
var MyComponent = Vue.extend({
	template:'<div>我是一个组件</div>'
})
```
注：[构造器]()这个部分也说了，vue的组件扩展是Vue.extend,创建一个组件的核心部分。template属性是模板，意思是这个组件的HTML结构就是这个模板结构。
+ 2.注册和使用
注册分两种，一种是全局注册，注册之后任意Vue实例下都可以使用；另一种是局部注册，只有在注册的Vue实例下才可使用。
`全局注册`
```
	Vue.component('my-component',MyComponent);
```
注：全局注册是Vue.component()，两个参数，第一个是注册的名字，my-component，说白了就是HTML标签，我们给它自定义的HTML标签名；第二个就是我们的构造器，合起来读：我们这个构造器现在叫my-component了。
`全局使用`
```
HTML
	<div id='exp'>
		<my-component></my-component>
	</div>

JS
	new Vue({
		el:'#exp'
	})
```
然后解析之后就是
```
	<div id='exp'>
		<div>我是一个组件</div>
	</div>
```
当然啦，有的同学可能会误会，这不就是把div换成我们给的html标签名吗。。不是的，这里只是演示template的用法，如果我们把template改为：
```
template：'<div><span>嘿嘿嘿</span><ul><li>哈哈哈</li>
</ul></div>'
```
那么就会被解析为：
```
	<div id='exp'>
		<div>
			<span>嘿嘿嘿</span>
			<ul>
				<li>哈哈哈</li>
			</ul>
		</div>
	</div>
```
`局部注册`
```
	var Child = Vue.extend({
		template:'<span>局部注册</span>'
	})
	var Parent = Vue.extend({
		template:'<div>我是父组件<childcom></childcom></div>',
		components:{
			'childcom':Child
		}
	})
```
注：这里呢，和全局注册不同，比如Child这个构造器，我们只想让它被Parent这个组件中使用，`想在哪里使用就在那里的构造器或者Vue实例中加个components属性`，在这个components属性中注册。这样我们就能在这个Parent中使用。
```
	<div id='exp2'>
		<parentcom></parentcom>
	</div>
```
<br>
####这里还是有很大的区别的，很容易犯错
>比如这样使用
```
	<div id='exp2'>
		<parent>
			<childcom></childcom>
		</parent>
	</div>
```
parent作写在为父组件，被注册之后可以放在HTML中使用，但是，childcom作为parent的子标签的形式在父组件中使用，为什么无效：
>
	因为当子组件注册到父组件时，Vue.js会编译好父组件的模板，模板的内容已经决定了父组件要渲染的HTML
	<parent-component></parent-component>相对于运行时，它的一些子标签只会被当做普通的HTML执行。<child-component></child-component>不是标准的HTML标签，会被浏览器直接忽视掉

>再比如这样使用
```
	<div id="exp2">
		<parent></parent>
		<childcom></childcom>
	</div>
OR
	<div id='exp3'>
		
	</div>
	<parent></parent>
```
都是无效的。。。因为组件是必须被挂在Vue实例下，否则是不会生效的！

上面这个是在父组件中有个子组件，还有就是在实例中加个局部组件
```
HTML
	<div id="exp4">
		<one-com></one-com>
	</div>

JS
	var oneCom = Vue.extend({
		template:'<div>我是组件</div>'
	})
	var vm = new Vue({
		el:'#exp4',
		components:{
			'one-com':oneCom
		}
	})
	
```
这样的话，oneCom这个构造器的组件只能在exp4这个Vue实例下使用，而且如同全局注册般使用。

#[动态 -- 模板](step2-script&complate.html)
先说一下[语法糖](step3-sugar.html)。。。<br>
关于构造器，Vue规定的是不需要每次都先创建一个构造器，再注册。可以直接
```
	Vue.component('my-comp',{
		template:'<div>haha</div>'
	})
```
这样注册，Vue后台会帮我们处理Vue.extend的。

ok。接下来是模板进阶
<br>
如果每次都用字符串拼接的方式去创建template。。握草，要他干嘛。。这只是一点，还有几个问题，Vue的模板是DOM模板，如果是自定义的那种，`<my-com></my-com>`这种就会有一些限制，比如不能`<template><option value="">...</option></template>`，这种解析粗来就是`<option value="">...</option>`;还有不能再ul，select这样的对内在标签有限制的标签内，就算放进来也会被提到元素的外面，从而导致解析不正确<br>
那对于自定义元素来说，应当使用is特性
```
	<table>
		<tr is='my-component'></tr>
	</table>

```
所以，其实一般使用template都是写个template这个html，里面就是我们要写的内容，然后用id找到这个template即可。一般有两种方式<br>
`answer1` 使用script标签
```
HTML
	<div id="app1">
		<my-comp></my-comp>
	</div>
	<script type='text/x-template' id="myComponent2">
		<div>This is a Component!</div>
	</script>

JS
	new Vue({
		el:'#app1',
		components:{
			'my-comp':{
				template:'#myComponent2'
			}
		}
	})
```
使用script标签作为模板，需要注意两点：<br>
一要有个id，然后在组件的template中直接写id名就可以了，Vue会根据这个id找到对应的元素，然后将这个元素内的 HTML作为模板进行编译；<br>二是要将script的标签的type属性改为`text/x-template`，意在告诉浏览器这不是一段js脚本，浏览器在解析HTML文档时会忽略`script`标签中定义的内容。<br>

那如果是使用template这个标签作为模板，就不需要指定type属性。其他都一样。在IE中，是不认识template这个标签的，会直接显示，需要先隐藏。<br>

那为什么要使用这两个标签呢。用div不行吗？功能上据我测试时没有问题的，但是这样的话，div会被浏览器解析，直接显示出来。所以使用template或者type为x-template的script标签作为模板 <br>

而且，在Vue中，可创建.vue后缀的文件，可在.Vue文件中定义组件


#[动态 -- Attr](https://github.com/Deliti/Vue/blob/master/day4/class%26style.html)
这个部分我在之前的学习中说过了。这里简要提一下在组件中v-bind的使用。我们看一下下面的小栗子
```
HTML
	<div id="app">
		<test-comp :test-name='name' :test-age:'age'></test-comp>
	</div>
	<template id="test">
		<table>
			<tr>
				<th>信息展示</th>
			</tr>
			<tr>
				<td>姓名</td>
				<td>{{testName}}</td>
			</tr>
			<tr>
				<td>年龄</td>
				<td>{{testAge}}</td>
			</tr>
		</table>
	</template>

JS
	var vm = new Vue({
		el:'#app',
		data:{
			name:'dlt',
			age:18
		},
		components:{
			'test-comp':{
				template:'#test',
				props:['testName','testAge']
			}
		}
	})
```
这段代码是什么意思呢。。一个Vue实例下面有个名为test-comp的组件，这个实例的data为一个对象，有name和age，到这里都是我们玩过的。然后在这个组件上加了v-bind，这里和上次的一样，就是给这个组件加两个`attribute`，一个叫`test-name`，一个叫`test-age`，那这个`test-name='name'`取得值就是vm.name。在JS中的props是什么意思呢。。这个待会再说，可以先理解为刚刚设置的属性名。运行代码就可以得到一个表格，为data的信息展示表格。所以在组件中，动态的attr包括class，style，自定义的属性名都可以用v-bind设置。下面我们来看一下props的含义。

#[动态 -- props](step4-props.html)
组件实例的作用域是孤立的，这意味着不能并且不应该在子组件的模板内直接引用父组件的数据。这是可以使用props把数据传给子组件`prop`是组件数据的一个字段，期望从父组件传下来，子组件需要显式调用`props`选项声明props 。。。摘自文档<br>
so，props的值就是我们刚刚设置的自定义attr，为什么要用v-bind设置attr的意义也不言而喻了，将数据从父组件中显式传给子组件。
>注意点：
1.在子组件中定义prop时，使用camelCase命名法，由于HTML特性不区分大小写，camelCase的prop用于特性时，需要转换为kabab-case。例如在prop中定义的myName，在用作特性时需要转换为my-name。
2.在子组件中定义了props，如props:['name','age']，在组件中就可以直接使用{{name}}，{{age}}。所以props就相当于data。


我们再来看一个[props完整例子](step4-props-par&chd.html)
实现的功能是两个table，数据同步，第一个tabel是Vue实例下的原table，第二个table就是我们做的组件。有以下体会：<br>
>prop默认为单向绑定：当父组件的属性变化时，将传导给子组件，但是反过来不会。这是防止子组件无意修改了父组件的状态。也可以使用.sync双向绑定，或.once一次绑定。但是如果绑定的状态是数组或对象，不管是父组件还是子组件修改都会修改，因为是引用传递。

#[el和data不同之处](step5-el&data.html)
传入Vue构造器的多数选项也是可以用在Vue.extend()或者Vue.component()中，不过有两个特例：`data和el`，Vue.js规定：在定义组件的选项时，data和el选项必须使用函数。
这样：
```
	Vue.component('my-com',{
		data:function(){
			return {a:1}
		},
		el:function(){
			return '#app'
		}
	})
```
如果在定义组件选项时，这样写就会提出一个错误：
```
	var datas = {
		a:1
	}
	Vue.component('my-com',{
		data:datas
	})
```
[更多请参考这里](https://github.com/Deliti/Vue/tree/master/day4)


Vue组件开发的工具我们差不多了解了，从起手式到模板，动态的attr参数，动态的数据。之后几天再学习剩下的内容，嘿嘿偷个懒啦~
[完整演示](demo.html)

>`filterBy filterKey`是Vue自带的过滤器




