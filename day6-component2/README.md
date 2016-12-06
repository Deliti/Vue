#Vue学习之路 -- 组件开发下篇
----
之前学习的组件开发是注册，模板，v-bind和props这些数据传递。这几天学习了后面的内容，也整理了一下。<br>
这篇总结主要内容有：<br>
* [vue组件作用域]()
* [vue组件通信]()
* [Slot分发内容]()
* [动态组件]()
* [动手写个组件]()

那我们就顺着这条线学习下去<br>
#[vue组件作用域]()
接着上回说到，我们可以有个组件，组件中还可以有子组件。那么他们的作用域是什么样的呢？
#[vue组件通信]()
由于vue的作用域都是独立的，每个组件的内容都是在其独自的作用域中编译的，我们希望子组件可以获得父组件的状态，根组件可以获得孙组件的数据。我们之前有用到props+v-bind显式的得到父组件的数据，那其他情况该如何去处理。<br>
这就用到了$parent,$children,$root这三个东东，比如在这样一个情景下：
```
HTML
	<div id="app">
		<parent-comp></parent-comp>
	</div>
	<template id="parent-comp">
		<child-comp></child-comp>
	</template>
	<template id="child-comp">
		<div>haha</div>
	</template>
```
对于parent-comp来说，app这个实例就是它的父组件，this.$parent就是这个；那child-comp就是其子组件，this.$children就是指子组件，但是这里要注意，this.$children是一个数组，得到的是每个注册过的子组件。对于child-comp，app实例就是根组件，this.$root就是指这个app实例。
想要数据的话，可以直接this.parent.a这样的方式去获取，但是vue还是不推荐这种，要用props显式调用，但是可以this.children[0].a这样使用子组件的数据。
[v-ref指令]()
当组件个数较多时，我们难以记住各个组件的顺序和位置，通过序号访问子组件不是很方便。在子组件上使用v-ref指令，可以给子组件指定一个索引ID。like this:
```
HTML
	<template id="parent-comp">
		<child-comp1 v-ref:cc1></child-comp1>
		<child-comp2 v-ref:cc2 :my-name='name' :my-age='age'></child-comp2>
	</template>
```
调用的话可以这样：
```
JS
	//父组件中
	methods:{
		showChildData:function(){
			console.log(this.$refs.cc1.msg)
		}
	}
```
[自定义事件]()
除了data之外，组件之间的通信更多是在与自定义事件中。有$on,$emit,$dispatch,$broadcast。
具体例子直接传送门，就不在这里详说了，直接说一下几点注意的：
+ 事件通过v-on绑定，这绑定的是methods当中的方法，但是methods的方法不是我们要得到触发的事件，比如在showChildData中，this.$broadcast('showData'),这时，就会沿着节点找到子节点，子组件中的event会有个对应的showData方法，执行该方法；
+ 向上派发和向下广播有所区别：向上派发会触发自身同名事件，而向下广播不会；
+ 向上派发和向下广播默认只会触发直系（子或者父，不包括祖先和孙）的事件，除非事件返回值为true，才会继续在这一条线上继续；
+ 事件不能显式的通过 this.事件名 来调用它。就算自己调用自己的方法，也是this.emit(方法名)去调用 