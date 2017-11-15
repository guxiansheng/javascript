
今天是个特别的日子！让我想到了很多很多， 比如我们家族的怪癖。
从我爷爷开始就不会拍马屁，还有我老汉，还有就是我，简直是如出一辙，就算不会拍马屁也就算了，唯一恼火的是，看着那种只会拍马屁的领导，老想跟他对着干......

言归正传：今天的目标是学习javascript模块化 当然不可能一下记录完全。所以慢慢来，慢慢的来才会走的越来越快！

javascript模块化开发 主要有：

  1.  AMD规范
  2.  CMD规范
  3.  commonJs规范
  4.  es6 module

区别：采用AMD规范的 requireJs 定义和引入的模块可以直接在浏览器端运行。
    而采用commonJs规范的模块化需要通过nodeJs和相关的打包工具来编译打包后才能运行，
    es6 module也需要通过webpack，gulp等相关的编译打包工具转换后 才能在浏览器运行，不能直接在浏览器运行（目前不支持）
    commonJS也是javascript在服务端的模块化规范，commonJs规范倡导：一个单独的文件 就是一个模块，通过require引入，通过exports对象导出.可以不必考虑变量污染和命名空间的问题。

以下是三种模块化的定义方式：

AMD：

define(['./bbb','./ccc'], function(b, c) {
    return {
        k : b + c
    }
})


commonJs：

var b = requier('./bbb')
var c = requier('./ccc')
module.exports = {
    k : b + c
}


es6 module

import b from './bbb'
import c from './ccc'
var k = b + c

export {k}

正美大大说commonJs相关的打包工具非常成熟 ，所以建议大家用commonJs ，理由就是用commonJs规范编写的框架 可以和nodejs编写的测试框架无缝配合哦！！！！

作为一个模块化开发的项目，总会有一个入口文件或者多个入口文件。入口文件的功能 大多数都是用来载入其他子模块的...也就是依赖

模块分很多种模块，首当其冲的就是核心模块，这些模块也是最先执行的模块，司徒正美在书中是这样说的：
这些模块都有三个特点：

    1. 扩展性，是指方便将其他模块的方法或者属性加入进来，丰富壮大整个核心模块
    2. 高可用，是指在其他地方都极其常用，不用重复定义
    3. 稳定性，是指不能再以后升级换代中轻易删除这些常用的api

在很多框架和库中的核心模块 包括以下的功能模块：

    1. 对象扩展
    2. 数组话
    3. 类型判定
    4. 无冲突处理
    5. domReady
    6. .....等

我今天首先来学习一下 对象的扩展吧！

为什么要用对象的扩展 也就是像jquery一样 可以自由的为jquery添加插件！类似于extend , mixin.....这样的方法。
只要对象扩展 就会涉及到对象属性的 增，删，改，就会用到javascript原声的js对象属性描述符api

这个api也是在es5之后才有的，之前是没有的。

可以通过这些对象的描述符api来定义，修改一个对象的属性，甚至还可以定义这些属性是 访问器属性还是数据属性，是可遍历属性 还是不可遍历.....等等很多特性

废话不多说，直接看jquery的 extend方法的源码吧！！！！

jquery.extend = jquery.fn.extend = function () {
    var options, name, src, copy, copyIsArray, clone,
        target = arguments[0] || {},
        i = 1,
        length = arguments.length,
        deep = false
    if (typeof target === 'Boolean') {
        deep = target
        target = arguments[1] || {}
        i++
    }

    if (typeof target !== 'object' && !jquery.isFunction(target)) {
        target = {}
    }

    if (i === length) {
        target = this
        i--
    }

    for (;i < length; i++) {
        if ((options = arguments[i]) != null ) {
            for (name in options) {
                try {
                    src = target[name]
                    copy = options[name]
                } catch (e) {   
                    continue
                }

                // 避免循环引用

                if (target === copy) {
                    continue
                }

                if (deep && copy && (jquery.isPlainObject(copy) || (copyIsArray = Array.isArray(copy)))) {
                    if (copyIsArray) {
                        copyIsArray = false
                        clone = src && Array.isArray(src) ? src : []
                    } else {
                        clone = src && jquery.isPainObject(src) ? src : {}
                    }

                    target[name] = jquery.extend(deep, clone, copy)
                } else if (copy !== void 0) {
                    target[name] = copy
                }
            }
        }
    }
    return target 
}


由于这项功能的重要和常用率 es6 就支持了原生的 对象扩展方法： Object.assgin 

缺点是 低版本浏览器不支持！！！！

不过低版本浏览器可以使用 polyfill来搞定。

今天暂时到这里吧。慢慢来 就会很快！








