Observable 是什么

定义：`Vue.observable`，让一个对象<strong style="color:#DD5145">变成响应式数据</strong>。`Vue` 内部会用它来处理 `data` 函数返回的对象

```javascript
Vue.observable({ count : 1})
//等同于
new vue({ count : 1})
```

Vue常用的修饰符

#### 表单修饰符

- lazy

定义：在我们填完信息，光标离开标签的时候，才会将值赋予给`value`，也就是在`change`事件之后再进行信息同步

```js
<input type="text" v-model.lazy="value">
<p>{{value}}</p>
```

- trim

定义：自动过滤用户输入的首空格字符，而中间的空格不会过滤

```js
<input type="text" v-model.trim="value">
```

- number

定义：自动将用户的输入值转为数值类型，但如果这个值无法被`parseFloat`解析，则会返回原来的值

```js
<input v-model.number="age" type="number">
```

#### 事件修饰符

- stop

定义：阻止了事件冒泡，相当于调用了`event.stopPropagation`方法

```js
<div @click="shout(2)">
  <button @click.stop="shout(1)">ok</button>
</div>
//只输出1
```

**prevent**

定义：阻止了事件的默认行为，相当于调用了`event.preventDefault`方法

```js
<form v-on:submit.prevent="onSubmit"></form>
```

**self**

定义：只当在 `event.target` 是当前元素自身时触发处理函数

```
<div v-on:click.self="doThat">...</div>
```

注意：使用修饰符时，顺序很重要；相应的代码会以同样的顺序产生。因此，用 `v-on:click.prevent.self` 会阻止**所有的点击**，而 `v-on:click.self.prevent` 只会阻止对元素自身的点击

**once**

定义：绑定了事件以后只能触发一次，第二次就不会触发

```js
<button @click.once="shout(1)">ok</button>
```

**capture**

定义：使事件触发从包含这个元素的顶层开始往下触发

```js
<div @click.capture="shout(1)">
   obj1
	<div @click.capture="shout(2)">
    obj2
		<div @click="shout(3)">
    	obj3
			<div @click="shout(4)">
    		obj4
			</div>
		</div>
	</div>
</div>
// 输出结构: 1 2 4 3 
```

**passive**

定义：在移动端，当我们在监听元素滚动事件的时候，会一直触发`onscroll`事件会让我们的网页变卡，因此我们使用这个修饰符的时候，相当于给`onscroll`事件整了一个`.lazy`修饰符

```js
<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
<!-- 而不会等待 `onScroll` 完成  -->
<!-- 这其中包含 `event.preventDefault()` 的情况 -->
<div v-on:scroll.passive="onScroll">...</div>
```

**注意：**不要把 `.passive` 和 `.prevent` 一起使用,因为 `.prevent` 将会被忽略，同时浏览器可能会向你展示一个警告。`passive` 会告诉浏览器你不想阻止事件的默认行为

**native**

定义：让组件变成像`html`内置标签那样监听根元素的原生事件，否则组件上使用 `v-on` 只会监听自定义事件

```js
<my-component v-on:click.native="doSomething"></my-component>
```

**注意：**使用.native修饰符来操作普通HTML标签是会令事件失效的

#### 鼠标按键修饰符

- left 左键点击
- right 右键点击
- middle 中键点击

```js
<button @click.left="shout(1)">ok</button>
<button @click.right="shout(1)">ok</button>
<button @click.middle="shout(1)">ok</button>
```

#### 键值修饰符

- 普通键（enter、tab、delete、space、esc、up...）
- 系统修饰键（ctrl、alt、meta、shift...）

#### v-bind修饰符

**async**

定义：能对`props`进行一个双向绑定

```js
//父组件
<comp :myMessage.sync="bar"></comp> 
//子组件
this.$emit('update:myMessage',params);

//以上这种方法相当于以下的简写

//父亲组件
<comp :myMessage="bar" @update:myMessage="func"></comp>
func(e){
 this.bar = e;
}
//子组件js
func2(){
  this.$emit('update:myMessage',params);
}
```

**注意：**

- 使用`sync`的时候，子组件传递的事件名格式必须为`update:value`，其中`value`必须与子组件中`props`中声明的名称完全一致
- 注意带有 `.sync` 修饰符的 `v-bind` 不能和表达式一起使用
- 将 `v-bind.sync` 用在一个字面量的对象上，例如 `v-bind.sync=”{ title: doc.title }”`，是无法正常工作的

**prop**

定义：设置自定义标签属性，避免暴露数据，防止污染HTML结构

```js
<input id="uid" title="title1" value="1" :index.prop="index">
```

**camel**

定义：将命名变为驼峰命名法，如将`view-Box`属性名转换为 `viewBox`

```js
<svg :viewBox="viewBox"></svg>
```

#### [#](https://vue3js.cn/interview/vue/404.html#面试官-vue项目本地开发完成后部署到服务器后报404是什么原因呢)vue项目部署到服务器后报404是什么原因？

##### 为什么history模式下有问题

`Vue`是属于单页应用（single-page application）

而`SPA`是一种网络应用程序或网站的模型，所有用户交互是通过动态重写当前页面，前面我们也看到了，不管我们应用有多少页面，构建物都只会产出一个`index.html`

```js
//nginx配置
server {
  listen  80;
  server_name  www.xxx.com;

  location / {
    index  /data/dist/index.html;
  }
}
```

可以根据 `nginx` 配置得出，当我们在地址栏输入 `www.xxx.com` 时，这时会打开我们 `dist` 目录下的 `index.html` 文件，然后我们在跳转路由进入到 `www.xxx.com/login`

关键在这里，当我们在 `website.com/login` 页执行刷新操作，`nginx location` 是没有相关配置的，所以就会出现 404 的情况

##### 为什么hash模式下没有问题

router hash` 模式我们都知道是用符号#表示的，如 `website.com/#/login`, `hash` 的值为 `#/login

它的特点在于：`hash` 虽然出现在 `URL` 中，但不会被包括在 `HTTP` 请求中，对服务端完全没有影响，因此改变 `hash` 不会重新加载页面

`hash` 模式下，仅 `hash` 符号之前的内容会被包含在请求中，如 `website.com/#/login` 只有 `website.com` 会被包含在请求中 ，因此对于服务端来说，即使没有配置`location`，也不会返回404错误

##### 解决方案

对`nginx`配置文件`.conf`修改，添加`try_files $uri $uri/ /index.html;`

```sh
server {
  listen  80;
  server_name  www.xxx.com;

  location / {
    index  /data/dist/index.html;
    try_files $uri $uri/ /index.html;
  }
}
```

**注意：**这么做以后，你的服务器就不再返回 404 错误页面，因为对于所有路径都会返回 `index.html` 文件。为了避免这种情况，你应该在 `Vue` 应用里面覆盖所有的路由情况，然后在给出一个 404 页面

```js
const router = new VueRouter({
  mode: 'history',
  routes: [
    { path: '*', component: NotFoundComponent }
  ]
})
```

