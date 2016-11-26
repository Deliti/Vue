#Vue入门学习
<br>
#[构造器](https://github.com/Deliti/Vue/blob/master/day2/构造器.html)
每次进行Vue操作的时候，只需要构造一个Vue实例即可，文档上是这样说的：
* 1.每个vue.js应用的起步都是通过构造函数Vue创建的Vue的根实例
* 2.一个Vue实例就是MVVM模式中的viewModel，因此在文档中经常使用vm这个变量名
* 3.在实例化Vue时，需要传入一个选项对象，包含数据模版等等选项
* 4.可扩展Vue构造器，从而用预定义选项创建可复用的组件构造器

也就是：
```
var MyComponent = Vue.extend({ 
		//扩展选项
	})
```
所有的MyComponen实例都将以预定义的扩展选项被创建
	`var myComponentInstanse = new MyComponent();`

刚好公司有个项目需要写个组件，看了这里发现js都是殊途同归的，各种表现根还是不变，这种感觉很玄妙，很舒爽。过两天再分享插件的开发，以及尝试将那个基于Jq的组件转换为Vue组件。^_^


#[属性和方法](https://github.com/Deliti/Vue/blob/master/day2/属性和方法.html)
Vue的双向数据绑定很出名，与AngularJS很相似。在之前感受Vue的时候，数据绑定这块很有魅力，今天这部分就是来探索一下。
* 代码部分
```javascript
var data = {a:1};
var vm = new Vue({
	data:data
})
console.log(vm.a === data.a); //true
```
data这个对象指向了{a:1},然后vm里面的data指向了这个data。我们打印发现vm.a === data.a,接下来是这样
```
vm.a = 3;
console.log(data.a); //3
```
我们让vm.a的值变为3，发现data.a也变成了3；反之依然。
<br>
###这里我想到了一种假设的实现方式
```
var aa = {
	a:3
};
var bb = aa;
aa = bb;
```
这样的话，不管是aa的值改变，还是bb的值改变，都会反映到对方。
* 接上
只有被代理的属性是响应的，也就是说，在new这个Vue对象的时候，data的属性方法是什么样的，这个实例就同这时的data双向绑定了。但是，如果在这个实例添加了新的属性，就不会改变视图。
```
data.b = 4;
console.log(vm.b);  //undefined

vm.c = 5;
console.log(data.c);  //undefined
```
* 实例属性和方法
除了代理属性，还有一些实例属性和方法，前缀为$，以便与代理的数据属性区分
```
<div id="example"></div>
<br>
var data1 = {a:1};
var vm1 = new Vue({
	el:'#example',
	data:data1
})
console.log(vm1.$data === data1);  //true
console.log(vm1.$el === document.getElementById('example')); //true
```
也就是vm对象参数中的属性就是这个实例的实例属性。这个和上面的代理属性不一样在于：vm.a对应的不是vm的实例属性，而是vm.$data这个实例属性的a --> `vm.$data.a`。这样就能把这两者区分开了。
* $watch方法
jquery中，有事件绑定这个功能，如$(a).on('click',listener)这种模式，Vue对数据绑定也有这样类似的监听事件。
```
vm1.$watch('a',function(newVal,oldVal){
	//这个回调函数在vm1.a 改变后调用
	console.log('wcao');
})
data1.a = 13;  //控制台输出wcao
```
这个事件是这样去读的：`vm1观察vm1.a这个代理属性的值，如果发生改变，执行回调函数。`



