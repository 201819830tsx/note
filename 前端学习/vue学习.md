## 一、钩子函数

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