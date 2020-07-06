---
title: Vue源码学习
date: 2020-07-06 14:10:53
tags:
 - 前端框架
categories: Web前端
---
### 1.0 如何监听一个对象的变化

1.Object.defineProperty方法//ES5中新增->可以自定义getter和setter函数，从而在
获取对象属性和设置对象属性的时候能够执行自定义的回调函数。
2.对象是个层次的结构，对象的某个属性可能仍是一个对象。
```bash
let data = {
    user: {
        name: "liangshaofeng",
        age: "24"
    },
    address: {
        city: "beijing"
    }
};
```
解决方案：如果对象的属性仍然是一个对象，那么使用递归算法，walk函数，继续new一个Observer
直到到达最底层的属性位置。
实现代码：
```bash
// 观察者构造函数
function Observer(data) {
    this.data = data;
    this.walk(data)
}

let p = Observer.prototype;

// 此函数用于深层次遍历对象的各个属性
// 采用的是递归的思路
// 因为我们要为对象的每一个属性绑定getter和setter
p.walk = function (obj) {
    let val;
    for (let key in obj) {
        // 这里为什么要用hasOwnProperty进行过滤呢？
        // 因为for...in 循环会把对象原型链上的所有可枚举属性都循环出来
        // 而我们想要的仅仅是这个对象本身拥有的属性，所以要这么做。
        if (obj.hasOwnProperty(key)) {
            val = obj[key];

            // 这里进行判断，如果还没有遍历到最底层，继续new Observer
            if (typeof val === 'object') {
                new Observer(val);
            }

            this.convert(key, val);
        }
    }
};

p.convert = function (key, val) {
    Object.defineProperty(this.data, key, {
        enumerable: true,
        configurable: true,
        get: function () {
            console.log('你访问了' + key);
            return val
        },
        set: function (newVal) {
            console.log('你设置了' + key);
            console.log('新的' + key + ' = ' + newVal)
            if (newVal === val) return;
            val = newVal
        }
    })
};

let data = {
    user: {
        name: "liangshaofeng",
        age: "24"
    },
    address: {
        city: "beijing"
    }
};

let app = new Observer(data);
```
1.上面的代码只监听了对象的变化，没有处理数组的变化。
2.当你重新set的属性是对象的话，那么新set的对象里面的属性不能调用getter和setter。
3.①实现observer②消息-订阅器③实现一个watcher④实现一个vue
4.①↑通过递归，给每个属性（包括子属性）加上get/set，有赋值触发set方法，发出通知，触发回调。
5.②↑watcher为订阅者，一旦触发notify函数，遍历watcher，调用update方法。
6.③↑实现watcher，内置构造函数，update函数（调用更新数据的函数），根据判断object.defineProperty的get是否有值来调用。
7.④↑实现一个vue，引入observer，引入watcher，observer自己的data，访问data的属性，watcher属性。
实现vue的代码如下：
```bash
import Watcher from '../watcher'
import {observe} from "../observer"

export default class Vue {
  constructor (options={}) {
    //这里简化了。。其实要merge
    this.$options=options
    //这里简化了。。其实要区分的
    let data = this._data=this.$options.data
    Object.keys(data).forEach(key=>this._proxy(key))
    observe(data,this)
  }


  $watch(expOrFn, cb, options){
    new Watcher(this, expOrFn, cb)
  }

  _proxy(key) {
    var self = this
    Object.defineProperty(self, key, {
      configurable: true,
      enumerable: true,
      get: function proxyGetter () {
        return self._data[key]
      },
      set: function proxySetter (val) {
        self._data[key] = val
      }
    })
  }
}
```
---

### 2.0 如何监听一个数组的变化
Vue.js包装了被观察数组的变异方法，故它们能触发视图更新。被包装的方法有：
```bash
.push()
.pop()
.shift()
.unshift()
.splice()
.sort()
.reverse()
```
Vue.js不能检测到下面数组变化：
```bash
.直接用索引设置元素，如vm.items[0] = {};
.修改数据的长度，如vm.items.length = 0。
```
修改push()函数的实现：
```bash
function FakeArray() {
    Array.call(this,arguments);
}

FakeArray.prototype = [];
FakeArray.prototype.constructor = FakeArray;

FakeArray.prototype.push = function () {
    console.log('我被改变啦');
    return Array.prototype.push.call(this,arguments);
};

let list = ['a','b','c'];

let fakeList = new FakeArray(list);
```
这里是类似一种继承，在继承了push函数的情况下多增一些方法。
构造函数默认返回的是this对象，而非数组，Array.call(this.arguments)这个返回的是数组。
如果直接return一个原生的Array出来，push函数就没达到重写了。

---

### 3.0如何实现一个watcher库
[实现watcher库请看这](https://github.com/melanke/Watch.JS)

### 4.0如何实现动态数据绑定

```bash
// html
<div id="app">
    <p>姓名:{{user.name}}</p>
    <p>年龄:{{user.age}}</p>
</div>
```
```bash
// js
const app = new Bue({
    el: '#app',
    data: {
        user: {
            name: 'youngwind',
            age: 24
        }
    }
});
```
直接遍历DOM模板把数据改成实际值替换，存在问题
①修改非DOM相关数据也会触发DOM的重新渲染。
②修改DOM相关属性会渲染更新整个DOM。

指令Directive
只更新数据变动相关的DOM，必须有个对象将DOM节点和对应的数据一一映射起来。
```bash
/**
 * 指令构造函数
 * @param name {string} 值为"text", 代表是文本节点
 * @param el {Element} 对应的DOM元素
 * @param vm {Bue} bue实例
 * @param expression {String} 指令表达式，例如 "name"
*  @param attr {String} 值为'nodeValue', 代表数据值对应的书节点的值
 * @constructor
 */
function Directive(name, el, vm, expression) {
    this.name = name;  // 指令的名称， 对于普通的文本节点来说，值为"text"
    this.el = el;              // 指令对应的DOM元素
    this.vm = vm;          // 指令所属bue实例
    this.expression = expression;       // 指令表达式，例如 "name"
    this.attr = 'nodeValue';        
    this.update();
}

// 这是指令的更新方法。当对应的数据发生改变了，就会执行这个方法
// 可以看出来，这个方法就是用来更新nodeValue的
Directive.prototype.update = function () {
    this.el[this.attr] = this.vm.$data[this.expression];
    console.log(`更新了DOM-${this.expression}`);
};
```
实现思路：遍历DOM节点时，会匹配出表达式，新建空的textNode，插入到这个节点前面，
然后remove掉这个文本节点。

存在问题：每次数据发生改变的时候，都需要循环directive，匹配表达式值才能找到指令，
效率很低。且根据$watch对应的回调函数与DOM无关，不会有el和attr。

解决思路：引入binding和watcher这两个类。
每个属性上都有一个数组，这个数组是订阅的意思，里面存放着一系列watcher，当watcher
代表数据发生改变时，会遍历数组，执行update更新。
Watcher是一个观察容器，可以装载Directive。
根据Binding（解决键值索引）、watcher（解决$watch）、Directive(更新DOM中属性值)三者实现双向数据绑定。

### 5.0批处理更新DOM
频繁操作DOM是低效率的，实现多次数据更新只更新一次DOM。

官方注：每观察到数据变化，Vue会开始一个队列，将同一事件循环内所有的数据变化
缓存起来，一个watcher多次被触发，只会推入一次到队列中。下次循环，清空队列，进行
必要的DOM更新。
解决原理：Batcher，批处理类。
```bash
/**
 * 批处理构造函数
 * @constructor
 */
function Batcher() {
    this.reset();
}

/**
 * 批处理重置
 */
Batcher.prototype.reset = function () {
    this.has = {};
    this.queue = [];
    this.waiting = false;
};

/**
 * 将事件添加到队列中
 * @param job {Watcher} watcher事件
 */
Batcher.prototype.push = function (job) {
    if (!this.has[job.id]) {
        this.queue.push(job);
        this.has[job.id] = job;
        if (!this.waiting) { // wating用于保护
            this.waiting = true;
            setTimeout(() => {
                this.flush();// 更新执行的函数
            });
        }
    }
};

/**
 * 执行并清空事件队列
 */
Batcher.prototype.flush = function () {
    this.queue.forEach((job) => {
        job.cb.call(job.ctx);
    });
    this.reset();//重置队列，保证下次能更新数据
};
```
