
数组化

什么是数组化？为什么要数组化？怎么实现数组化？

1.数组化就是把 不是数组或者类似数组的数据结构 或者数据类型变成数组

2.因为数组本身就是一种数据结构 也可以通过遍历让我们批量操作元素，让他们享受数组的正统方法！

3.下面就是 实现数组化的方法，我也是从正美大大的书里学习到的！！！嘿

在浏览器中也存在很多类数组的对象.

首先看jquery的markArray

var makeArray = function (array) {
    var ret = [];
    if (array != null) {
        var i = array.length;
        if (i == null || typeof array === 'string' || jQuery.isFunction(array) || array.setInterval) {
            ret[0] = array;
        }
        while (i) {
            ret[--i] = array[i]
        }
    }
    return ret
}

这种数组化的方法 一般都会保证就算没得参数或者是Null的情况下 也要返回空数组

Prototype.js的 数组化方法

function $A(iterable) {
    if (!iterable) {
        return [];
    }
    if (iterable.toArray) {
        return iterable.toArray();
    }
    var length = iterable.length || 0, results = new Array(length);

    while (length--) {
        results[length] = iterable[length];
    }
    return results
}

mootools的数组化方法

function $A(iterable) {
    if (iterable.item) { // 验证是否是类数组
        var l = iterable.length, array = new Array(l);
        while (l--) {
            array[l] = iterable[l];
        }
        return array;
    }
    return Array.prototype.slice.call(iterable);
}

Ext的数组化方法

var toArray = function() {
    retuen isIE ? function (a, i, j, res) {
        res = [];
        Ext.each(a, function (v) {
            res.push(v);
        });
        return res.slice(i || 0, j || res.length)
    } : function (a, i, j) {
        return Array.prototype.slice.call(a, i || 0, j || a.length);
    }
}()


underscore的数组化方法

 _.toArray = function(obj) {
    if (!obj) return [];

    // 如果是数组，则返回副本数组
    // 是否用 obj.concat() 更方便？
    if (_.isArray(obj)) return slice.call(obj);

    // 如果是类数组，则重新构造新的数组
    // 是否也可以直接用 slice 方法？
    if (isArrayLike(obj)) return _.map(obj, _.identity);

    // 如果是对象，则返回 values 集合
    return _.values(obj);
  };

这些方法其实都是大同小异，我们可以取其精华，去其糟泊融入到自己的框架中....







