---
title: vue学习笔记
date: 2020-07-10 16:22:15
tags: vue
---

学习地址
cn.vuejs.org
```
var app = new Vue({
	//必须挂载一个根元素
	el:'#app', 
	//页面所要用到的属性定义在 data 对象中
	data:{
		message:'hello'
	},
	//页面要用到的方法定义在 methods 对象中
	methods:{
	
	}
})
```

1.插值表达式
```
{{ message }}
```

2.元素属性上的绑定
```
<span v-bind:title="message">绑定元素title属性</span>
```

3.控制元素显示
<p v-if="seen">能否看到这个元素</p>
v-if是懒加载模式,如果检测到 seen 为false,不会渲染此元素

4.循环显示列表
```
<ul>
	<li v-for="todo in todos"> {{todo.text}} </li> 或者 <li v-for=(todo,index) in todos> {{index}} - {{todo.text}} </li>
</ul>
```

5.绑定事件
```
<button v-on:click="bindEvent">绑定click事件</button>
```

6.双向绑定 
```
<input v-model="message" />
```

7.组件注册
```
Vue.component("todo-item",{
	template: '<li>todo-item组件</li>'
})
```

接收父作用域的值,传入的属性名称为 todo,在父组件使用 v-bind:todo = 'item',item由 v-for指令中的item得到,这样使用
```
<todo-item 
	v-for="(item,index) in todos"
	v-bind:todo="item"
	v-bind:key="item.id"
	v-on:move-item="moveItem">
</todo-item>

Vue.component("todo-item",{
	methods:{
		moveItem(){
			//$emit的第一个参数,是在父作用域指定的 v-on: 绑定的事件名称是一样的,第二个参数是给父作用域传值
			this.$emit('move-item',1);
		}
	},
	props:['todo'],
	template: '<li v-on:click="moveItem">{{ todo.text }}</li>'
})
```

api地址
cn.vuejs.org/v2/api

8.v-once 只渲染一次
```
<span v-once>{{message}}</span> 
```
这个标签里的插值表达式只会渲染一次
message再变动 span里面的值不会改变

9.v-html
```
<span v-html="rawHtml"></span>
```
会对rawHtml里面的html标签进行解释并运行

10.对html标签绑定属性,必须使用 v-bind 指令
```
<div v-bind:id="{{valId}}"></div>
```

11.绑定的属性值可以使用 javascript 表达式 

12.绑定动态属性,使用方括号属性
v-bind:[attrName]=
v-on:[eventName]=

13.v-bind缩写 
v-bind:href    缩写  :href
v-bind:[attrName]  缩写   :[attrName]

v-on缩写
v-on:click   缩写  @click
v-on:[eventName]  缩写  @[eventName]
后面接有方法或运算符则是直接运行
访问原始dom事件对象,$event

14.计算属性
可以实现和 methods:{} 方法同样的功能,但计算属性可以缓存
计算属性不需要在 data 中声明
和 data 同级声明  computed:{}
计算属性可以像 property 一样使用,跟某个属性绑定,属性变动,计算属性跟着变动
计算属性可以缓存,methods:{} 里面的方式不会缓存
在 created()方法中定义的方法可以不在methods中声明但可以被调用

15.侦听器
和 data 同级声明 , watch:{},在数据变化时执行异步或开销较大的操作时，这个方式是最有用的
侦听器使用的属性需要在data中事先声明

16.class和style属性绑定
使用 v-bind 就能实现,但普通的v-bind只能拼接字符串
:class和:style可以使用数组和对象 json字面量对象,data中的对象或计算属性
:class可以和普通class属性共存

17.v-if v-else v-else-if
多行的显示使用 <template v-if>
v-show 无论条件真假都会渲染
v-if有更高的切换开销
v-show有更高的初始渲染开销

18.事件修饰符
方法只有纯粹的数据逻辑，而不是去处理 DOM 事件细节

19.v-for遍历对象,第一个参数为value,第二个为name,第三个为index

20.使用 v-model 绑定表单元素
a.input和textarea 不使用 value
```
<input type="text" v-model="message">
<textarea v-model="message">
```

b.单个复选框,使用boolean绑定
```
<input type="checkbox" v-model="checked">
```
checked为true或false

多个复选框,绑定到数组
```
<input v-model="checkNames" value="1">
<input v-model="checkNames" value="2">.
<input v-model="checkNames" value="3">
```
checkNames为 [1,2,3]

c.单选按钮,绑定到value
```
<input type="radio" value="1" v-model="radioVal">
<input type="radio" value="2" v-model="radioVal">
```
radioVal为 1或2

d.单选选择框绑定 selected
```
<select v-model="selected">
<option disabled value="">请选择</option>
<option value="1">11</option>
<option value="2">22</option>
<option value="3">33</option>
```
selected为 1或2或3,取value,如果没有value,默认value就是text

多选时绑定到一个数组

option的value需用 v-bind:value来绑定

21.子组件data必须是一个函数

22.组件注册
Vue.component("",{option}) 全局组件注册
子组件通过props接收父域传过来的值
父域通过 v-bind:attrName 将值传递给子组件,可以传递对象

23.子组件向父域传递的值可以通过 $event访问到

24.向子组件不通过props传递文本,将父组件的 text 作为插槽插入子组件,在子组件加入 <slot></slot>

25.动态组件
将组件名称存入数组,使用 v-bind:is 属性绑定计算属性
点击按钮触发事件,改变计算属性为当前的组件名,实现动态组件

26.父子组件通过 props和 $emit来通讯,如果不是父子组件,通过 vuex来进行通讯
action 操作触发 state 数据源改变 view 视图改变,一个单向的状态管理
单个组件使用这个操作没有问题,如果是多个组件
a.多个组件中的操作都会改变数据源,必须是同一数据源,而不是副本
b.多个组件中的视图都依赖同一数据源改变

action 触发 mutations 状态(可跟踪调试) 改变 store 仓库中的state 数据源 view 视图更新
组件使用store中的数据，使用计算属性 count(){return store.state.count}
在根组件注入 store,子组件通过 this.$store可以获取到  this.$store.state.count

a.当一个组件需要获取多个状态的时候，
将这些状态都声明为计算属性会有些重复和冗余。
为了解决这个问题，我们可以使用 mapState 辅助函数帮助我们生成计算属性 import { mapState } from 'vuex'
computed:mapState({}) 这里传对象
当映射的计算属性的名称与 state 的子节点名称相同时，我们也可以给 mapState 传一个字符串数组
computed:mapState([]) 这里传数组

// 使用对象展开运算符将此对象混入到外部对象中
```
  ...mapState({
    // ...
  })
  ```
  
b.需要从 store的state中派生出一些状态,使用 getter,相当于store的计算属性,也会缓存,store为第一个参数
```
doDone:state=>{
	return state.todos.filter(todo => todo.done)
}
```
getter通过方法访问不会缓存,每次都会调用
mapGetter将 store 中的 getter 映射到局部计算属性,
使用对象解构符 ...mapGetter([]),或者 ...mapGetter({}) 这样可以将getter里的计算映射到局部另外一个名称的计算属性

c.更新store中状态的唯一方法提交 mutations
无参提交  state.commit("incment")
带参数提交 state.commit("incment",1)
对象提交 state.commit("incment",{amount:10})
type提交 state.commit({type:"incment",amount:10})
在组件中使用 this.$store.commit("incment")提交或者 使用 mapMutations 将组件中的methods映射为 store.commit调用
...mapMutations([])或 ...mapMutations({})
只能同步更新,在mutations中进行异步提交不能追踪状态

d.Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，因此你可以调用 context.commit 提交一个 mutation
在组件中使用 this.$store.dispatch('xxx') 分发 action，或者使用 mapActions 辅助函数将组件的 methods 映射为 store.dispatch 调用
async await 语法糖,async声明过的函数返回的是 promise,不必再写很多的then

e.使用单一状态树，应用的所有状态会集中到一个比较大的对象。当应用变得非常复杂时，store 对象就有可能变得相当臃肿
为了解决以上问题，Vuex 允许我们将 store 分割成模块（module）。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块