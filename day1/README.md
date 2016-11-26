#感受Vue的魅力
##[1.html](https://github.com/Deliti/Vue/blob/master/day1/1.html)
一个div#id='app'，对这个div进行数据填充。一般的做法是document.getElementById('app').innerHTML = data,对其进行dom操作，很麻烦，后来有了jquery，$('#app').html(data);那么对于专注数据处理的Vue来说，这是怎么处理的呢：
```html
<div id='app'>{{msg}}</div>  //HTML
```
```javascript
new Vue({
	el:'#app',
	data:{
		msg:'hello Vue.js'
	}
})
```
这样打开浏览器，显示就是 hello Vue.js 	<br>
貌似不用写获取节点，只要`new`一个`Vue`对象，传入el，data是el里面所涉及到的数据对象，好像就可以了，不着急，接着往下Go
<br>
##[2.html](https://github.com/Deliti/Vue/blob/master/day1/2.html)
初识Vue的时候，就知道在input中输入，直接显示在p标签中，正好可以再次感受一下：
```html
<div id="app">
	<p>
		{{msg}}
	</p>
	<input v-model='msg' type="text" name="" id="">
</div>
```
```javascript
new Vue({
	el:'#app',
	data:{
		msg:'hello vue.js'
	}
})
```
和1.html的差不多，就是在input中添加了一个v-model这个指令，做什么的不清楚，但是效果出来了。继续学
<br>
##[3.html](https://github.com/Deliti/Vue/blob/master/day1/3.html)
这次实现的是list渲染，这个很有用，项目中很常见的处理数据，生成一个list或者table，一般用jq写会涉及到很长的createElement,appendTo这样的操作，麻烦容易出错，还不容易维护。vue的写法是怎么用的：
```html
<div id="app">
	<ul>
		<li v-for='todo in todos'>
			{{todo.text}}
		</li>
	</ul>
</div>
```
```javascript
data:{
	todos:[
		{text:'learn javascript'},
		{text:'learn vue.js'},
		{text:'build something awesome'}
	]
}
```
这里可以看到，依然是data这个对象，但是`todos`是个数组，在html中有个v-for指令，和上面的v-modle很像，先不管，`todo in todos`，再结合{{todo.text}}，似乎有点
```
$.each(todos,function(i){
	var li = $('<li></li>');
	li.html(this.text);
	li.appendTo(ul);
})
```
这个感觉有点像。那么对于两者的代码量和代码管理上，vue好像更好，直接套用template，传入数据，直接生成，就没了一堆麻烦的dom操作，很带劲。
<br>
#[4.html](https://github.com/Deliti/Vue/blob/master/day1/4.html)
这个同样是对指令的感受，在button上加个v-on指令，vue对象当中传入method这个参数对象，似乎代替了click这样的事件绑定
##[5.html](https://github.com/Deliti/Vue/blob/master/day1/5.html)
最后是对上面的几种方法简单的综合。
<br>
初次学到这里我还是蛮迷糊的，好像还点了解vue了，但是还是迷迷糊糊，所以继续往后学习了之后再回来看第一天的代码，有种恍然开朗的feel。在接下来的日子，揭开vue的面纱，展现出她更加迷人的地方。

