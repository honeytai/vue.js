# vue.js
learning process in vue.js
基本概念：
实例 new Vue({})
挂载点 el内容
模板 template： 

v-test v-html的区别：是否转义成为一个字符串 都是用作显示 类似插值表达式
v-on:click=" " 函数方法在实例的methods中添加 也可以简写成@click=“”
v-bind:title="title" 属性绑定 title指的是实例中的title 也可以省略v-bind
单向绑定和双向绑定：单向：数据可以决定页面的内容 双向则是页面也可以影响数据，使用v-model

计算属性：放置在computed中，只有依赖的属性发生了变化才会做计算,如果数据没有发生变化，就会利用缓存来显示，所以效率很高。调用方法将总会再次执行函数
侦听器：监听某个数据的变化，放置在watch中

v-if v-for v-show （v-for 具有比 v-if 更高的优先级）
v-if 是“真正”的条件渲染，因为它会确保在切换过程中条件块内的事件监听器和子组件适当地被销毁和重建。
v-if 也是惰性的：如果在初始渲染时条件为假，则什么也不做——直到条件第一次变为真时，才会开始渲染条件块。
相比之下，v-show 就简单得多——不管初始条件是什么，元素总是会被渲染，并且只是简单地基于 CSS 进行切换。
一般来说，v-if 有更高的切换开销，而 v-show 有更高的初始渲染开销。因此，如果需要非常频繁地切换，则使用 v-show 较好；如果在运行时条件很少改变，则使用 v-if 较好。
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

vue组件也是一个小的vue实例，所以也可以添加方法，对于一个根实例没有模板，所以会找挂载点。

子组件和父组件的通信： 发布订阅模式

vue.cli
template中只能有一个div，data数据也不是一个对象，而是一个函数，返回值为一个对象

Vue的双向数据绑定原理是什么？vue.js 是采用数据劫持结合发布者-订阅者模式的方式，通过Object.defineProperty()来劫持各个属性的setter，getter，在数据变动时发布消息给订阅者，触发相应的监听回调。
具体步骤：
第一步：需要observe的数据对象进行递归遍历，包括子属性对象的属性，都加上 setter和getter这样的话，给这个对象的某个值赋值，就会触发setter，那么就能监听到了数据变化
第二步：compile解析模板指令，将模板中的变量替换成数据，然后初始化渲染页面视图，并将每个指令对应的节点绑定更新函数，添加监听数据的订阅者，一旦数据有变动，收到通知，更新视图
第三步：Watcher订阅者是Observer和Compile之间通信的桥梁，主要做的事情是:1、在自身实例化时往属性订阅器(dep)里面添加自己2、自身必须有一个update()方法3、待属性变动dep.notice()通知时，能调用自身的update()方法，并触发Compile中绑定的回调，则功成身退。
第四步：MVVM作为数据绑定的入口，整合Observer、Compile和Watcher三者，通过Observer来监听自己的model数据变化，通过Compile来解析编译模板指令，最终利用Watcher搭起Observer和Compile之间的通信桥梁，达到数据变化 -> 视图更新；视图交互变化(input) -> 数据model变更的双向绑定效果。

vue-router有哪几种导航钩子？
三种，
第一种：是全局导航钩子：router.beforeEach(to,from,next)，作用：跳转前进行判断拦截。
第二种：组件内的钩子
第三种：单独路由独享组件
