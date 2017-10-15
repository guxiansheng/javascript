                                               继续javascript事件相关的学习 gogogo！！！！

HTML5事件：

    1. contextmenu事件 上下文菜单事件

        通常在我们按下鼠标右键的时候，会自动弹出一个上下文的菜单栏，但是有时候我们并不希望出现系统自带和默认的上下文菜单栏
        我们更希望阻止它，弹出我们自定义的菜单栏。

        所以就出现了contextmenu事件 供我们用来 阻止默认的菜单，实现我们自己的右键上下文菜单。
        由于contextmenu事件是可以冒泡的 所以我们可以把这个事件定义在 document上 以此来处理页面中所有类似的事件。
        在兼容dom的浏览器中我们可以用 event.preventDefault() 来阻止这个默认事件，
        在IE中 我们还是用event.returnValue = false 来阻止这个默认事件。

        因为contextmenu实际上是属于鼠标类事件 所以在事件对象中，包含了当前鼠标点击右键时候的鼠标坐标（位置），
        我们可以借此来 显示我们自定义的上下文菜单哦！！！

        支持这个事件的浏览器有： IE, FF, Chrome, Opear 11+, Safari

    2. beforeunload事件 页面卸载前触发这个事件

        这个事件会在浏览器卸载之前触发，目的是为了不让用户立马离开当前页面，在用户离开当前页面时，弹出一个弹出框，
        让用户选择是 离开页面 还是继续使用当前页面。
        我们可以自定义弹出框的内容，方法就是将这个事件对象的 event.returnValue的值 设置为：我们要显示给用户的字符串。同时作为函数的返回值返回。（当前只有：Safari, Chrome）支持。

    3. DOMContentLoaded事件 

        故名思意 就是说在DOM内容加载完成的时候 触发的事件。
        window的 load事件 是在所有一切加载完毕的时候（包括DOM，JS文件，css文件，图片，所有外部资源）触发。
        但是这个过程 有可能会因为资源过大 导致用户迟迟不能和页面交互，从而影响用户体验，所以为了更早的让用户可以和页面进行交互，
        就有了在 load事件之前触发的事件，也就是只需要DOM加载完成，不需要所有资源加载完成就可以操作和交互的事件。DOMcontentload!!!!

        可以为 document， window 指定这个这个事件处理程序，这个事件会冒泡到window。此事件 一般都会先于 load事件触发。

        支持 这个事件的浏览器有 IE9+, FF, Chrome, Opear 9+, Safari 3,1+。

        在不支持 DOMcontentload事件的 浏览器中 我们可以用 其它方法来代替，这个我们后里面来再说。

    4. readystatechange事件 

        IE为 DOM文档中的一部分元素 提供了这个事件的接口, 凡是支持这个事件的元素 都有一个 readyState属性。
        此属性包含有以下5个值：

            1. uninitialized （未初始化）
            2. loading （正在加载中）
            3. loaded （加载完毕）
            4. interactive （可以交互了）
            5. complate （完成）
            但是 并不是每一个对象 都会依次经过这几个过程，有的会跳过一些过程 直接跳到complate 。
            
            在这几个阶段中 interactive 和 DOMcontentload 事件触发的时机类似 都是代表已经可以让用户交互了！！！

            在与load事件一起用的时候 其实很难保证 两个事件的执行顺序。有可能先，也有可能后。主要还是得看页面的资源加载的大小。
            所以在触发元素的readystatechange事件的时候 我们要通过判断readyState属性的值来达到尽早的让用户可以和页面交互。

            if (document.readyState == 'interactive' || document.readyState.complate) {
            doSomething....
            }

            在 script元素 和 link元素上 都部署了这个接口，可以用来判断外部资源是否加载完毕！！！！

    5. pageshow, pagehide事件

        FF 和 Opear 有一个特性 叫做 “往返缓存”，也叫做 "bfcache"
        意思就是当用户 通过浏览器的 “前进”和“后退” 按钮时可以让页面从缓存红读取页面 从而让页面能够 快速的加载进来！！
        bfcache实际上 是将整个页面都保存在了内存当中 包括页面数据，dom， javascript状态！。
        如果当页面是从 bfcache中 读取出来的时候 页面是不会触发load事件的。

        我们再说pageshow事件：
        这个事件 是在页面显示时触发，也就是说 无论页面是从bfcache中读取显示，还是重新刷新以后显示，都会触发此事件。
        pageshow事件会在 load事件后面触发。
        必须将这个事件 添加给window。
        pageshow事件的 event对象当中 有一个叫做 "persisted"的属性
        这个属性在 pageshow事件对象当中 保存了一个布尔值，
        如果 页面是被保存在了 bfcache中 那就这个属性值就是true,反之就是 false。

        pagehide事件 是在页面卸载之前触发，而且还是在 unload事件之前触发，事件处理程序也是要添加到window上。
        此事件对象中也有一个保存布尔值的 "persisted"属性，
        当页面即将被保存到bfcache中时 这个属性的值是 true， 反之就是 false。

        支持这两个事件的 浏览器有：IE9+, FF, Chrome, Opear 9+, Safari5+

    6. hashchange事件
        此事件 主要是当url参数列表发生变化的时候，url#号后面的所有字符发生变化的时候能够通知开发人员。
        此事件必须绑定在window对象上
        支持hashchange事件的浏览器有：IE8+, FF, Chrome, Opear10.6+, Safari5+

        在 FF, Chrome, Opear下触发hashchange事件的事件对象中 会包含 oldUrl, newUrl两个属性 表示hash值改变前的url,
        和改变后的 url。

        最好还是用 location.hash来获取hash值 。

        不过在用之前 我们也最好要检测一下当前浏览器是否支持 hashchange事件了！！！。

设备事件：

    1. orientationchange事件

        此事件是苹果公司为 移动端Safari浏览器添加的事件，作用就是可以让开发人员 知道目前用户使用移动设备是横屏还是竖屏，
        以及横屏的方向是home键是向左还是向右。

        当用户改变移动设备的查看模式时 触发这个事件，但是事件对象中 并没有任何有价值的信息，只有在window.orientation中保存了有
        相关的3个值。

        0：表示移动设备此时为纵向模式。
        90：表示移动设备此时为横向模式，并且home键在右边。
        -90：表示移动设备此时为横向模式，并且home键在左边。

        所有IOS设备 都支持orientationchange事件 和 window.orientation。

    2. MozOrientation事件

        FF3.6也引入了类似的事件 Moz前缀代表特有的事件。
        当设备的加速计检测到设备改变方向的时候，就会触发这个事件，此事件必须添加到window对象上。
        当触发这个事件时， 事件对象中包含了x, y, z三个属性，他们的值都在 -1到1之间，表示的是不同坐标轴上的方向。
        当 x=0, y=0, z=1的时候 设备是纵向模式，
        如果把设备向左边倾斜，x会减小，反之x会增大。

        只有带加速记得设备才支持此事件哦！！！
    
    3. deviceorienttation事件

        此事件与MozOrientation事件类似有相同的支持限制，在window对象上触发，不同的是设备在3维空间超各个方向的定位。
        触发此事件时事件对象中包含5个属性：

        1.alpha 围绕Z轴旋转，是一个 0-360之间的浮点数！
        2.beta 围绕X轴旋转，是一个 -180到180之间的浮点数！
        3.gamma 围绕Y轴旋转， 是一个 -90到90之间的浮点数！
        4.absolute 布尔值，表示设备是否返回一个绝对值。
        5.compassCalibrated，表示设备指南针是否校对过。
        支持此事件的浏览器有：IOS4.2+ Safari, Chrome, Amdroid版Webkit。
    
    4. devicemotion事件

        此事件主要是告诉我们，设备在什么时候移动，在做什么移动，而不仅仅是方向。
        例如：
        1.设备此时正在往下掉落。
        2.正被用户拿着在路上行走。
        ....

        触发此事件时，事件对象中包含以下属性：

        1.acceleration 这是一个包含了x,y,z的对象，在不考虑重力的情况下，告诉你在每个方向上的加速度！！
        2.accelerationIncludingGravity 这是一个包含了x,y,z的对象，在考虑Z轴自然重力的情况下，告诉你在每个方向上的加速度！！
        3.interval 表示毫秒的数值
        4.rotationRate 一个包含表示方向：alpha，beta，gamma的对象。

        我们在使用这些属性时 应该先判断是否为null。
        支持此事件的浏览器有：IOS4.2+ Safari, Chrome, Amdroid版Webkit。

                                        以上事件相关有些细节的地方没有特别详细的道明，可以自行google.









    
