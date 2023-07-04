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
        { path: '/user', component: user },
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

