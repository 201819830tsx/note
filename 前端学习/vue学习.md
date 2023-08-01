## **一、HbuildX运行vue项目**

1，打开Hbuilder创建一个新项目

2，模板选择Vue项目

3，等待一会项目就创建好了

4，右键单击选择npm run build

5，继续右键单击选择npm run serve

6，找底部出现的地址和端口号

7，在浏览器中输入即可访问vue页面了

使用nvm管理node版本

## 二、nvm下载及安装

**下载地址:**https://github.com/coreybutler/nvm-windows/releases

**安装使用**：https://blog.csdn.net/JJ_Smilewang/article/details/127823953?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522168845810116782427416486%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=168845810116782427416486&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-127823953-null-null.142

安装过程中一直报**node -v 提示'node' 不是内部或外部命令，也不是可运行的程序 或批处理文件**的错误，快逼疯了，

解决方案：https://blog.csdn.net/taylorzun/article/details/105471841

注意事项：

①环境配置好一定要全部关掉。

②终端必须关闭重新打开，安装nvm之前的node版本一定要卸载重装。

③安装好node之后，必须nvm use  [your node version]。

## 三、钩子函数

#### 1、经典的钩子函数

```
beforeCreated：这个钩子函数实在vue实例创建后，触发的。这个时候还没有进行data里的数据监听和事件的初始化
```

```
其实大家很多时候都会在created钩子函数中是调用事件，那么这个数据监听和事件初始化就是在beforeCreated之前和created之后进行的。
```

```
beforeMount 这个进行模板编译，编译模板但是没有元素挂载，无法获取dom
```

```
mounted 元素挂载结束，可以获取dom 元素 
```

```
beforeUpdata 组件更新前调用 
```

```
updataed 组件更新后调用
```

```
beforedestory  vue实例销毁前执行
```

```
destoryed   vue实例销毁之后执行  vue实例销毁后，dom元素还存在但是数据双向绑定，vue的功能就没有了，比如数据双向绑定。下面有一段代码，大家可以copy过去在控制台上打印看一看，加深对这几个钩子函数的理解 ，
```

![image-20230703225842308](https://raw.githubusercontent.com/201819830tsx/pic/master/img/image-20230703225842308.png)

![image-20230703230451866](https://raw.githubusercontent.com/201819830tsx/pic/master/img/image-20230703230451866.png)

![img](https://raw.githubusercontent.com/201819830tsx/pic/master/img/1511283-20181220191956879-804358578.png)

## 四、路由

<font color='blue'>注意：路由信息是用`$route`,而路由导航是用`$router`</font>

在Vue.js 2中，`$route` 和 `$router` 是两个不同的对象，分别用于访问路由信息和执行路由导航。

- `$route`： `$route` 是一个用于访问当前激活路由的只读属性。它包含了当前路由的各种信息，如路由路径、参数、查询参数等。通过 `$route`，可以获取当前路由的信息并在组件中进行使用。
- `$router`：`$router` 是路由器的实例对象，它提供了一些用于动态导航的方法。通过 `$router`，可以执行路由导航操作，如跳转到不同的路由、返回上一级路由等。常见的方法包括 `$router.push()`、`$router.replace()`、`$router.go()` 等。

#### 1.入门学习

我的理解：**路由可以理解为计算机网络中的路由器，一个是转发包，一个是转发组件（对应的HTML片段），将组件映射到对应的地址，进行渲染。**

使用 Vue.js，我们已经用组件编写了我们的应用程序。将 **Vue Router** 添加到组合中时，我们所需要做的就是将组件映射到路由，并让 Vue Router 知道在哪里渲染它们。

~~~html
<script src="https://unpkg.com/vue@3"></script>
<script src="https://unpkg.com/vue-router@4"></script>

<div id="app">
  <h1>Hello App!</h1>
  <p>
    <!-- use the router-link component for navigation. -->
    <!-- specify the link by passing the `to` prop. -->
    <!-- `<router-link>` will render an `<a>` tag with the correct `href` attribute -->
    <router-link to="/">Go to Home</router-link>
    <router-link to="/about">Go to About</router-link>
  </p>
  <!-- route outlet -->
  <!-- component matched by the route will render here -->
  <router-view></router-view>
</div>
~~~

不使用常规`a`标签，而是使用自定义组件`router-link`来创建链接。这使得 Vue Router 可以在**不重新加载页面**的情况下更改 URL、处理 URL 生成及其编码。

`router-view`将显示与 URL 对应的组件

~~~javascript
// 1. Define route components.
//声明HTML模板
const Home = { template: '<div>Home</div>' }
const About = { template: '<div>About</div>' }

// 2. Define some routes
// Each route should map to a component.
// We'll talk about nested routes later.
const routes = [
  { path: '/', component: Home },
  { path: '/about', component: About },
]

// 3. Create the router instance and pass the `routes` option，
//创建VueRouter实例
const router = VueRouter.createRouter({
  // 4. Provide the history implementation to use. We are using the hash history for simplicity here.
  history: VueRouter.createWebHashHistory(),
  routes, // short for `routes: routes`
})

// 5. Create and mount the root instance.
const app = Vue.createApp({})

//通过调用app.use(router)，我们触发初始导航,提供对任何组件内部this.$router以及当前路线的访问：this.$route
app.use(router)

app.mount('#app')

// Now the app has started!
~~~

另外一种代码

~~~JavaScript
<script>
    /*
    * 申明三个模板( html 片段 )
    */
    var user = { template: '<p>用户管理页面的内容</p>' };
    var second = { template: '<p>产品管理页面的内容</p>' };
    var order = { template: '<p>订单管理页面的内容</p>' };
    /*
    * 定义路由
    */
    var routes = [
        { path: '/', redirect: '/user'}, // 这个表示会默认渲染 /user，没有这个就是空白
        { path: '/user', component:   },
        { path: '/product', component: second },
        { path: '/order', component: order }
    ];
    /*
    * 创建VueRouter实例
    */
    var router = new VueRouter({
        routes:routes
    });
    /*
    * 给vue对象绑定路由
    */
    new Vue({
        el:"#app",
        router
    })
 
</script>
~~~

#### 2.VueRouter使用（5+2）

五个步骤

①下载：下载VueRouter模块到当前工程，版本为3.6.5

```vue
yarn add vue-router@3.6.5
```

②引入

~~~vue
import VueRouter from 'vue-router'
~~~

③安装注册(插件初始化)

~~~vu
Vue.use(VueRouter)
~~~

④创建路由对象

~~~vu
const router = new VueRouter()
~~~

⑤注入、 将路由对象注入到new Vue 实例中，建立关联

~~~vu
new Vue({
  render: h => h(App),
  router
}).$mount("#app")
~~~

两个核心步骤（要在src目录下创建view目录来存放路由使用的组件）

①创建需要的组件，配置路由规则（在main.js中配置）

![image-20230705155027585](https://raw.githubusercontent.com/201819830tsx/pic/master/img/image-20230705155027585.png)

~~~vue
/*
    * 申明三个模板( html 片段 )
    */
    var user = { template: '<p>用户管理页面的内容</p>' };
    var second = { template: '<p>产品管理页面的内容</p>' };
    var order = { template: '<p>订单管理页面的内容</p>' };
    /*
    * 定义路由
    */
    var routes = [
        { path: '/', redirect: '/user'}, // 这个表示会默认渲染 /user，没有这个就是空白
        { path: '/user', component:   },
        { path: '/product', component: second },
        { path: '/order', component: order }
    ];
~~~



②配置导航，配置路由出口（路径匹配的组件显示的位置）

~~~vue
<template>
  <div id="app">
      <div class="menu">
          <!--
              router-link 相当于就是超链
              to 相当于就是 href
          -->
          <router-link to="/find">发现音乐</router-link>
          <router-link to="/friend">朋友</router-link>
          <router-link to="/my">我的音乐</router-link>
      </div>
       
      <div class="workingRoom">
          <!--
              点击上面的/user,那么/user 对应的内容就会显示在router-view 这里
           -->
           <router-view></router-view>   
      </div>
   
  </div>
</template>
~~~



#### 3.组件使用

页面组件，配合路由使用，需要放在views文件夹中，用于页面展示

复用组件，放在components文件夹，用于封装复用

#### 4.封装路由模块

在main.js中只导入router模块，然后注入到vue对象中，

~~~vu
import router from './router/index.js'


Vue.config.productionTip = false

new Vue({
  render: h => h(App),
  router
}).$mount('#app')
~~~

封装

<font color='blue'>注意：此处推荐使用@,代表从src目录开始，指的是绝对路径</font>

~~~vue
//import FriendVue from '@/views/Friend.vue'
import FriendVue from '../views/Friend.vue'
import FindVue from '../views/Find.vue'
import MyVue from '../views/My.vue'
import Vue from 'vue'
import VueRouter from 'vue-router'
//安装注册(插件初始化)
Vue.use(VueRouter)

//定义路由，配置路由规则
const routes = [
	{ path: '/find', component: FindVue},
	{ path: '/friend', component: FriendVue},
	{ path: '/my', component: MyVue}
]

//创建VueRouter对象实例
const router = new VueRouter({
	routes: routes
})

//导出
export default router
~~~

<font color='blue'>注意：最后要导出，作为模块使用</font>

浏览器会自动将<router-link>转换为a标签

~~~vue
<div class="menu">
    <a href="#/find" class="">发现音乐</a>
    <a href="#/friend" class="router-link-exact-active router-link-active" aria-current="page">朋友</a>
    <a href="#/my" class="">我的音乐</a>
</div>
~~~

![image-20230706121829652](https://raw.githubusercontent.com/201819830tsx/pic/master/img/image-20230706121829652.png)

在声明VueRouter对象时，加入如下代码，可以实现匹配的重命名

~~~javascript
linkActiveClass: '类名1',
linkExactActiveClass: '类名2'
~~~

#### 5.声明式导航-跳转传参

在跳转路由时，进行传值

##### 1.查询参数传参

①语法格式

- to="/path?参数名=值"

②对应页面组件接收传递过来的值

- $route.query.参数名

如果是在create中要发请求，拿到参数需要加上this

~~~vu
this.$route.query
~~~

#####  2.动态路由传参

①配置动态路由（我的理解就是将参数名与参数值拆解分开）

URL`/users/johnny`和`/users/jolyne`都会映射到同一路由

~~~javascript
const routes = [
  // dynamic segments start with a colon
  { path: '/users/:id', component: User },
]
~~~

| 图案                         | 匹配路径                | $route.params                            |
| :--------------------------- | :---------------------- | :--------------------------------------- |
| /用户/:用户名                | /用户/爱德华多          | `{ username: 'eduardo' }`                |
| /用户/：用户名/帖子/：帖子ID | /用户/爱德华多/帖子/123 | `{ username: 'eduardo', postId: '123' }` |

②配置导航链接

- to="/path/参数值"

③对应页面组件接收传递过来的值

- $route.params.参数名

#### 6.路由重定向

1.重定向也是在`routes`配置中完成的。重定向`/home`自 至`/`：

**2.捕获404**

当路径找不到时，给个提示界面，*代表任意路径，前面的都不匹配就匹配最后一个

~~~javascript
const routes = [{ path: '/home', redirect: '/' }]
//例子
const routes = [
	{path: '/', redirect: '/my'},
	{ path: '/find', component: FindVue},
	{ path: '/friend', component: FriendVue},
	{ path: '/my', component: MyVue},
	{ path: '*', component: NotFound}
]
~~~

3.模式设置

![image-20230706142735271](https://raw.githubusercontent.com/201819830tsx/pic/master/img/image-20230706142735271.png)

#### 7.编程化导航

**在 Vue 实例内部，可以用`$router`访问路由器实例**

##### 1.path路径跳转

参数可以是字符串路径或位置描述符对象

~~~javascript
// literal string path
this.$router.push('/users/eduardo')

// object with path
this .$router.push({
  path: '/users/eduardo'
})



export default {
  name: 'app',
  methods: {
	  skip() {
		  this.$router.push({
			  path: '/find'
		  })
	  }
  }
}
~~~

##### 2.name命名路由跳转

（适合path路径长的场景，给path命名，简化）

~~~javascript
//在VueRouter的路由规则中配置
const routes = [
	{ path: '/', redirect: '/my'},
	{ name: 'music', path: '/find', component: FindVue},
	{ path: '/friend', component: FriendVue},
	{ path: '/my', component: MyVue}
]
//关键配置
{ name: 'music', path: '/find', component: FindVue}

export default {
  name: 'app',
  methods: {
	  skip() {
		  this.$router.push({
			  //通过路径
			  // path: '/find'
			  //通过name
			  name: 'music'
		  })
	  }
  }
}
~~~

##### 3.path路径传参

![image-20230706150304815](https://raw.githubusercontent.com/201819830tsx/pic/master/img/image-20230706150304815.png)

![image-20230706151137681](https://raw.githubusercontent.com/201819830tsx/pic/master/img/image-20230706151137681.png)

**小bug**

```javascript
this.$router.push('/find?key=${this.inputValue}')
```

在这里，使用的字符串是单引号 `'` 而不是模板字符串所需的反引号 ```。因此，`${this.inputValue}` 不会被动态地插入到字符串中，而只是作为普通的字符串进行传递。为了解决这个问题，你可以将单引号替换为反引号，如下所示：

```javascript
this.$router.push(`/find?key=${this.inputValue}`)
```

##### 4.name命名传参

![image-20230706160848748](https://raw.githubusercontent.com/201819830tsx/pic/master/img/image-20230706160848748.png)

![image-20230706161102776](https://raw.githubusercontent.com/201819830tsx/pic/master/img/image-20230706161102776.png)

##### 5.小结

①**查询参数传参**

不需要修改路由，字符串格式为`/路由路径?参数名=参数值` ，通过`this.$router.query.参数名`来获取参数值

②动态路由传参

需要修改路由，路由格式为`/路径/:参数名`，字符串格式为`/路由路径/参数值`，通过`this.$router.params.参数名`来获取参数值

~~~javascript
export default {
  name: 'app',
  data() {
  	return {
		inputValue: ''
	}
  },
  methods: {
	  skip() {
		  //1.通过路径跳转
		  //（1）this.$router.push('路由路径')[简写]
		  
		  //     this.$router.push('路由路径?参数名=参数值'),这里使用的是反引号
		  
		  // this.$router.push(`/find?key=${this.inputValue}`) 此处为查询参数传参实例
		  
		  // this.$router.push(`/find/${this.inputValue}`) //此处为动态路由传参实例
		  
		  //（2）this.$router.push({  [完整写法]更适合传参
		  //       path: '路由路径', 
		  //	   query: {
		  //	  	 参数名: 参数值
		  //	   }
		  //     })
		  /* this.$router.push({
			  path: '/find',
			  query: {
				  key: this.inputValue
			  }
		  }) 此处为查询参数传参实例*/
		  
		  /* this.$router.push({
			  path: `/find/${this.inputValue}`
		  }) 此处为动态路由传参实例*/
		  
		  
		  //2.通过命名跳转
		  /* this.$router.push({
			  name: 'music'
		  }) */
		  
		  /* this.$router.push({
		  	  name: 'music',
		  	  query: {
		  		  key: this.inputValue
		  	  },
			  params: {
				  key: this.inputValue
			  }
		  }) query方式与动态路由方式*/
		  
		  this.$router.push({
			  name: 'music',
			  query: {
				  key: this.inputValue
			  }
		  })
	  }
  }
}
~~~

<font color='blue'>注意，一定要看清楚自己定义的是什么</font>，有时候是params

~~~vue
<p>value：{{ $route.params.key }}</p>
~~~

## 五、VueX

![68321278417](https://raw.githubusercontent.com/201819830tsx/pic/master/img/1683212784179.png)

#### 1.使用

##### 1.安装 vuex

安装vuex与vue-router类似，vuex是一个独立存在的插件，如果脚手架初始化没有选 vuex，就需要额外安装。

```bash
yarn add vuex@3 或者 npm i vuex@3
```

##### 2.新建 `store/index.js` 专门存放 vuex

​	为了维护项目目录的整洁，在src目录下新建一个store目录其下放置一个index.js文件。 (和 `router/index.js` 类似)

​	![68321280582](D:\BaiduNetdiskDownload\Vue2+3入门到实战-配套资料\02-MD笔记\day07\assets\1683212805824.png)

##### 3.创建仓库 `store/index.js` 

```jsx
// 导入 vue
import Vue from 'vue'
// 导入 vuex
import Vuex from 'vuex'
// vuex也是vue的插件, 需要use一下, 进行插件的安装初始化
Vue.use(Vuex)

// 创建仓库 store
const store = new Vuex.Store()

// 导出仓库
export default store
```

##### 4. 在 main.js 中导入挂载到 Vue 实例上

```js
import Vue from 'vue'
import App from './App.vue'
import store from './store'

Vue.config.productionTip = false

new Vue({
  render: h => h(App),
  store
}).$mount('#app')
```

此刻起, 就成功创建了一个 **空仓库!!**

##### 5.测试打印Vuex

App.vue

```js
created(){
  console.log(this.$store)
}
```



~~~vue
npm install vue@3
~~~

#### 2.非模块化使用小结

##### 1.直接使用

1. state --> $store.state.数据项名
2. getters --> $store.getters.属性名
3. mutations --> this.$store.commit('方法名', 其他参数)
4. actions --> this.$store.dispatch('方法名', 其他参数)

##### 2.辅助函数

类似模块化，以mapMutations为例

> mapMutations和mapState很像，它把位于mutations中的方法提取了出来，我们可以将它导入

```js
import  { mapMutations } from 'vuex'
methods: {
    ...mapMutations(['addCount'])
}
```

> 上面代码的含义是将mutations的方法导入了methods中，等价于

```js
methods: {
      // commit(方法名, 载荷参数)
      addCount () {
          this.$store.commit('addCount')
      }
 }
```

此时，就可以直接通过this.addCount调用了

```jsx
<button @click="addCount">值+1</button>
```

但是请注意： Vuex中mutations中要求不能写异步代码，如果有异步的ajax请求，应该放置在actions中

#### 3.模块化使用小结

##### 1.直接使用

1. state --> $store.state.**模块名**.数据项名
2. getters --> $store.getters['**模块名**/属性名']
3. mutations --> $store.commit('**模块名**/方法名', 其他参数)
4. actions --> $store.dispatch('**模块名**/方法名', 其他参数)

##### 2.借助辅助方法使用

1.import { mapXxxx, mapXxx } from 'vuex'

computed、methods: {

​     // **...mapState、...mapGetters放computed中；**

​    //  **...mapMutations、...mapActions放methods中；**

​    ...mapXxxx(**'模块名'**, ['数据项|方法']),

​    ...mapXxxx(**'模块名'**, { 新的名字: 原来的名字 }),

}

2.组件中直接使用 属性 `{{ age }}` 或 方法 `@click="updateAge(2)"`