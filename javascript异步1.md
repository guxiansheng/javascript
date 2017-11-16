        // 异步的实现原理 就是把回调函数 作为参数 传递过去,等到有结果以后 触发回调函数.
        // 事件绑定有明显的 订阅-发布的模式,而异步操作没有, 异步操作是系统自动调用回调, 事件绑定是用户手动触发
        

        // jquery v1.5以前  $.ajax()返回的是一个XHR对象
        // jquery v1.5以后  $.ajax()返回的是一个deferred对象 拥有done, fail方法 等待着返回的结果以后调用
        // 记住一句话————虽然 JS 是异步执行的语言，但是人的思维是同步的————因此，开发者总是在寻求如何使用逻辑上看似同步的代码来完成 JS 的异步请求。
        // 而 jquery 的这一次更新，让开发者在一定程度上得到了这样的好处。
        // 之前无论是什么操作，我都需要一股脑写到callback中，现在不用了。现在成功了就写到done中，失败了就写到fail中，
        // 如果成功了有多个步骤的操作，那我就写很多个done，然后链式连接起来就 OK 了。

        // deffered对象主要解决了 回调函数的嵌套的问题 还提供了 链式写法. 还有主动改变deffered对象状态的 方法. 如果乱写 会导致程序乱套
        // dtd.resolve dtd.reject(可以手动改变状态的方法)
        // dtd.then dtd.done dtd.fail(提供链式写法的方法)

        // promise 返回的是 promise对象 由于promise对象 没有 resolve ,reject和方法 所以不会手动改变状态的方法 所以程序不会乱套
        // waitHandle函数内部，使用dtd.resolve()来该表状态，做主动的修改操作
        // waitHandle最终返回promise对象，只能去被动监听变化（then函数），而不能去主动修改操作 一个“主动”一个“被动”，完全分开了。


        /*
            function waitHandle() {
                var dtd = $.Deferred()
                var wait = function (dtd) {
                    var task = function () {
                        console.log('执行完成')
                        dtd.resolve()
                    }
                    setTimeout(task, 2000)
                    return dtd.promise()  // 注意，这里返回的是 primise 而不是直接返回 deferred 对象
                }
                return wait(dtd)
            }

            var w = waitHandle() // 经过上面的改动，w 接收的就是一个 promise 对象
            $.when(w)
            .then(function () {
                console.log('ok 1')
            })
            .then(function () {
                console.log('ok 2')
            })
        */