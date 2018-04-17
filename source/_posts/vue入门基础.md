---
title: vue入门基础
date: 2017-01-06
categories:
- vue
comments: true
tag: vue
---


#### Hello World



```javascript
<body>

　　<!-- 在angularJS中用ng-model -->  <!-- {{mseeage?message:11}}支持三元表达式 -->

　　<!--{{*}} 可以 只绑定一次 视图变化时不进行改变 -->

　　<!--{{{}}} 绑定html标签 识别html标签 -->

<div class="app">
　　<input type="text" v-model='message'>

　　<!-- {{message}} -->

　　<!-- <br> {{message?message:1}} -->

<br> {{*message}} {{{message}}}
<!-- more -->
</div>
</body>
<script src="vue1.0.28.js"></script>
<script>
var vm = new Vue({ //实例 MVVM
　　　　　　el: ".app",
　　　　　　data: {
　　　　　　　　// message: "333"
　　　　　　message: "<h1>hello</h1>"
　　　　　　}
　　　})
</script>
```

#### vue实例
```javascript
<body>

<div id="app">{{message.age}}</div>

<script src="vue1.0.28.js"></script>
<script>
var mess = {
    age: 18
    }
var vm = new Vue({
        el: "#app",
　　　　　data: {
　　　　　　message: mess
　　　　　　}
　　　})
　　　　// vm.message.age = 100;
　　　　//我们当前实例vm和mess这个对象指向的是同一内存空间

　　　　//在实例创建后挂载以前不存在的属性是不会刷新视图的
console.log(vm.message == mess)
</script>
</body>
```
#### vue属性和方法
```javascript
<body>
<div id="app">
　　{{message}}
　　<input type="text" v-model='message'><br> {{age}}
</div>

<script src="vue1.0.28.js"></script>
<script>
var vm = new Vue({
　　　　el: "#app",
　　　　data: {
　　　　message: "aaaa"
　　　　}
})
//vm.$el 不能更改绑定数据的元素
console.log(vm.$el == document.getElementById("app"))

//vm.$data 就是data对应的这个对象
console.log(vm.$data);
// vm.$data = { age: 200 } //可以直接更改对象指定
// console.log(vm.$data);
//vm.$watch 监听数据变化
vm.$watch('message', function(newValu, oldVlue) {
　　　　console.log(newValu, oldVlue)
})
</script>
</body>
```
#### vue生命周期
```html
构建 new Vue（）实例
观察数据 （observe Data）
初始化方法 （Init Events）
没有问题，开始创建，调用 created 方法
没发现有el元素，看是否有vm.$mount（el）方法 （这个方法的作用是就是告诉我们初始化开始，可以挂载数据了）
发现el元素，看是否有模版，开始编译模版
若有模版就替换，若没有模版就直接插进去
编译完成
插入文档中，然后调用ready（）方法
调用vm.$destroy（）就销毁了
先进行销毁，然后把事件、属性、子组件 去掉
最后销毁掉，完全死亡

created 先实例化，在实例化后（检测el）
vm.$mount('#app') 手动挂载实例
beforeCompile 开始编译之前
compiled 编译完成
ready 插入文档后
vm.$destory 手动销毁实例
destroyed 销毁实例 
```
```javascript
<body>
<div id="app">{{message}}</div>
<script src="vue1.0.28.js"></script>
<script>
var vm = new Vue({
　　　　data: {
　　　　　　message: "sss"
　　},
　　　created() {
　　　　　　alert("1:创建实例")
　　},
　　　　beforeCompile() {
　　　　　　alert("2:编译前")
　　},
　　　　compiled() {
　　　　　　alert("3:编译完成")
　　},
　　　　ready() {
　　　　　　alert("4:准备好了")
　　},
　　　　beforeDestroy() {
　　　　　　alert("5:销毁前")
　　},
　　　　destroyed() {
　　　　　　alert("6:销毁了")
　　}
})
//手动挂载 el
vm.$mount("#app");
//销毁需要调用$destroy
vm.$destroy();
</script>
</body>
```
#### 属性的计算 
```javascript
<body>
<div id="app">
　　{{pages}}
　　<input type="text" v-model="page">
</div>
<script src="vue1.0.28.js"></script>
<script>
var vm = new Vue({
　　　　el: "#app",
　　　　data: {
　　　　　　page: 1,
　　　　　　num: 2
　　　},
　　　computed: {
　　　　　　pages: function() { //默认的方法是获取
　　　　　　//在vm中所有的this指向的都是当前实例
　　　return this.page * this.num
　　　　}
　　}
})
</script>
</body>
```
#### computed 的 set get 方法
```javascript
<body>
<div id="app">
　　{{pages}}
　　<input type="text" v-model="num">
</div>
<script src="vue1.0.28.js"></script>
<script>
var vm = new Vue({
　　　　el: "#app",
　　　　data: {
　　　　　　page: 1,
　　　　　　num: 2
　　　},
　　　　computed: {
　　　　　　pages: {
　　　　　　　　get() { //默认的方法是获取
　　　　　　//在vm中所有的this指向的都是当前实例
　　　　　　　　return this.page * this.num
　　　　　　},
　　　　set(val) {
　　　　　　//在设置方法去更改data中数据
　　　　　　this.num = val;
　　　　　　}
　　　　}
　　}
})
 
vm.pages = 9;
</script>
</body>
```
#### vue 解决加载标签时的闪烁问题
```javascript
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<meta http-equiv="X-UA-Compatible" content="ie=edge">
<title>Document</title>
<style>
[v-cloak] {
　　　　display: none;
}
</style>
</head>

<body>
<div id="app">
　　{{message}}
　　<!-- 在ng中 ng-bind可以解决单个标签闪烁问题 -->
　　<!-- v-text 只能解决单个闪烁问题 -->
　　<!-- 在ng中 ng-cloak 可以解决多级数据闪烁问题 -->
　　<!-- v-cloak 可以解决多级数据闪烁问题 -->
　　<div v-text="message"></div>
　　<div v-cloak>
　　{{message}}
　　</div>
</div>

<script src="vue1.0.28.js"></script>
<script>
var vm = new Vue({
　　　　el: "#app",
　　　　data: {
　　　　　　message: "涉及到极点拉萨警方立即as啊打算减肥阿道夫啊"
　　　　}
　　})
</script>
</body>
```
#### v-if v-else v-show template语法 以及 v-if 和 v-show的区别
```javascript
<body>
<div id="app">
　　<div v-if="false">
　　我是内容
　　</div>
　　<div v-show="false">
　　我是内容
　　</div>

　　<!-- template标签是不会被渲染的 解决无意义的空标签问题 template一般配合v-else使用 -->
　　<!-- 在 v-show上不支持template语法 -->
　　<template v-if="true">
　　<div>1</div>
　　<div>2</div>
　　</template>

　　<!-- v-else要跟在v-if后面，也可以跟在v-show后面 -->
　　<div v-else>
　　如果template中 v-if的条件不成立 就显示 v-else
　　</div>
</div>


<script src="vue1.0.28.js"></script>
<script>
var vm = new Vue({
　　　　el: "#app",
　　　　data: {
　　　　　　message: "涉及到极点拉萨警方立即as啊打算减肥阿道夫啊"
　　　　}
})
　　　　// v-show/v-if的区别
　　　　//v-show => ng-show 操作的是样式 （display:none） （一般作用于样式切换）
　　　　//v-if => ng-if 操作的是DOM （如果条件不成立 就移除dom了) （一开始知道存在与否，不会去切换的时候用）
　　　　//区别：v-if有更高的切换消耗 而v-show有更高的初始渲染消耗
</script>
</body>
```
#### 遍历
```javascript
<body>
　　<!-- 在angularjs中，用ng-repeat vue中用 v-for -->
<div id="app">
　　<template v-for=" (key,m) in datas">
　　　　{{$index}}-{{key}}:{{$key}},{{m}}<br/>
　　</template>
　　<!-- $index 表示当前第几个 （在vue中 $odd $even $fist $last 都没有 -->
</div>
<script src="https://cdn.bootcss.com/vue/1.0.28/vue.js"></script>
<script>
var vm = new Vue({
　　　　el:"#app",
　　　　data:{
　　　　　　datas:{
　　　　　　　　name:"test",
　　　　　　　　age:"12"
　　　　　　}
　　　　}
})
</script>
</body>
```
#### 遍历数组   （如果没有唯一的键供追踪，就要用track-by）
```javascript
<body>
<div id="app">
　　　　<!-- track-by 强制通过索引去遍历 -->
　　　　<!-- 如果在数组中取到的值为相同的值 那么需要用到track-by 防止报黄报错 -->
　　<ul v-for="(key,m) in datas" track-by="$index">
　　　　<li>{{key}}：{{$index}}-{{m.name}}:{{m.price}}</li>
　　　　<!-- 没有$key -->
　　</ul>
</div>
<script src="https://cdn.bootcss.com/vue/1.0.28/vue.js"></script>
<script>
var vm = new Vue({
　　　　　　el:"#app",
　　　　　　data:{
　　　　　　　　datas:[
　　　　　　　　　　{name:"苹果",price:"90"},
　　　　　　　　　　{name:"香蕉",price:"91"},
　　　　　　　　　　{name:"橘子",price:"92"}
　　　　　　　　]
　　　　　　}
　　})
</script>
</body>
```
#### 嵌套遍历
```javascript
<body>
<div id="app">
　　<div v-for="(key,mess) in datas">
　　　　<div>{{$index}}:
　　　　　　{{mess.name}}:{{mess.price}}
　　　　　　<div v-for="mess2 in mess.type">
　　　　　　　　<!-- 在父级写上索引 儿子可以拿到父亲的索引值 -->
　　　　　　　　{{key}}:{{mess2}}
　　　　　　</div>
　　　　</div>
　　</div>
</div>
<script src="https://cdn.bootcss.com/vue/1.0.28/vue.js"></script>
<script>
var vm = new Vue({
　　　　　　el:"#app",
　　　　　　data:{
　　　　　　　　datas:[
　　　　　　　　　　{name:"苹果",price:"90",type:["red","blue"]},
　　　　　　　　　　{name:"香蕉",price:"91",type:["black"]},
　　　　　　　　　　{name:"橘子",price:"92",type:["pink"]}
　　　　　　　　]
　　　　　　}
　　})
</script>
</body>
```
#### 动态绑定数据 （v-bind）
```javascript
<body>
<div id="app">
　　<!-- v-bind绑定动态属性 （把src绑定到v-bind上后，等待数据加载完成，才去渲染数据，不用{{}} ） -->
　　<!-- 可以直接使用：的方式直接进行绑定 动态去data上的值 -->
　　<!-- <img v-bind:src="imgUrl" alt="">
　　<a v-bind:href="href">{{href}}</a> -->
　　<img :src="imgUrl" alt="">
　　<a :href="href">{{href}}</a>
</div>
<script src="https://cdn.bootcss.com/vue/1.0.28/vue.js"></script>
<script>
var vm = new Vue({
　　　　el:"#app",
　　　　data:{
　　　　　　imgUrl:"https://blog.zachchan.com/wp-content/uploads/2017/12/201711071936012.png",
　　　　　　href:"https://blog.zachchan.com"
　　　　}
　　})
</script>
</body>
```
#### 绑定事件 （@,v-on:click 、methods）
```javascript
<body>
<div id="app">
　　　　<!-- v-on 绑定事件 ：事件名字 -->
　　　　<!-- v-on 执行方法的时候需要传递参数的时候添加(),
　　　　如果不需要传递参数就不要写了，如果需要传递参数，我们要手动传递事件源 -->
　　　　<!-- 我们可以通过@符号 取代 v-on: -->
 
　　　　<!-- <div v-on:click="add(1,$event)">hello Vue</div> -->
　　<div @click="add(1,$event)">hello Vue</div>
</div>
<script src="https://cdn.bootcss.com/vue/1.0.28/vue.js"></script>
<script>
var vm = new Vue({
　　　　el:"#app",
　　　　data:{
　　　　　　imgUrl:"https://blog.zachchan.com/wp-content/uploads/2017/12/201711071936012.png",
　　　　　　href:"https://blog.zachchan.com"
　　},
　　methods:{
　　　　　　//我们在vue中把方法都定义在methods对象中
　　　　add:function(num,e){
　　　　　　//e是事件源
　　　　　　console.log(num,e)
　　　　　　alert(100)
　　　　}
　　}
})
</script>
</body>
```





##### [GitHub代码仓库：https://github.com/zachchan/Learn/tree/master/vue/vue-1.0](https://github.com/zachchan/Learn/tree/master/vue/vue-1.0 "GitHub代码仓库")