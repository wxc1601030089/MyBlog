---
title: 生成器和promise
date: 2020-06-23 14:02:25
tags:
 - JavaScript
categories: Web前端
---
## 生成器，promise的使用和原理
```bash
使用try catch的问题：
JS依赖于单线程执行模型，在操作结束前，无法渲染数据，用户会不满。
解决办法：
可以使用回调函数解决这个问题，这样每个数据获取后调用回调函数，不会导致UI渲染暂停。
```
```bash
function* name(){} //在关键字function后面添加星号*定义生成器函数
yield "****" //使用yield函数生成独立的值
调用生成器会创建一个叫作迭代器的对象，与生成器通信。
const nameIterator = name() //调用生成器，得到一个迭代器，控制生成器的执行。
const result = nameIterator.next() //next将返回一个对象，包含返回值，以及一个指示器(done值)告诉我们是否还会生成值,返回undefined时代表状态已完成(done:true)
每生成一个值后生成器会非阻塞地挂起执行，等待下一个值请求的到达。
for-of循环的原理如上，不同于next方法，for-of循环同时还需要查看生成器是否完成，它在后台自动做了完全相同的工作。
function* WarriorGenerator(){
   yield "Sun Tzu";
   yield* NinjaGenerator();// yield*将执行权交给了另一个生成器，会先执行NinjaGenerator
   yield "Genghis Khan"; // 执行完 NinjaGenerator后继续执行yield
}  // 生成（输出）顺序为Sun Tzu,Hatori,Yoshi,Genghis Khan.

function* NinjaGenerator(){
   yield "Hatori";
   yield "Yoshi";
}
生成器当中写无限循环的代码是可行的，每次调用一次next方法，while循环才会迭代一次并返回下一个ID值。
```

```bash
还可以与生成器交互，在next函数中传入值，作为yield函数的返回值，第一次next无法提供初始值，可调用生成器自身的函数传入参数。
也可使用try catch块，通过迭代器上有效的throw方法抛出异常，在catch函数中获取throw的值进行操作。
生成器内部构成（像一个小程序，在状态中运动的状态机）
步骤：挂起开始，执行，挂起让渡，完成。
与闭包相似，每次使用函数就会创建一个上下文，生成器的上下文会被挂起来后恢复使。保持引用就不会被销毁。
如果代码执行到return的时候，会进入完成状态。
```

```bash
promise有两个状态，resolve和reject，resolve->fulfilled状态，reject->Rejected状态，都为已完成状态。
promise案例的真实使用，通过Ajax请求数据，成功状态则处理数据，处理成功进入已完成状态，使用数据。
请求失败则使用onerror函数调用reject回调函数，拒绝该请求。
可以使用promise.all等待多个promise，then函数后有一个result参数，为一个数组，每一项都对应promise数组中的对应性。
多个promise中有一个失败，整个对象都会被拒绝。使用promise.race函数传入一个promise数组，会返回一个新promise对象，某个被拒绝就这个promise就被拒绝。
```