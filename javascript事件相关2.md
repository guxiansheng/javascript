 今天继续学习 javascript 事件相关的知识 
 鼠标 和 滚轮事件 在web开发当中 这也算是最常用的事件了吧！

 我们先来学习学习 鼠标事件，在dom3级中 为鼠标事件新增加了 9个事件，
 其中有6个事件在dom2级的时候 大多数浏览器就已经支持了 所以被加入到了 dom3级的规范当中来 

 1. click   事件， 鼠标单击最常用的事件之一了吧。
 2. dblclick事件， 鼠标双击
 3. mousedown事件，鼠标按下
 4. mouseup事件，  鼠标抬起
 5. mouseover事件, 鼠标移入
 6. mouseout事件,     鼠标移出
 7. mouusenter事件 （不会冒泡的事件），
 8. mouseleave事件 （不会冒泡的事件），
 9. mousemove事件     鼠标移动

 其中click事件是由   mousedown 和 mouseup 事件组合而成的。
 可以通过 阻止这两个事件 从而阻止 click事件的触发！

 可以通过检测 查看浏览器 是否支持上面的DOM2级鼠标事件
 var isSupported = document.implementation.hasFeature("MouseEvents", "2.0")

 可以通过检测 查看浏览器 是否支持上面的DOM3级鼠标事件
 var isSupported = document.implementation.hasFeature("MouseEvent", "3.0")

 还可以通过 事件对象的属性 判断鼠标触发点击事件的时候 相对于 屏幕， 浏览器窗口，和整个页面的(水平，垂直)偏移位置。

 1. 相对于浏览器视口的(水平，垂直)偏移位置 所有浏览器都支持

 var x = clientX;
 var y = clientY;

 2. 相对于页面的(水平，垂直)偏移位置 ieee，7，8不支持 其他浏览器都支持

 var x = pageX;
 var y = pageY;

 3. 相对于屏幕(水平，垂直)偏移的位置

 var x = scrollX;
 var y = scrollY;

 ie6，7，8 可以通过下面的计算来 得出相对于页面的(水平，垂直)偏移位置

 var x = clientX + (document.body.scrollLeft || document.documentElement.scrollLeft);
 var y = clientY + (document.body.scrollTop || document.documentElement.scrollTop);

 可以写成一个兼容函数
 DOM鼠标事件支持 
 altKey, ctrlKey, shiftKey, meatKey是否为 true来判断 是否在键盘上按下了这些键

 IE鼠标事件支持
 altLeft, ctrlLeft, shiftLeft 是否为 true来判断 是否在键盘上按下了这些键

 当触发 mouseover和mouseout这两个事件时 有可能会关联到别的元素，DOM事件的事件也提供了相关的属性来指向 相关的元素
 而 IE 也有不同的事件对象的属性来表示相关的元素

 DOM事件对象的属性为：
 event.relateTarget.

 IE事件对象的属性为：
 toElement, fromElement

 可以写成一个兼容函数。

鼠标按钮：
一个鼠标上面 有左键（主键），右键，中间的滚轮。
我们要判断当属变按下的时候 是按下了三个键中的哪一个？在事件对象里面 也有相应的属性提供 

DOM事件对象中是：
event.button

当值为 0 时 说明按下了 左键
当值为 1 时 说明按下了 中间的滚轮键
当值为 2 时 说明按下了 右键

IE事件对象中也是：
event.button

当值为 1 时 说明按下了 左键
当值为 4 时 说明按下了 中间的滚轮键
当值为 2 时 说明按下了 右键

可以写成一个兼容函数。

鼠标滚轮事件：
mousewheel事件 除了火狐意外的浏览器都支持 包括ie6,7,8。
此事件 可以定义在任何元素上 都会冒泡到document（ie8）上 或者 window对象（ie9）上。

通过判断 event.wheelDelta 的数值得正负 来判断是向前滚动 还是 向后滚动。
正 是代表向前
负 是代表向后

注意 在oprea9.5版本以前 刚好相反
正 是代表向后
负 是代表向前 
所以 这里可以通过浏览器判断一下版本号 给结果取反 就OK了

而火狐了 
支持鼠标滚轮事件的事件是：
DOMMouseScroll

它通过event.detail 的数值得正负 来判断是向前滚动 还是 向后滚动。
正 是代表向后
负 是代表向前 

所以这里也可以 集合前面的结果来 写成一个兼容的函数。
由于时间太晚了 具体代码就不再这里写了。
布丁-阿_____________________ 晚安 !!!