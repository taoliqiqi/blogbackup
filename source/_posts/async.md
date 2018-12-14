---
title: 8张图看清 async、await 和 promise 的执行顺序
date: 2018-12-14 09:27:04
tags:
	- 面试
categories: "async"
---
>[原文链接](https://mp.weixin.qq.com/s?__biz=MzA5NzkwNDk3MQ==&mid=2650588676&idx=1&sn=dfbc649186ac49d37a2febb280d441d3&chksm=8891d620bfe65f36b2be7166499da5f769fd2ee1b6d9d7e267a955707ffd6c5d62d859fb7450&mpshare=1&scene=1&srcid=1212JLhO2ohKJjTzDCE2ZYyJ&key=13671e9bf917d50185994fbfe3db645c7ebf4667a381b88709227d671c9fa472c02ce8132503537a7eca1fda92f2ca58ea925e929530cc8f71b7f7dd59e434c3b14f72cf17a8811a816255b7066693c3&ascene=0&uin=MTA3MzgwOTg2Mg%3D%3D&devicetype=iMac+MacBookPro12%2C1+OSX+OSX+10.14+build(18A391)&version=12020110&nettype=WIFI&lang=zh_CN&fontScale=100&pass_ticket=2gQlwVbgFZZtQg1qDP%2BTWRrdcEviXdK2Tg9cpnyg4lw%3D)
## 测试一下自己有没有必要看
### 先来一道烂大街的「今日头条」的面试题 
```javascript
    async function async1() {
        console.log( 'async1 start' )
        await async2()
        console.log( 'async1 end' )
    }
    
    async function async2() {
        console.log( 'async2' )
    }

    console.log( 'script start' )

    setTimeout( function () {
        console.log( 'setTimeout' )
    }, 0 )

    async1();

    new Promise( function ( resolve ) {
        console.log( 'promise1' )
        resolve();
    } ).then( function () {
        console.log( 'promise2' )
    } )
    
    console.log( 'script end' )
    
    
    // script start
    // async1 start
    // async2
    // promise1
    // script end
    // promise2
    // async1 end
    // setTimeout
```
##  需要具备的前置知识
1. promise的使用经验
2. 浏览器端的eventloop

>不过如果是对 ES7 的 async 不太熟悉，是没关系的哈，因为这篇文章会详解 async。
那么如果不具备这些知识呢，推荐几篇我觉得讲得比较清楚的文章

>https://segmentfault.com/a/1190000012806637 这是我之前写的讲解eventloop的文章，我觉得还算清晰，但是没涉及 async。
https://segmentfault.com/a/1190000007535316 这是我读过的讲async await最清楚的文章
http://es6.ruanyifeng.com/#docs/promise promise就推荐阮一峰老师的ES6吧，不过不熟悉 promise 的应该较少啦。

## 1.对于async await的理解
我推荐的那篇文章，对 async/await 讲得更详细。不过我希望自己能更加精炼的帮你理解它们

这部分，主要会讲解 3 点内容
1. async 做一件什么事情？
2. await 在等什么？
3. await 等到之后，做了一件什么事情？
4. 补充: async/await 比 promise有哪些优势？（回头补充）

### 1.1 async 做一件什么事情？
>一句话概括： 带 async 关键字的函数，它使得你的函数的返回值必定是 promise 对象

也就是:
如果async关键字函数返回的不是promise，会自动用Promise.resolve()包装；

如果async关键字函数显式地返回promise，那就以你返回的promise为准；

这是一个简单的例子，可以看到 async 关键字函数和普通函数的返回值的区别。



```javascript
async function fn1(){
    return 123
}

function fn2(){
    return 123
}
console.log(fn1())

console.log(fn2())

Promise {<resolved>: 123}

// 123
```

所以你看，async 函数也没啥了不起的，以后看到带有 async 关键字的函数也不用慌张，你就想它无非就是把return值包装了一下，其他就跟普通函数一样。

关于async关键字还有那些要注意的？

- 在语义上要理解，async表示函数内部有异步操作
- 另外注意，一般 await 关键字要在 async 关键字函数的内部，await 写在外面会报错。

### 1.2 await 在等什么？
>一句话概括： await等的是右侧「表达式」的结果

也就是说，

右侧如果是函数，那么函数的return值就是「表达式的结果」

右侧如果是一个 'hello' 或者什么值，那表达式的结果就是 'hello'

```javascript
async function async1() {
    console.log( 'async1 start' )
    await async2()
    console.log( 'async1 end' )
}

async function async2() {
    console.log( 'async2' )
}

async1()

console.log( 'script start' )
```

这里注意一点，可能大家都知道await会让出线程，阻塞后面的代码，那么上面例子中， 'async2' 和 'script start' 谁先打印呢？

是从左向右执行，一旦碰到await直接跳出, 阻塞async2()的执行？

还是从右向左，先执行async2后，发现有await关键字，于是让出线程，阻塞代码呢？

**实践的结论是，从右向左的。先打印async2，后打印的script start**

之所以提一嘴，是因为我经常看到这样的说法，「一旦遇到await就立刻让出线程，阻塞后面的代码」

这样的说法，会让我误以为，await后面那个函数， async2()也直接被阻塞呢。

### 1.3 await 等到之后，做了一件什么事情？
那么右侧表达式的结果，就是await要等的东西。

等到之后，对于await来说，分2个情况

- 不是promise对象
- 是promise对象

**如果不是 promise , await会阻塞后面的代码，先执行async外面的同步代码，同步代码执行完，再回到async内部，把这个非promise的东西，作为 await表达式的结果**

**如果它等到的是一个 promise 对象，await 也会暂停async后面的代码，先执行async外面的同步代码，等着 Promise 对象 fulfilled，然后把 resolve 的参数作为 await 表达式的运算结果。**

## 2. 画图一步步看清宏任务、微任务的执行过程
我们以开篇的经典面试题为例，分析这个例子中的宏任务和微任务。
```javascript
    async function async1() {
        console.log( 'async1 start' )
        await async2()
        console.log( 'async1 end' )
    }
    
    async function async2() {
        console.log( 'async2' )
    }

    console.log( 'script start' )

    setTimeout( function () {
        console.log( 'setTimeout' )
    }, 0 )

    async1();

    new Promise( function ( resolve ) {
        console.log( 'promise1' )
        resolve();
    } ).then( function () {
        console.log( 'promise2' )
    } )
    
    console.log( 'script end' )
```
先分享一个我个人理解的宏任务和微任务的慨念，在我脑海中宏任务和为微任务如图所示

![image](https://mmbiz.qpic.cn/mmbiz_png/MpGQUHiaib4ib4nGx0JnagiaV52kAiaYNQiaSsJF1Y7lG5K4oTHAFwPa3d9j9F5anmmOfXWaqcxoXaqiaeEzn3v6HFXvA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

也就是「宏任务」、「微任务」都是队列。

一段代码执行时，会先执行宏任务中的同步代码，

- 如果执行中遇到setTimeout之类宏任务，那么就把这个setTimeout内部的函数推入「宏任务的队列」中，下一轮宏任务执行时调用。
- 如果执行中遇到promise.then()之类的微任务，就会推入到「当前宏任务的微任务队列」中，在本轮宏任务的同步代码执行都完成后，依次执行所有的微任务1、2、3

下面就以面试题为例子，分析这段代码的执行顺序.

每次宏任务和微任务发生变化，我都会画一个图来表示他们的变化。

>*直接打印同步代码 console.log('script start')*

首先是2个函数声明，虽然有async关键字，但不是调用我们就不看。然后首先是打印同步代码 console.log('script start')

![image](https://mmbiz.qpic.cn/mmbiz_png/MpGQUHiaib4ib4nGx0JnagiaV52kAiaYNQiaSsRjibib0s3qrAiaCDnmsSpdic7gf2CicRIXO7yhibAVNCHmyUBwA58dFric6SA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

>*将setTimeout放入宏任务队列*

默认所包裹的代码，其实可以理解为是第一个宏任务，所以这里是宏任务2

![image](https://mmbiz.qpic.cn/mmbiz_png/MpGQUHiaib4ib4nGx0JnagiaV52kAiaYNQiaSsSu64jd9OPiaR3zmn9EszLctO87gKtEbOdIl6Ujrpn6SY8GCbjHda2nQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

>*调用async1，打印 同步代码 console.log( 'async1 start' )*

我们说过看到带有async关键字的函数，不用害怕，它的仅仅是把return值包装成了promise，其他并没有什么不同的地方。所以就很普通的打印 console.log( 'async1 start' )

![image](https://mmbiz.qpic.cn/mmbiz_png/MpGQUHiaib4ib4nGx0JnagiaV52kAiaYNQiaSs62AFb3BL8coK4pxFYsFPc8AEdSD87FykPr2UPMLBhMZCB88uPjcgiaA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

>*分析一下 await async2()*

前文提过await，1.它先计算出右侧的结果，2.然后看到await后，中断async函数

- 先得到await右侧表达式的结果。执行async2()，打印同步代码console.log('async2'), 并且return Promise.resolve(undefined)
- await后，中断async函数，先执行async外的同步代码

目前就直接打印 console.log('async2')

![image](https://mmbiz.qpic.cn/mmbiz_png/MpGQUHiaib4ib4nGx0JnagiaV52kAiaYNQiaSsIFW9yPWxuhFfuwnE5O82uRZ6v340iaukUcQ4Vnt6xDaqHAO7P2qiaoSw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

被阻塞后，要执行async之外的代码。

>*执行new Promise()，Promise构造函数是直接调用的同步代码，所以 console.log( 'promise1' )*

![image](https://mmbiz.qpic.cn/mmbiz_png/MpGQUHiaib4ib4nGx0JnagiaV52kAiaYNQiaSsjop1A0qnfSfiaoXeuDucE9ju4FglpjGutta6J44PYyibmL9iasPlDAZaw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

>*代码运行到promise.then()*

代码运行到promise.then()，发现这个是微任务，所以暂时不打印，只是推入当前宏任务的微任务队列中。

**注意：这里只是把promise2推入微任务队列，并没有执行。微任务会在当前宏任务的同步代码执行完毕，才会依次执行**

![image](https://mmbiz.qpic.cn/mmbiz_png/MpGQUHiaib4ib4nGx0JnagiaV52kAiaYNQiaSsPgSY43oPjNYhKJNdDxr2sZfhNsa20sHeEUuTKY3MyxVYJewJCwG4Sw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

>*打印同步代码 console.log( 'script end' )*

没什么好说的。执行完这个同步代码后，「async外的代码」终于走了一遍

下面该回到 await 表达式那里，执行await Promise.resolve(undefined)了。

![image](https://mmbiz.qpic.cn/mmbiz_png/MpGQUHiaib4ib4nGx0JnagiaV52kAiaYNQiaSstAL7PDqbwgNZ8F08gKwwlGiaahPxC6Jhw2KrIib1Q3BP3JpmMLRsD7MA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

>*回到async内部，执行await Promise.resolve(undefined)*

这部分可能不太好理解，我尽量表达我的想法。

对于 await Promise.resolve(undefined) 如何理解呢？

根据 MDN 原话我们知道 https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/await

**如果一个 Promise 被传递给一个 await 操作符，await 将等待 Promise 正常处理完成并返回其处理结果。**

在我们这个例子中，就是Promise.resolve(undefined)正常处理完成，并返回其处理结果。那么await async2()就算是执行结束了。

目前这个promise的状态是fulfilled，等其处理结果返回就可以执行await下面的代码了。

那何时能拿到处理结果呢？

回忆平时我们用promise，调用resolve后，何时能拿到处理结果？是不是需要在then的第一个参数里，才能拿到结果。

（调用resolve时，会把then的参数推入微任务队列，等主线程空闲时，再调用它）

所以这里的 await Promise.resolve() 就类似于

```javascript
Promise.resolve(undefined).then((undefined) => {
})
```

把then的第一个回调参数 (undefined) => {} 推入微任务队列。

then执行完，才是await async2()执行结束。

await async2()执行结束，才能继续执行后面的代码。

如图

![image](https://mmbiz.qpic.cn/mmbiz_png/MpGQUHiaib4ib4nGx0JnagiaV52kAiaYNQiaSszmFzBrCOYR0w9a2xYt9rnXxAs11bz6UkTWCH4Hb6YQVB8WDiaTLiaNEQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

此时当前宏任务1都执行完了，要处理微任务队列里的代码。

微任务队列，先进选出的原则，

- 执行微任务1，打印promise2
- 执行微任务2，没什么内容..

但是微任务2执行后，await async2()语句结束，后面的代码不再被阻塞，所以打印

console.log( 'async1 end' )

>*宏任务1执行完成后,执行宏任务2*

宏任务2的执行比较简单，就是打印

console.log('setTimeout')

==补充在不同浏览器上的测试结果==

>*谷歌浏览器，目前是版本是「版本 71.0.3578.80（正式版本） （64 位）」 Mac操作系统*
![image](https://mmbiz.qpic.cn/mmbiz_png/MpGQUHiaib4ib4nGx0JnagiaV52kAiaYNQiaSsFvIgrLCKm5a9fRR9lwfBItial37ZmAS4EtHG3NBS9oicr43FVoibBfpSA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

>*Safari浏览器的测试结果*
![image](https://mmbiz.qpic.cn/mmbiz_png/MpGQUHiaib4ib4nGx0JnagiaV52kAiaYNQiaSsfmUHcUicPJAMcbMTibGq3acnvmdY3I9JxQDcySI9LY90vXd1e4uUthnw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

>*火狐浏览器的测试结果*
![image](https://mmbiz.qpic.cn/mmbiz_png/MpGQUHiaib4ib4nGx0JnagiaV52kAiaYNQiaSss5nDwGbQl5GSqb2p9l3pSTkTs1Xo6kIqyeWbDibj8wraGdPfhKZQMUA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)