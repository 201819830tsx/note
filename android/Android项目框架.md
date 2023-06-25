## 一.项目架构

#### 1.以天通项目为例，这是最基本的项目结构

![image-20230625162919780](https://raw.githubusercontent.com/201819830tsx/pic/master/img/image-20230625162919780.png)

#### 2.app模块

![image-20230625163532182](https://raw.githubusercontent.com/201819830tsx/pic/master/img/image-20230625163532182.png)

## 二、文件存贮

#### 1、Shared Preferences存储

键值对形式，有三种方式来获取Shared Preferences对象，最常用的方式为Context类中的getSharedPreferences()方法，这个方法接收两个参数，第一个为**文件名称**，不存在的会自动创建，该文件一般都存放在/data/data/<package name>/shared_prefs/目录下，第二个方法为操作模式，只有**MODE_PRIVATE**一种模式。

以下为最经典的使用方式

~~~java
SharedPreferences.Editor editor = getSharedPreferences("data", MODE_PRIVATE).edit();
edit.putString("name","tom");//添加数据
editor.apply();//提交数据，完成存储
~~~

读取的时候通过

~~~java
SharedPreferences pref = getSharedPreferences("data", MODE_PRIVATE).edit();
String name = pref.getString("name", "");//根据键获取值
~~~

