## 一.节点学习

#### 1.节点概念

DOM把所有的html都转换为节点
**整个文档** 是一个节点
**元素** 是节点
**元素属性** 是节点
**元素内容** 是节点
**注释** 也是节点

通过document.getElementById获取了id=d1的**div标签**对应的**元素节点**
然后通过**attributes** 获取了该节点对应的**属性节点**
接着通过**childNodes**获取了**内容节点**

~~~html
<html>
<body>
<div id="d1">hello HTML DOM</div>
  
</body>
  
<script>
function p(s){
    document.write(s);
    document.write("<br>");
}
var  div1 = document.getElementById("d1");
p("文档节点"+document);
p("元素"+div1);
p("属性节点"+div1.attributes[0]);
p("内容节点"+div1.childNodes[0]);
</script>
  
</html>
~~~

![image-20230626154316237](https://raw.githubusercontent.com/201819830tsx/pic/master/img/image-20230626154316237.png)

#### 2.获取节点

![image-20230626154413804](https://raw.githubusercontent.com/201819830tsx/pic/master/img/image-20230626154413804.png)

#### 3.节点属性

![image-20230626154438219](https://raw.githubusercontent.com/201819830tsx/pic/master/img/image-20230626154438219.png)

#### 4.节点关系

**childNodes**和**children**都可以获取一个元素节点的子节点。其中childNodes 会**包含**文本节点，children 会**排除**文本节点，通过children来找到对应的节点

![image-20230626154541465](https://raw.githubusercontent.com/201819830tsx/pic/master/img/image-20230626154541465.png)

~~~HTML
<!DOCTYPE html>
<html>
 
<table id="t">
<tbody id="tbody">
    <tr>
        <td>英雄名称</td><td>操作</td>
    </tr>
    <tr>
        <td>盖伦</td><td><a href="#" onclick="removeTr(this)">删除
 
</a></td>
    </tr>
    <tr>
        <td>提莫</td><td><a href="#" onclick="removeTr(this)">删除
 
</a></td>
    </tr>
    <tr>
        <td>乞求着</td>
		<td>
		<a href="#" onclick="removeTr(this)">删除</a>
		<a href="#" onclick="removeTr(this)">清空</a>
		</td>
    </tr>
</tbody>
</table>
<script>
function removeTr(a){
var t=document.getElementById("tbody");
var pNode=a.parentNode;
var trNode=pNode.parentNode;
var d=confirm("确定删除?"+trNode.children[1].children[1].textContent);
if(d==true){t.removeChild(trNode);};
}
</script>
 
</html>
~~~

运行效果

![image-20230626154703001](https://raw.githubusercontent.com/201819830tsx/pic/master/img/image-20230626154703001.png)

