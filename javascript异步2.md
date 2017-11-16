        用 promise 封装 传统的异步操作
        /*
            var wait = function () {
                var task = function () {
                    console.log('执行完成')
                }
                setTimeout(task, 2000)
            }
            wait()
        */

        /*
            const wait = function () {
                // 定义一个promise对象
                const promise = new Promise((resolve, reject) => {
                    // 将之前的异步操作, 包括到这个 new Promise函数之内
                    con task = function () {
                        console.log('执行完成')
                        resolve(); // callback 中去执行 resolve 或者 reject
                    };
                    setTimeout(task, 2000)
                });
                // 返回 promise对象
                return promise;
            }
        */
        /*
            const w = wait()
            w.then(() => {
                console.log('ok 1')
            }, () => {
                console.log('err 1')
            }).then(() => {
                console.log('ok 2')
            }, () => {
                console.log('err 2')
            })
            then还是和之前一样，接收两个参数（函数），
            第一个在成功时（触发resolve）执行，
            第二个在失败时(触发reject)时执行。
            而且，then还可以进行链式操作。
        */

        /*
            因为以下所有的代码都会用到Promise，因此干脆在所有介绍之前，先封装一个Promise，封装一次，为下面多次应用。

            const fs = require('fs')
            const path = require('path')  // 后面获取文件路径时候会用到
            const readFilePromise = function (fileName) {
                return new Promise((resolve, reject) => {
                    fs.readFile(fileName, (err, data) => {
                        if (err) {
                            reject(err)  // 注意，这里执行 reject 是传递了参数，后面会有地方接收到这个参数
                        } else {
                            resolve(data.toString())  // 注意，这里执行 resolve 时传递了参数，后面会有地方接收到这个参数
                        }
                    })
                })
            }
        */

        /*
            const fullFileName = path.resolve(__dirname, '../data/data2.json')
            const result = readFilePromise(fullFileName)
            result.then(data => {
                console.log(data)
            })
        */
        /*
            再加一个需求，在打印出文件内容之后，我还想看看a属性的值，代码如下。
            之前我们已经知道then可以执行链式操作，
            如果then有多步骤的操作，那么前面步骤return的值会被当做参数传递给后面步骤的函数，
            如下面代码中的a就接收到了return JSON.parse(data).a的值

            const fullFileName = path.resolve(__dirname, '../data/data2.json')
            const result = readFilePromise(fullFileName)
            result.then(data => {
                // 第一步操作
                console.log(data)
                return JSON.parse(data).a  // 这里将 a 属性的值 return
            }).then(a => {
                // 第二步操作
                console.log(a)  // 这里可以获取上一步 return 过来的值
            })
            总结一下，这一段内容提到的“参数传递”其实有两个方面：

            执行resolve传递的值，会被第一个then处理时接收到
            如果then有链式操作，前面步骤返回的值，会被后面的步骤获取到
        */

        /*
            对于Promise中的异常处理，我们建议用catch方法，而不是then的第二个参数。请看下面的代码，以及注释。

            const fullFileName = path.resolve(__dirname, '../data/data2.json')
            const result = readFilePromise(fullFileName)
            result.then(data => {
                console.log(data)
                return JSON.parse(data).a
            }).then(a => {
                console.log(a)
            }).catch(err => {
                console.log(err.stack)  // 这里的 catch 就能捕获 readFilePromise 中触发的 reject ，而且能接收 reject 传递的参数
            })

            在若干个then串联之后，我们一般会在最后跟一个.catch来捕获异常，
            而且执行reject时传递的参数也会在catch中获取到。这样做的好处是：

            让程序看起来更加简洁，是一个串联的关系，没有分支（如果用then的两个参数，就会出现分支，影响阅读）
            看起来更像是try - catch的样子，更易理解
        */

        /*
            如果现在有一个需求：先读取data2.json的内容，当成功之后，再去读取data1.json。
            这样的需求，如果用传统的callback去实现，会变得很麻烦。
            而且，现在只是两个文件，如果是十几个文件这样做，写出来的代码就没法看了（臭名昭著的callback-hell）。
            但是用刚刚学到的Promise就可以轻松胜任这项工作
            
            const fullFileName2 = path.resolve(__dirname, '../data/data2.json')
            const result2 = readFilePromise(fullFileName2)
            const fullFileName1 = path.resolve(__dirname, '../data/data1.json')
            const result1 = readFilePromise(fullFileName1)

            result2.then(data => {
                console.log('data2.json', data)
                return result1  // 此处只需返回读取 data1.json 的 Promise 即可
            }).then(data => {
                console.log('data1.json', data) // data 即可接收到 data1.json 的内容
            })
            上文“参数传递”提到过，如果then有链式操作，前面步骤返回的值，会被后面的步骤获取到。
            但是，如果前面步骤返回值是一个Promise的话，情况就不一样了 ———— 如果前面返回的是Promise对象，
            后面的then将会被当做这个返回的Promise的第一个then来对待 ———— 如果你这句话看不懂，
            你需要将“参数传递”的示例代码和这里的示例代码联合起来对比着看，然后体会这句话的意思。
        */

        /*
            Promise.all和Promise.race的应用

            我还得继续提出更加奇葩的需求，以演示Promise的各个常用功能。如下需求：
            读取两个文件data1.json和data2.json，现在我需要一起读取这两个文件，
            等待它们全部都被读取完，再做下一步的操作。此时需要用到Promise.all

            // Promise.all 接收一个包含多个 promise 对象的数组
            Promise.all([result1, result2]).then(datas => {
                // 接收到的 datas 是一个数组，依次包含了多个 promise 返回的内容
                console.log(datas[0])
                console.log(datas[1])
            })

            读取两个文件data1.json和data2.json，现在我需要一起读取这两个文件，但是只要有一个已经读取了，就可以进行下一步的操作。此时需要用到Promise.race

            // Promise.race 接收一个包含多个 promise 对象的数组
            Promise.race([result1, result2]).then(data => {
                // data 即最先执行完成的 promise 的返回值
                console.log(data)
            })

            Promise.resolve的应用

            实际上，并不是Promise.resolve对 jquery 的deferred对象做了特殊处理，而是Promise.resolve能够将thenable对象转换为Promise对象。什么是thenable对象？———— 看个例子

            // 定义一个 thenable 对象
            const thenable = {
                // 所谓 thenable 对象，就是具有 then 属性，而且属性值是如下格式函数的对象
                then: (resolve, reject) => {
                    resolve(200)
                }
            }

            // thenable 对象可以转换为 Promise 对象
            const promise = Promise.resolve(thenable)
            promise.then(data => {
                // ...
            })
            上面的代码就将一个thenalbe对象转换为一个Promise对象，只不过这里没有异步操作，所有的都会同步执行，但是不会报错的。
        */