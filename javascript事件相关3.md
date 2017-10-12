                                                      键盘事件 和 文本事件
 一 键盘事件：
    当用户在键盘上敲击的时候 就会触发键盘事件，DOM0级规定了键盘事件，DOM3级又重新规范了键盘事件。
    DOM2级 本来是有键盘事件 不过听说再定稿以前又删除了相关内容，所以目前为止 也是靠着DOM0级的键盘事件在操作，
    因为 DOM3级的键盘事件各浏览器支持程度 差异比较大 要做跨浏览器开发 实属不易，所以 最可靠的，还是DOM0级的键盘事件了。

    DOM0级 有三个键盘事件：
    1.keydown   鼠标按下那一瞬间触发
    2.keypress  鼠标按下字符键或者说能够影响到 文本内容显示的键的那一瞬间触发
    3.keyup     鼠标抬起那一瞬间触发

    当用户 按住任意一个键不放的时候 会重复触发 keydown 和 keypress事件
    当用户 按住一个字符键或者说能够影响到 文本内容显示的键不放的时候 会重复触发 keypress事件

    键盘事件用 也支持：shiftkey, ctrlkey, altkey, meatkey的判断。 IE不支持 meatkey

    键码：
    当发生 keydown keyup事件时 event事件对象中 包含一个keyCode属性 这个属性中保存了与键盘上特定的键对应的值。
    如果按下的是 数字或者字符时 此时的 keyCode中 包含的是一个与之对应的 ASCII码。
    DOM 和 IE 都支持这个属性

    键盘的每个键码 可以在google

    不过这里有个例外 ：就是当在键盘上按下 分号“;”时  此时不同的浏览器 keyCode值不一样。
    在 IE， Safari，Chrome中 keyCode ：186
    在 Opera, FF中 keyCode: 59

    字符编码：
    charCode属性
    此属性只有在触发keypress事件时才会有这个属性，这个属性的值保存的是按下这个键的 ASCII码。
    支持这个属性的浏览器有：IE9，Safari，Chrome，FF
    而IE6, IE7, IE8 和 Opear 中 是保存在 keyCode中的，所以可以写成一个 兼容性的函数。

    if (typeof event.charCode == "number") {
        return event.charCode;
    } else {
        return event.keyCode
    }

    DOM3级中的属性变化：
    DOM3级中 已经不再包含 charCode属性了 新增了下面两个属性
    1. key    为了取代keyCode的 包含的值是 一个字符串 。用户在键盘上按下的什么键 key里面就保存的相应的字符串。
       当用户 按下字符 S 时 就保存的 "S" 当用户按下 shift 键时 就保存的 "shift"

    2. char   当按下字符时保存的值与 key 相同， 当按下非字符时保存的值是 null。

    因为 Safari 5 和 Chrome支持的是 keyIdentifier属性 

    所以这三个属性的兼容性 差别太大，不建议在跨浏览器开发中使用哈！！！

    还有键盘事件对象中的 location属性可以判断出用户是在键盘的哪个位置 或者 不同设备 或者 虚拟键盘上触发的键盘事件。
    但是因为 兼容性太差 也不建议大家在 跨浏览器的开发中使用哈！！！

    文本事件：ie9+ Safari 和 Chrome支持

    textInput 是DOM3级中的一个新增事件
    这个事件主要是用来 替代keypress事件的，区别在于：
    1. 在可编辑的区域中输入字符时才会触发这个事件，
    2. 在输入实际的字符时才会触发这个事件。

    由于这个事件主要考虑的 就是输入字符的时候触发， 所以事件对象中有个 data 属性专门用来保存 用户输入的字符的 字符串。
    例如： 
    1. 当用户输入的是 大写的 "S" , 那么data中就保存的是 大写的 "S",
    2. 当用户输入的是 小写的 "s" , 那么data中就保存的是 小写的 "s".

    此事件对象中 还有一个 inputMethod属性 保存的是数值，不同的数值 代表用户是以不同的方式 输入的文本，
    比如 粘贴，拖拽......等等。

    但是由于兼容性太差 也不建议大家跨浏览器开发。

    复合事件：是DOM3级中新增的一类事件用来处理 IME输入序列的
    IME可以输入在物理键盘上 找不到的字符哦....通常需要同时按住 多个字符键，但是同时只能输入一个字符哈！！！
    有以下三种 复合事件：

    1. compositionstart     表示要开始输入了之前触发

    2. compositionupdate    表示在输入字段中插入新的字符时触发

    3. compositionend       表示输入完毕时触发

    当触发这三个事件时  事件对象中 包含一个 data 属性里面保存着 事件发生时候的输入值。

    悲剧的是 ie9+ 是唯一支持 复合事件的浏览器 所以 GG 了。跨浏览器还是别用了吧

    最后来看一个 变动事件吧：
    这个事件是 DOM2级的事件
    定义了 以下几个属性了：

    1. DOMSubtreeModified
    2. DOMNodeRemoved
    3. DOMNodeInserted
    4. DOMNodeInsertedIntoDocument  不会冒泡的事件
    5. DOMNodeRemovedFromDocument   不会冒泡的事件
    6. DOMAttrModified
    7. DOMCharacterDataModified

    可以通过 dicument.implementation.hasFeature("MutationEvents", "2.0") 方法来判断 浏览器是不是支持这个 变动事件。

    当我们通过 removeChild, replaceChild 这两个方法删除节点的时候
    会在节点被删除以前先触发以下事件触发：

    1.DOMNodeRemoved
    2.DOMNodeRemovedFromDocument 不会冒泡的事件
    3.DOMSubtreeModified

    当我们通过 appendChild, replaceChild, insetBefore 这三个方法添加/替换节点的时候
    会在节点被添加以后触发以下事件触发：

    1.DOMNodeInserted
    2.DOMNodeInsertedIntoDocument 不会冒泡的事件
    3.DOMSubtreeModified

    除了ie9+以外 所有现代浏览器都支持这三个变动事件
    1. DOMSubtreeModified
    2. DOMNodeRemoved
    3. DOMNodeInserted

    其余暂不考虑！！！！！

    今天就到这里了！ 由于最近也在读zepto的源码 所以精力实在有限 ，不详细的地方，只有自己查漏补缺了 也欢迎指出来！不胜感激。
    当然 写写的这些笔记 都只是为了让自己的所学能够留下点印记而已 ，愿大家一起进步!!!!
    
    晚安