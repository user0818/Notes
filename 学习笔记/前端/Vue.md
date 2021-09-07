# 第一个Vue程序

vue是一套用于构建用户界面的**渐进式框架**。这是一款响应式框架，也就意味着，当数据改变时，框架会帮我们自动更新视图。

- model层：主要负责存储数据
- view层：主要负责展示视图
- ViewModel层（核心）：负责转换Model中的数据对象来让数据变得更加容易管理和使用

有趣的是，当model层的数据改变时，vue会立即自动地把所有涉及到该数据的位置进行更新。例如：

```html
<div id="app">
  {{ message }}
</div>

<script>
    //创建一个vue实例，他就是ViewModel的代理对象
    var app = new Vue({
      el: '#app',	 //绑定元素
      data: {
        message: 'Hello Vue!'	//声明数据
      }
    })
</script>

>app.message='hello world!'	//div中的数据会变成:hello world!
```

这是因为vue将Dom和数据进行了绑定。

新建Vue实例时，可以传入一个option对象。这个对象中的可选的参数详见：https://cn.vuejs.org/v2/api/#%E9%80%89%E9%A1%B9-%E6%95%B0%E6%8D%AE

## MVVM模型

![image-20210319162624221](https://gitee.com/mw515031/image/raw/master/image/image-20210319162624221.png)

==数据的双向绑定：==

**单向绑定**：把Model绑定到View，当我们用JavaScript代码更新Model时，View就会自动更新。因此，我们不需要进行额外的DOM操作，只需要进行Model的操作就可以实现视图的联动更新。

**双向绑定**：把Model绑定到View的同时也将View绑定到Model上，这样就既可以通过更新Model来实现View的自动更新，也可以通过更新View来实现Model数据的更新。所以，当我们用JavaScript代码更新Model时，View就会自动更新，反之，如果用户更新了View，Model的数据也自动被更新了。

Vue.js主要有三种数据绑定形式，并且都是基于单向绑定和双向绑定。

### 单向绑定

#### 插值形式

从形式上可以看做model主动去绑定了dom，用于操作文本

插值形式就是{{data}}的形式，它使用的是**单向绑定**。

我们首先定义好一个JavaScript对象作为Model，并且把这个Model的两个属性绑定到DOM节点上，如下：

```html
<body>

<div id="vm">
<p>Hello, {{name}}!</p>
<p>You are {{age}} years old!</p>
</div>

</body>
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script type="text/javascript">
    var vm = new Vue({
        el: '#vm',
        data: {
            name: 'DroidMind',
            age: 18
        }
    });
</script>
```

Vue实例就是ViewModel的代理对象：

el: 指定了要把Model绑定到id为vm的DOM节点上。

data: 指定了Model，我们初始化了Model的两个属性name和age，在View内部的<p>节点上，可以直接用{{ name }}引用Model的某个属性。

当我们创建一个Vue实例，Vue可以自动把Model的状态映射到el指定的View上，并且实现绑定，这样我们就可以通过Model的操作来实现对DOM的联动更新，例如，打开浏览器console，在控制台输入vm.name = 'Bob'，执行上述代码，可以观察到页面立刻发生了变化，原来的Hello, Robot!自动变成了Hello, Bob!。Vue作为MVVM框架会自动监听Model的任何变化，在Model数据变化时，更新View的显示。这种Model到View的绑定就是单向绑定。

![image-20210319181144036](https://gitee.com/mw515031/image/raw/master/image/image-20210319181144036.png)

#### v-bind形式

和值插式类似，不过这种方式是操作属性值。可以看做dom主动绑定了mode。

如果我们希望html的某些属性能够支持单向绑定，我们只需要在该属性前面加上v-bind:指令，这样Vue在解析的时候会识别出该指令，就会将该将其属性的值跟Vue实例的Model进行绑定。这样我们就可以通过Model来动态的操作该属性从而实现DOM的联动更新。例如：

`<p class="classed">`

上面`<p>`节点的class样式指定的值为classed，它是一个静态的属性值，如果我们希望将该属性值跟Model进行一个绑定，只需要加上一个v-bind:指令，如下所示：

`<p v-bind:class="classed">`

绑定之后，classed不再是一个静态的字符串，而是vue实例中的data.classed变量，也就是它跟Model的classed进行了绑定，所以我们可以通过操作Model的classed来实现对View的class属性的动态更新，从而实现View的动态更新。

```html
<body>

<div id="vm">

    <p v-bind:class="classed">Hello, {{name}}!</p>

</div>

</body>
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script type="text/javascript">
    var vm = new Vue({
        el: '#vm',
        data: {
            name: 'DroidMind',
            classed: 'red'
        }
    });
</script>
<style>
    .red {
        background: red;
    }
    .blue {
        background: blue;
    }
</style>
```

如上面代码所示，vm.classed的初始值为red，此时`<p>`的样式属性对应的是.red，此时背景就为红色，我们可以通过在浏览器的控制台输入`vm.classed='bule'`，此时背景就自动变成了蓝色。可以看到通过对class属性进行绑定我们就可以动态的改变class对应的样式，这个都是通过Model的操作完成的，没有设置任何的DOM操作。

![image-20210319183423974](https://gitee.com/mw515031/image/raw/master/image/image-20210319183423974.png)

### 双向绑定

#### v-model形式

数据的双向绑定是vue实现的一大功能。

使用v-model指令，实现视图和数据的双向绑定。

所谓双向绑定，指的是vue实例中的data与其渲染的DOM元素的内容保持一致，无论谁被改变，另一方会相应的更新为相同的数据。这是通过设置属性访问器实现的。

v-model主要用在表单的input输入框，完成视图和数据的双向绑定。

v-model只能用在"input、select、textarea"这些表单元素上。

==双向绑定的缺点==：不知道data什么时候变了，也不知道是谁变了，变化后也不会通知，当然可以watch来监听data的变化，但这复杂，还不如单向绑定。

v-model主要是用在表单元素中，实现了双向绑定。当用户填写表单时，View的状态就被更新了，如果此时Model的数据也会随着输入的数据动态的更新，那就相当于我们把Model和View做了双向绑定。

model绑定的是value。 

```html
<body>

<form id="vm" action="#">

    <p><input v-model="email"></p>

    <p><input v-model="name"></p>

</form>
</body>

<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<script type="text/javascript">
    var vm = new Vue({
        el: '#vm',
        data: {
            email: '',
            name: ''
        }
    });
</script>
```

我们可以在表单中输入内容，然后在浏览器console中用vm.$data查看Model的内容，也可以用vm.name查看Model的name属性，它的值和FORM表单对应的`<input>`是一致的。如果在浏览器console中用JavaScript更新Model，例如，执行`vm.name='Bob'`，表单对应的`<input>`内容就会立刻更新。可以看到通过v-model实现了表单数据和Model数据的双向绑定。

![image-20210319183708671](https://gitee.com/mw515031/image/raw/master/image/image-20210319183708671.png)

#### 举例

一个最能体现双向绑定的例子：

1. 绑定输入框

```html
<div id="app">
    <input v-model="message">{{message}}
</div>

<script>
    var app = new Vue({
        el: '#app',
        data: {
            message: 'hello'
        }
    });
</script>
```

输入框和后边的显示都绑定在了message上，当任意一个变化时，都会引起另一个的变化。

- 改变输入框
- app.message='xxxx'

2. 绑定单选框

```html
<div id="app">
    <input type="radio" name="sex" value="男" v-model="message">男
    <input type="radio" name="sex" value="女" v-model="message">女
    <br>{{message}}
</div>

<script>
    var app = new Vue({
        el: '#app',
        data: {
            message: ''
        }
    });
</script>
```

当选中不同时，展现的message不同

## Vue的生命周期

![13119812-5890a846b6efa045](https://gitee.com/mw515031/image/raw/master/image/13119812-5890a846b6efa045.jpg)

# 语法

## v-if

判断是否显示该标签

```html
<div id="app">
    
    <!--若ok值为true，显示该标签.这里的ok也可以替换为表达式-->
	<p v-if="ok">yes</p>
	<p v-else>no</p><!--前提是v-else和v-if之间不能有别的元素-->

</div>

<script>
    //创建一个vue对象
    var app = new Vue({
        el: '#app',	 //绑定元素
        data: {
            ok: true	//声明数据
        }
    });
</script>
```

## v-for

循环显示每个元素

```html
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
<script>
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: '学习 JavaScript' },
      { text: '学习 Vue' },
      { text: '整个牛项目' }
    ]
  }
})
</script>
```



## v-once

vue中，model变化会自动刷新到view中。但是有些场景下我们不想随着model层变化，只需要在标签中添加一个`v-once`即可实现

## v-on

如果要给一个标签绑定一个事件，可以使用`v-on:事件名`进行绑定。

`v-on:事件名`可以简写为`@事件名`

```html
<div id="app">
    <button v-on:click="sayHi">click</button>

</div>

<script>
    //创建一个vue对象
    var app = new Vue({
        el: '#app',	 //绑定元素
        data: {
            message:'hello'
        },
        methods: {
            //方法必须定义在methods对象中
            sayHi:function (){
                alert(this.message)
            }
        }

    });
</script>
```

v-on还可以带一些修饰符：

- `@click.stop`：停止冒泡

- `@click.prevent`：阻止默认事件（比如form表单中，点击提交后==跳转==就是默认事件）
- `@keyup.enter`：这里的enter代表按键的名字，当监听到特定按键（比如这里是enter键）就会触发。这里的按键名也可以替换为按键对应的代码
- `@click.once`：只触发一次

## v-bind

如果不想把属性写死在代码里，可以在属性名前添加`v-bind:`，这时属性值可以写作model中的变量。

`v-bind:`可以直接简写为`:`

```html
<body>
    <div id ="app"> 
        <a v-bind:href="url">百度一下</a>
        <a :href="url">百度一下</a>
    </div>

    <script>
        
     //创建Vue实例,得到 ViewModel
     var vm = new Vue({
        el: '#app',
        data: {
            url:'http://www.baidu.com'
        },
        methods: {

        }
     });
    </script>
</body>
```

属性值可以为以下形式

- 对象：比如`{String:boolean}`形式，当布尔值为真实显示String
- 数组：比如在`style`可以添加多个样式
- 函数：返回值为对象（这里要采用绑定加调用的方式，即方法后直接跟小括号）

## v-show

区别与`v-if`：

v-if：是否显示该标签

v-show：是否设置属性`display:none`

# 计算属性

## 什么是计算属性？

计算属性是在内存中实现的，所以速度快是一大特点，可以理解为缓存。

其主要作用是在展示数据前，对数据进行预处理。

计算属性和方法的区别：

1. 调用时

- 但是计算属性毕竟是属性，虽然他的定义的形式是一个方法，但是仍然用属性名调用
- 方法调用时需要加上参数（即使没有参数也得加上括号）

2. 是否每次都执行函数

- 方法每次执行都会产生计算。

- 计算属性在执行一次之后，会把结果以属性的形式存在内存中，以后的每次调用都会直接从内存中取到该值
  - 注意：当每次model域刷新时（比如model值变化），计算属性也会刷新。

```html
<div id="app">
{{currentTime1()}}<br>		<!--注意括号-->
{{currentTime2}}
</div>

<script>

    let vm = new Vue({
        el: '#app',
        data: {
            message:null
        },
        methods:{
            currentTime1: function (){
                return Date.now();
            }
        },
        computed:{
            currentTime2: function (){
                return Date.now();
            }
        }
    });
</script>
```

总结：

调用方法时，每次都需要进行计算，既然有计算就会产生系统开销。如果这个计算结果不是经常改变的，那么就可以把这个结果缓存起来，以节约系统开销。这就是计算属性的作用。

## get和set

上边计算属性的写法其实是简写形式。完整的计算属性其实就是一个对象，对象中有两个方法：get和set。

```javascript
computed: {
            name:{
                set: function(){},
                get: function(){}
            }
        }
```

但是一般情况下，属性都是只读的，所以不需要set方法，所以可以简写成以下形式。

```javascript
computed: {
            name: function(){}
        }
```

注：当检测到计算属性发生改变时（即给计算属性赋值时），会回调set方法。所赋的值可以作为set方法的参数，从而在该方法中改变某些属性值。

# 四、组件

组件，就是抽取出来的页面框架，就是平常网站中常见的导航啦，侧边栏等。

```html
<div id="app">

    <!--循环数据列表items，用item代表每次循环的值;把item和形参prop绑定在一起，也可以理解为把item的值传给该参数-->
    <newtag v-for="item in items" v-bind:prop="item"></newtag>
    <newtag v-bind:qin="message"></newtag>

</div>

<script>
    //创建一个组件，名字叫做newtag
    Vue.component("newtag", {
        props: ['prop'],						//可以理解为形参列表，用来传给下一行的模板
        template: '<li>{{ prop }}</li>'		//将所有组件按照这个模板渲染
    });

    let app = new Vue({
        el: '#app',
        data: {
            items: ['java', 'python', 'spark'],	//数据
            message:'hello111'
        }
    });
</script>
```

效果：

![image-20210320151536334](https://gitee.com/mw515031/image/raw/master/image/image-20210320151536334.png)

解释一下上述代码为：

把组件当成一个函数，形参就是props中声明的，而`v-bind`就相当于给这个函数穿了个参数。

==注意：==创建组件时，组件名不能定义为大写，实测不能识别。

# 五、网络通信

由于Vue是一款视图层框架，且尤雨溪严格遵守SoC，所以Vue中没有AJAX通信功能。官方推荐使用Axios框架解决通信问题。

```html
<div id="app">
{{message}}
</div>

<script>

    let vm = new Vue({
        el: '#app',
        data: {
            message:null
        },
        mounted(){
            //这里是请求的本地伪造的数据，业务中应该请求服务器。
            axios.get('data.json').then(response=>(this.message=response.data));
            						//链式调用中是一个lambda表达式，内容是把返回的数据部分绑定到model的message变量上
        }
    });
</script>
```

```json
{ "firstName":"Bill" , "lastName":"Gates" }
```

# 七、slot

乱七八糟，没整明白

# 八、自定义事件内容分发

乱七八糟，没整明白

# 九、vue-cli

接下来的部分就是如何使用Vue来真实搭建一个项目。包括打包部署等功能，需要的环境介绍如下：

- webpack：是一款打包工具，类似于maven，可以自动地将项目进行打包和部署。
- nodejs：JavaScript的运行环境
- npm：nodejs中的包管理和分发工具，可以理解为maven仓库，常用命令:
  - `npm install`：安装包
  - `npm run dev`：热部署
  - `npm set registry https://registry.npm.taobao.org/`:给npm永久设置淘宝镜像

## 初始化一个vue-cli项目

1. 进入想要新建项目的位置，`vue init webpack myvue`来初始化一个vue项目

![image-20210321145029048](https://gitee.com/mw515031/image/raw/master/image/image-20210321145029048.png)

2. 设置一些初始化项目的参数

   ![image-20210321145508115](https://gitee.com/mw515031/image/raw/master/image/image-20210321145508115.png)

   ==注意：Vue build选择第一个（standalone）会安装编译模式和运行模式==

3. 等待创建成功后项目就安装完毕，如果上一步最后一步没有选择yes，还要执行一次`npm install`

4. 执行命令`npm run dev`进行热部署，之后便能访问项目了

## 关于Vue的个人理解

1. 概述

vue是一款面向组件开发的前端框架，它把一个前端框架分抽取成了几个组件（相当于iframe），每个组件对应一个.vue文件，对于开发者而言，每个人只需要针对每个组件进行设计就行。而且组件之间还可以相互嵌套，这样大大的把模块之间的耦合给分解开。一个组件由`<template>`,`<script>` `<style>`三部分组成，分别负责模板，业务逻辑和样式三个部分，各司其职。

配置文件是以.js文件的形式存在，在配置文件中，用到哪个组件就要用`import 组件名 from '组件文件名去掉后缀'`，其中组件名有两种情况：

- 如果组件在node_modules中，直接写文件名去掉后缀
- 如果是自定义的的组件，要写从当前文件开始的相对路径

同理，如果写的组件要想被别人使用，要把该组件暴露出去给别人调用：

```javascript
export default {
  name: "组件名"
}
```

2. 结构

![image-20210321153313680](https://gitee.com/mw515031/image/raw/master/image/image-20210321153313680.png)

- component中放各种.vue组件
- router中放路由的配置文件
- App.vue：所有组件的集成处
- main.js：总的配置文件
- index.html：主页(由于所有组件集成在了App.vue中，因此该文件中，除了主要结构只需要在body中写一行引入App.vue的代码即可)

# 十、路由

## 如何实现路由

由于vue只关注视图，而且要想前后端分离的话，浏览器和服务器之间就只能有数据交换，就不能把页面跳转的功能交给服务器来执行，因此就需要另一个组件来实现页面跳转功能，也就是vue-router

==注意：vue提供的一整套的技术框架是基于spa的单页应用，如果想用他提供的一整套框架，比如webpack和vue-router，就必须做成单页应用。但是如果仅仅只是想用vue的功能进行开发（**比如前七章提供的功能**），可以和以前一样写静态html，通过script的src引入vue.js，然后还是写js一样在底部构建vue实例。==

路由说白了就是当发送不同的请求时，跳转到不同的组件。类似于springmvc中的controller。

使用步骤如下：

1. 安装vue-router

```sh
npm install vue-router
```

2. 在main.js中引入该文件并使用

```javascript
import Vue from 'vue'
import router from './router'	//这里指的是配置文件所在的位置
```

3. 配置路由映射，router/index.js

用到哪个组件，就要在开头引入哪个组件

```javascript
import Vue from 'vue'
import Router from 'vue-router'
import main from "../components/main";

Vue.use(Router)

export default new Router({
  mode:"history",			//路径中不显示'#'
  routes: [
    {
      path: '/main',		//配置监听的请求
      component: main,		//监听到该请求后返回到哪个组件
    }
  ]
})

```

4. 怎么发请求

```vue
<router-link to="/main"></router-link>
```

5. 配置在哪里展示`<router-view></router-view>`

vue是一款单页面的框架，假设所有的组件只在同一个html上显示，只不过根据情况显示的组件不同。App.vue是所有组件集成的地方，所以==（不考虑嵌套路由的情况下）组件都会展示到app.vue上==。因此要在app.vue上标明接受组件展示。

```vue
<template>
  <div id="app">
    <router-view></router-view>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>
```

6. 还要在main.js创建的实例中，传入路由。

接下来看一下main.js：

```javascript
import router from './router'

new Vue({
  el: '#app',
  router,
  render: h => h(App)
})
```

- 这里的`el`绑定的是html中的dom元素。
- router是从路由配置文件中拿到的组件

- 全局只有一个Vue实例。这个唯一的实例就是来渲染App组件的。所以App组件是一切的开始。

## 参数传递

方法一：路由参数取值

1. 发送请求

我们通常赢router-link发送请求，如果想带有参数的话，只需要进行稍微的改变。main.vue：

```vue
<router-link :to="{name:'info',params:{id:1}}"></router-link>
```

2. 接受请求

当接受请求时只需要在路径上绑定个同名参数就能接受。router/index.js：

```js
export default new Router({
  mode:"history",
  routes: [
        {
          path:'/user/info/:id',
          name:'info',
          component:info
        }
  ]
})
```

这参数会被保存到返回的组件中，组件可以调用该参数。

3. 调用参数

info.vue：`    {{$route.params.id}}`

方法二：绑定参数取值

1. 发送请求

同上

2. 接受请求

开启参数绑定

```js
export default new Router({
  mode:"history",
  routes: [
        {
          path:'/user/info/:id',
          name:'info',
          component:info,
          props:true
        }
  ]
})
```

3. 调用参数

```vue
<template>
  <div>
    <p>信息</p>
    {{id}}			//展示参数
  </div>
</template>

<script>
export default {
  name: "info",
  props:['id']		//开启接受参数
}
</script>
```

## 重定向

```js
{
    path:'/gohome',
    redirect:'/main'
}
```

## error页面

除了我们监听的请求外，其他所有都跳转404组件

```js
{
    path:'*',
   	component:notfound
}
```

## 路由钩子与异步请求

与AOP相似，我们可以定义一些钩子函数，在某些行为之前或者之后去进行操作。

比如在info组件中：

```vue
<template>
  <div>
    <p>信息</p>
  </div>
</template>

<script>
export default {
  name: "info",
  beforeRouteEnter:(to, from, next)=>{
    console.log('进入路由之前');
    next();
  },
  beforeRouteLeave:(to, from, next)=>{
    console.log('离开路由之前')
    next();
  }
}
</script>
```

当路由进入或者离开该组件时，执行相应函数。

注意，最后要执行`next()`方法放行，否则会卡在这里不能继续。

`next()`的四种方式：

- `next():`跳入下一页面
- `next('/path'):`改变路由的跳转方向
- `next(false):`返回原来的页面
-  `next(vm=>{}):`仅在beforeRouteEnter中可用

## 路由嵌套

上述我们提到了，当我们路由返回组件时，每个返回组件都是展示在App组件中展示。这样就形成了`html-->App-->returned component`的形式。有时候，我们并不想让自己请求的组件作为App组件的全部，而是其他组件的一部分。

比如一个文档网站，App组件已经渲染了侧边栏和顶部栏形成的主页，我们点击侧边栏的按钮发出新的请求时，得到的文章组件并不想让他作为App组件的全部（这样顶部栏和侧边栏就消失了，整个App组件下就只有这个文章组件），而是让他成为主页的一部分

![image-20210321193917759](https://gitee.com/mw515031/image/raw/master/image/image-20210321193917759.png)

1. 配置嵌套路由，router/index.js

因为子路由要显示在main组件下，所以要在在该组件下配置子路由

```js
import Vue from 'vue'
import Router from 'vue-router'
import main from "../components/main";

import list from "../components/user/list";
import info from "../components/user/info";

Vue.use(Router)

export default new Router({
  mode:"history",
  routes: [
    {
      path: '/main',
      component: main,
      children:[		//个人理解是，监听main组件里发出的请求，并返回响应组件
        {
          path:'/user/list',
          component:list
        },
        {
          path:'/user/info',
          component:info
        }
      ]
    }
  ]
})

```

2. 声明显示的位置,main.vue

在main.vue中要显示组件的位置上填上：

```vue
<router-view></router-view>
```

这样该组件中发的请求就会展示在此处。

模块化NB！

## Axios向服务器请+求数据

```vue
<template>
  <div>
    <p>信息</p>
    {{message}}
  </div>
</template>

<script>
export default {
  name: "info",
  data:function (){		//组件的data必须是一个函数，这样可以保证每个组件的data是独立的实例
    return {
      message:''
    }
  },
  beforeRouteEnter:(to, from, next)=>{		//在进入该路由前，请求数据
    next((vm)=>{
      vm.getdata()
    });
  },
  beforeRouteLeave:(to, from, next)=>{
    console.log('离开路由之前')
    next();
  },
  methods:{
    getdata:function (){				//定义一个请求数据的方法
      this.axios({
        method:'get',
        url:'/static/data.json'
      }).then(response=>(console.log(this.message=response.data)));
    }
  }
}
</script>
```

































































































