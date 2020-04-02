

# 概念

### 项目与产品

项目：可直接通过区域系统发布，配置基本信息，然后通过具体的用户直接访问。

产品：不能直接访问的系统，必须首先创建业务系统，

​			将产品下面的资源复制到对应的业务系统下面，然后通过具体的用户直接访问，

​			并且任意几个产品下面的资源都可以重新组装成一个新的系统。  

### 用户、角色、组

* 用户

应用系统的具体操作者，用户可以自己拥有权限信息，可以归属于0～n个角色，可属于0～n个组。他的权限集是自身具有的权限、所属的各角色具有的权限、所属的各组具有的权限的合集。它与权限、角色、组之间的关系都是n对n的关系。

* 角色

为了对许多拥有相似权限的用户进行分类管理，定义了角色的概念，例如系统管理员、管理员、用户、访客等角色。角色具有上下级关系，可以形成树状视图，父级角色的权限是自身及它的所有子角色的权限的综合。父级角色的用户、父级角色的组同理可推。

* 组

为了更好地管理用户，对用户进行分组归类，简称为用户分组。组也具有上下级关系，可以形成树状视图。在实际情况中，我们知道，组也可以具有自己的角色信息、权限信息。这让我想到我们的QQ用户群，一个群可以有多个用户，一个用户也可以加入多个群。每个群具有自己的权限信息。例如查看群共享。QQ群也可以具有自己的角色信息，例如普通群、高级群等。

### 客户端

ut 是用户标签  ;  

md 是元数据标识  ; 

st 是系统标识,通常由设计器为每一个控件自动生成

#### 属性

md：页面元素上绑定的元数据字段英文名称。

mdtype：页面元素上绑定的元数据类型。

mdcn：页面元素上绑定的元数据中文名称。

ut：唯一标识页面元素的属性，可通过 this.getUT()获取标签对象

st：唯一标识页面元素的属性，可通过 this.getST()获取标签对象

autosize：用于列表控件元素，主要表示该列表控件每次展示数据条数将根据页面的控件高度自动计算。

rowheight：主要用于列表控件，指明列表控件展示数据时每行的高度。

pagSize：用于列表控件元素，指明该列表控件展示数据时，每页展示多少条数据。

bindevends：指明页面元素绑定的事件和触发的方法。

tpo：用于列表控件，主要说明操作列的属性设置。

ct：标识控件的类别。

bt：标识输入类别，主要用在 input 元素

search：主要用在列表模型，标识查询条件，查询条件规则如下：分为两位，第一位:1:and 2:or 第二位开始: 1:= 2:like:%%
	3:like % 4:> 5:< 6:>= 7<= 8:<> 9:like @v% 10:not like %@v 11:not
	like %@v% 12: not like 13:is null 14:is not null 默认为:12。

ht：元素类别  

#### 模型

模型页面通常会在 html 第一行包含多种属性，其中包含了数据模型，

页面模型声明，页面资源信息，页面操作，等信息  。

![1585799171326](%E5%85%AC%E5%8F%B8.assets/1585799171326.png)

常见属性

pagemodulecn 页面模型中文名称
pagemodule 页面模型名称
pagemodulepath 页面模型资源路径
autorefreshparentmodule自动刷新父模型
module 数据模型，一般为数据表或视图
modulecn 数据模型中文名称
moduleheight 模型高度
modulewidth 模型宽度
moduletype数据模型类型信息模型，查询模型，业务排版模型可选tableinfo,search,partmodule	   modules 本页面加载的模型
formtype 本页面窗体类型{3：弹出|4：滑出}
lem 页面操作
autorefreshonpageload页面刷新的时候自动执行 pageLoad 方法可选值
service_search 绑定数据查询方法
service_delete 绑定数据删除方法
moduleversion 模型版本

### 规范

```html
<!--@ module = 模型名 -->
不能删，记住
```

### JS

#### 知识点

###### this

- 1、在对象方法中， this 指向调用它所在方法的对象。
-  2、单独使用 this，它指向全局(Global)对象。
-  3、函数使用中，this 指向函数的所属者。
-  4、严格模式下函数是没有绑定到 this 上，这时候 this 是 undefined。
-  5、在 HTML 事件句柄中，this 指向了接收事件的 HTML 元素。
-  6、apply 和 call 允许切换函数执行的上下文环境（context），即 this 绑定的对象，可以将 this 引用到任何对象。

###### call()

```js
function add (x, y) 
{ 
    console.log (x + y);
} 
function minus (x, y) 
{ 
    console.log (x - y); 
} 
add.call (minus , 1, 1);    //2 
```

这个例子中的意思就是用 add 来替换 minus ，add.call(minus ,1,1) == add(1,1) ，所以运行结果为：

console.log (2); // 注意：js 中的函数其实是对象，函数名是对 Function 对象的引用。

A.call( B,x,y )： 改变函数A的this指向，使之指向B ;把A的函数放到B中运行，x 和 y 是A方法的参数。

用call来实现继承，用this可以继承所有方法和属性。

###### 继承

```js
function Parent() {
  this.name = 'parent5';
  this.play = [1, 2, 3];
}

function Child() {
  Parent.call(this);
  this.type = 'child5';
}
// 产生一个中间对象隔离`Child`的`prototype`属性和`Parent`的`prototype`属性引用的同一个原型。
Child.prototype = Object.create(Parent.prototype); 
// 给Child的原型对象重新写一个自己的constructor。
Child.prototype.constructor = Child;
```



#### 常用方法

###### 去除特殊字符

```js
/*
 * 去除特殊字符，包括空格
 */
function stripscript(s) 
{ 
	var pattern = new RegExp("[`~!@#$^&*()=|{}':;',\\[\\].<>/?~！@#￥……&*（）——|{}【】‘；：”“'。，、？]") 
	var rs = ""; 
	for (var i = 0; i < s.length; i++) { 
		rs = rs+s.substr(i, 1).replace(pattern, ''); 
	} 
	return rs; 
}
```

###### 克隆，深拷贝

```js
/**
 * 对象克隆&深拷贝
 */
function clone(obj) {
  // Handle the 3 simple types, and null or undefined
  if (null == obj || "object" != typeof obj) return obj;
  // Handle Date
  if (obj instanceof Date) {
    var copy = new Date();
    copy.setTime(obj.getTime());
    return copy;
  }
  // Handle Array
  if (obj instanceof Array) {
    var copy = [];
    for (var i = 0,
    len = obj.length; i < len; ++i) {
      copy[i] = clone(obj[i]);
    }
    return copy;
  }
  // Handle Object
  if (obj instanceof Object) {
    var copy = {};
    for (var attr in obj) {
      if (obj.hasOwnProperty(attr)) copy[attr] = clone(obj[attr]);
    }
    return copy;
  }
  throw new Error("Unable to copy obj! Its type isn't supported.");
}
```

###### 继承

```js
//伪经典继承，将原型链和借用构造函数的技术组合到一块。使用原型链实现对原型属性和方法的继承，而通过构造函数来实现对实例属性的继承。
function SuperType(name) {
	this.name = name;
	this.colors = ["red", "blue", "green"];
}

SuperType.prototype.sayName = function() {
	alert(this.name);
}

function SubType(name, age) {
	// 继承属性
	SuperType.call(this, name);
	this.age = age;
}

// 继承方法
SubType.prototype = new SuperType();
SubType.prototype.constructor = SubType;
SubType.prototype.sayAge = function() {
	alert(this.age);
};

var instance1 = new SubType("Nicholas", 29);
instance1.colors.push("black");
alert(instance1.colors); //"red,blue,green,black"
instance1.sayName(); //"Nicholas";
instance1.sayAge(); //29
var instance2 = new SubType("Greg", 27);
alert(instance2.colors); //"red,blue,green"
instance2.sayName(); //"Greg";
instance2.sayAge(); //27 
```













