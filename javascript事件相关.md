国庆过完了 虽然系统的学习了BOM, DOM, DOM扩展，DOM2, DOM3的相关内容  但是为国庆制定的学习计划却未能完成 还是太懒了！
今天来看看 js的事件流， 事件处理程序， 事件对象吧。

一：事件流

    在 javascript 中 什么是事件流?
    其实也就是 事件传播的过程和阶段， 事件流 总共有3个阶段 
    1. 事件捕获阶段。
    2. 目标元素阶段。
    3. 事件冒泡阶段。

    这三个阶段的 顺序和过程也是按上面的 1，2，3的顺序传播事件的。

    首先来说事件捕获， 举个例子：

    var oBtn = document.getElementById("btn");
    oBtn.onclick = function () {
        alert("1");
    };

    我们为页面上 一个按钮元素 绑定一个事件处理程序，当我们点击这个按钮的时候，
    首先 接收到这个消息的是 window对象。 如果这时候 window对象上 也绑定了onclick事件，
    那么就会触发 window对象上的onclick属性所保存的事件处理函数.
    如果没有 则事件就会传递向document对象. 
    同理 如果document对象上也绑定了onclick事件，那么事件也会被触发.
    然后 事件依次向下传递给
    html,
    body,
    div...
    目标元素。
    直到事件传递到目标元素的父元素的时候 事件捕获阶段 就算完成了 。

    然后 当事件传递到达目标元素的时候 这就是第二个阶段 目标元素阶段了（虽然也属于事件冒泡的一部分）
    这时候就会触发 我们绑定到目标元素上面的事件处理函数了 

    最后一个阶段 就是事件冒泡阶段：
    也就是 事件离开目标元素 顺着事件捕获时候的路 依次返回的阶段 就是事件冒泡阶段 。
    事件最终也是会冒泡到window对象上才会停止。

    以上就是我们说的 整个事件流的过程 。 

二: 事件处理程序

    js 的事件处理程序了有三种绑定事件处理程序的方法

    1.为html元素特性赋值一个事件处理函数。
    2.DOM0级绑定事件处理函数的方式。
    3.DOM2级绑定事件处理程序的方式。

    我们先来看看第一种 为html元素 添加事件处理程序

    function show() {
        alert("1")
    }
    <button id="btn" onclick="show()"></button>

    这种绑定事件的好处 暂时没想到，
    坏处了 倒是有几点：
    1.违背w3c的标准 结构， 样式， 行为没有做到完全分离。
    2.可阅读 和 可维护性差，紧密耦合。

    第二种是DOM级的绑定方式 

    var oBtn = document.getElementById("btn");
    oBtn.onclick = function () {
        alert("1");
    };

    为DOM元素的属性直接绑定 事件处理函数
    这种绑定事件的方式优缺点如下：
    1.结构 与 行为分离， 可阅读，可维护，低耦合。
    2.事件处理函数的作用域就是 DOM元素的作用域 很容易访问当前 DOM元素的其它属性什么的。
    3.缺点就是 只能为同类型的事件绑定一个事件处理函数。

    第三种是 DOM2级绑定事件的方式

    以IE9+的主流浏览器支持:
    addEventListener,
    removeEvenetListen.

    IE6,IE7,IE8支持
    attachEvent,
    detachEvent.

    以下是 绑定事件处理程序的跨浏览器兼容写法

    var addEvent = (function () { 
        if (document.addEventListener) { 
            return function (el, type, fn) { 
                el.addEventListener(type, fn, false); 
            }
        } else { 
            return function (el, type, fn) { 
                el.attachEvent('on' + type, function () { 
                    return fn.call(el, window.event); 
                }); 
            } 
        } 
    })();

    这里其实有个地方值得我们思考
    也就是
    el.addEventListener(type, fn, false);的第三个参数
    这个第三个参数为false的时候 表示事件只在冒泡阶段可以触发 在捕获阶段 不触发。一般也是false
    因为 在冒泡阶段触发事件 可以做到最大的浏览器兼容性。
    还有就是 如果设置为了 true那么 有可能会在捕获阶段 触发目标元素的事件处程序 那样的话 目标元素的事件处理程序有可能被触发两次。（书上是这么说的 我也没有实践过！！！）。


    为什么 attachEvent绑定事件的时候 不用传入第三个参数了？？？
    因为 attachEvent是针对 IE6,ie7,ie8浏览器的 它们只支持 事件冒泡 不支持 事件捕获。

    以上绑定事件处理程序 也以相同的方法 删除已经绑定的事件处理程序，不过在删除的时候 只能删除指定的事件处理函数，
    删除匿名事件处理函数是没有用的 根本不是一个对象。
    var removeEvent = function( obj, type, fn ) {
        if (obj.removeEventListener)
            obj.removeEventListener( type, fn, false );
        else if (obj.detachEvent) {
            obj.detachEvent( "on"+type, obj["e"+type+fn] );
            obj["e"+type+fn] = null;
        }
    }; 

三: 事件对象

    事件对象 分为 DOM事件对象 和 IE事件对象
    事件对象 只在事件处理函数执行的时候存在 函数执行完毕 事件对象就消失。 
    事件对象 在事件处理函数被执行的时候 作为第一个参数被传入事件处理函数中，IE6，7，8的事件对象 被挂在到了window对象上
    直接在window对象上取

    DOM事件对象有以下属性

    bubbles	            返回布尔值，指示事件是否是起泡事件类型。
    cancelable	        返回布尔值，指示事件是否可拥可取消的默认动作。
    currentTarget	    返回其事件监听器触发该事件的元素。
    eventPhase	        返回事件传播的当前阶段。
    target	            返回触发此事件的元素（事件的目标节点）。
    timeStamp	        返回事件生成的日期和时间。
    type	            返回当前 Event 对象表示的事件的名称。
    initEvent()         初始化新创建的 Event 对象的属性。
    preventDefault()	通知浏览器不要执行与事件关联的默认动作。
    stopPropagation()	不再派发事件。



    IE的事件对象包括下面这些属性 

    cancelBubble	    如果事件句柄想阻止事件传播到包容对象，必须把该属性设为 true。
    fromElement	        对于 mouseover 和 mouseout 事件，fromElement 引用移出鼠标的元素。
    keyCode	            对于 keypress 事件，该属性声明了被敲击的键生成的 Unicode 字符码。对于 keydown 和 keyup 事件，它指定了被敲击的键的虚拟键盘码。虚拟键盘码可能和使用的键盘的布局相关。
    offsetX,offsetY	    发生事件的地点在事件源元素的坐标系统中的 x 坐标和 y 坐标。
    returnValue	        如果设置了该属性，它的值比事件句柄的返回值优先级高。把这个属性设置为 fasle，可以取消发生事件的源元素的默认动作。
    srcElement	        对于生成事件的 Window 对象、Document 对象或 Element 对象的引用。
    toElement	        对于 mouseover 和 mouseout 事件，该属性引用移入鼠标的元素。
    x,y	                事件发生的位置的 x 坐标和 y 坐标，它们相对于用CSS动态定位的最内层包容元素。

    各自大多数 都有对应的属性 就不一一列举了。

    阻止事件冒泡 和 默认事件：

    var  stopEvent = function(e){
      e = e || window.event;
      if(e.preventDefault) {
        e.preventDefault();
        e.stopPropagation();
      }else{
        e.returnValue = false;
        e.cancelBubble = true;
      }
    },

    

    在通过DOM2级事件绑定事件的时候 为同一个事件类型 绑定多个事件处理函数的时候 

    DOM2级标准的 事件执行顺序是按照绑定事件的顺序 依次执行 
    而 IE6，7，8的 执行顺序刚好相反

    但是 事件对象有个属性 可以阻止别的同类型事件处理函数的触发 而让本身先触发 这就是事件优先级
    stopImmediatePropagation属性就有这个作用。

    最后是自定义事件的写法：

    var eve = new Event("custome");
    element.addEventListener("custome", function(){
        console.log("custome");
    })

    element.dispatchEvent(eve);



    继续努力 今晚就到这里吧 晚安 布丁-啊————————————————————————