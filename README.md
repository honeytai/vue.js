# vue.js
learning process in vue.js
基本概念：
实例 new Vue({})
挂载点 el内容
模板 template： 

v-test v-html的区别：是否转义成为一个字符串
v-on:click=" " 函数方法在实例的methods中添加 也可以简写成@click=“”
v-bind:title="title" 属性绑定 title指的是实例中的title 也可以省略v-bind
单向绑定和双向绑定：单向：数据可以决定页面的内容 双向则是页面也可以影响数据，使用v-model

计算属性：放置在computed中，只有依赖的属性发生了变化才会做计算
侦听器：监听某个数据的变化，放置在watch中

v-if v-for v-show
v-show：div标签display：none；但是不会销毁标签
v-if：会直接把div标签去掉
v-for：<li v-for="(item, index) of list" :key="index">{{item}}</li> 添加一个key值是为了更高效的渲染循环，
里面的内容不要一样，可以使用index，但是对列表进行操作的时候会有问题

组件：
Vue.component('todo-item',{
			props: ['content'],
			template: '<li>{{content}}</li>'
		})
<todo-item
				v-for='(item,index) of list'
				:key="index"
				:content="item"
			>
			</todo-item>
全局组件，需要传入一个父内容

局部组件 var todoList={
template: '<li>item</li>'
}
同时还要在实例中添加components属性：
components: {'todo-List': todoList}

vue组件也是一个小的vue实例，所以也可以添加方法，对于一个根实例没有模板，所以会找挂载点
