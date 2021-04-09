双向数据绑定原理实践
指视图（v）的变化能实施的让数据模型（m）发生变化，数据的变化也能实时更新到视图。

第一类：视图的变化能实时的让数据模型变化，，常见用于用户输入数据的输入框，（input 等），监听数据的变化，将值传入到模型中进行处理

<label for="inputText">Name:</label>
<input id="inputText" type="text"/>
<span id="value"></span>

<button id="change-button">change value</button>

var inputData = "";
var inputText = document.querySelector("#inputText");
inputText.addEventListener("input", function(e) {
  inputData = e.target.value;
  document.querySelect('#value').innerText = inputData
});
第二类：数据变化时，对应的视图的值也发生变化，需要更新视图的值。

var button = document.querySelector("#change-button");
button.addEventListener("click", function(e) {
  inputText.value = "change";
  inputDate = "change";
});
双向绑定实现方式

手动绑定：两个单身绑定的结合，通过手动 set 和 get 数据来触发 UI 或数据变化。
脏检查机制：发生指定的事件（http 请求，dom 事件），遍历数据响应的元素，然后进行数据比较，对变化的数据进行操作。
数据劫持：通过 hack 的方式（Object.defineProperty()对数据的 setter 和 getter 进行劫持，在数据变化的时候，通知相应的数据订阅者，已出发相应的监听回调）
数据劫持可以独立于框架和事件运行，代码量小，以其为例子，介绍绑定流程 。

MDN Object.defineProperty() 方法会直接在一个对象上定义一个新属性，或者修改一个对象的现有属性，并返回此对象。

为一个作用于中的对象进行 sette。getter 劫持， setter：改变一个属性的值时，会执行一些对应的 setter getter：获取这个属性的值

// 数据劫持示例

var o = {};
var bValue;
Object.defineProperty(o, "b", {
  get: function() {
    return bValue;
  },
  set: function(val) {
    bValue = val;
  },
  enumerable: true,
  configurable: true, // 属性是否可删除，除了writable特性外的其他特性是否可以被修改
});
o.b = 33; // 触发setter

删除set函数的赋值语句，即使执行了赋值，值也终是undefined，因为set的操作被清空了

在更新数据模型时关联到相应的UI修改逻辑：

