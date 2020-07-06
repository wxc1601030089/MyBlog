---
title: ES6语法知识点
date: 2020-05-13 14:01:50
tags:
 - 语法知识
 - JavaScript
categories: Web前端
---
学习ES6语法记录知识点
---

##2020/5/13 14:03 Write

关键词：JavaScript

## 1.let和const命令

```bash
ES6新增的let命令，用法类似于Var，let声明的变量只在let命令所在的代码块有效（作用域问题）。
let要先声明再调用，否则系统会报错。
For循环当中的i就很适合使用let命令，For循环当中设置变量那一块的作用域与循环体内部的作用域不同，相当于父子作用域。
var命令声明的变量在脚本运行时就存在，所以先调用对象不会报错，但是显示undefined。
let命令会绑定这个块级作用域，在声明之前无法改变变量的值。（暂时性死区）
let在相同作用域内不允许重复声明一个变量。
注意块级作用域的问题，{{}}=>为两层作用域。
ES6中块级作用域声明的函数类似于Var，提升到全局作用域或函数作用域的头部。
const声明一个只读的常量，声明后无法改变，声明需要赋值，否则会报错。
const的作用域与let命令相同，同样存在暂时性死区，而且也无法重复声明。
const声明的变量的值并不是不得改动，变量指向那个内存地址所保存的数据不得改动。
const a = [];
a.push('Hello'); // 可执行
a.length = 0;    // 可执行
a = ['Dave'];    // 报错
上面代码中，常量a是一个数组，这个数组本身是可写的，但是如果将另一个数组赋值给a，就会报错
将对象冻结可以使用Object.freeze方法。
ES6声明变量有六种方法：
var，function，let，const，import，class
ES6中，let，const，class命令声明的全局变量不属于顶层对象的属性，全局变量将逐步与顶层对象的属性脱钩。
window.a(顶层变量) window->浏览器窗口对象
任何环境下，globalThis都是存在的，都可以从它拿到顶层对象，指向全局环境下的this。
```

## 2.变量的解构赋值
```bash
let [foo, [[bar], baz]] = [1, [[2], 3]];
foo // 1
bar // 2
baz // 3

let [ , , third] = ["foo", "bar", "baz"];
third // "baz"

let [x, , y] = [1, 2, 3];
x // 1
y // 3

let [head, ...tail] = [1, 2, 3, 4];
head // 1
tail // [2, 3, 4]

let [x, y, ...z] = ['a'];
x // "a"
y // undefined
z // []
以上为解构赋值的例子
null≠undefined（严格）
let { foo, bar } = { foo: 'aaa', bar: 'bbb' };
foo // "aaa"
bar // "bbb"
解构也可用于对象，字符串，数值和布尔值，函数参数
注意圆括号问题
用途：1.交换变量的值，2.函数返回多个值，3.函数参数的定义,4.提取JSON数据,5.函数参数的默认值，6.遍历Map解构，7.输入模块的指定方法
```

## 3.字符串的扩展
```bash
Unicode表示法
"\u0061"
// "a"
但是，这种表示法只限于码点在\u0000~\uFFFF之间的字符。超出这个范围的字符，必须用两个双字节的形式表示

新增字符串的遍历器接口
U+005C：反斜杠（reverse solidus)
U+000D：回车（carriage return）
U+2028：行分隔符（line separator）
U+2029：段分隔符（paragraph separator）
U+000A：换行符（line feed）

JSON.stringfy()的改造
模板字符串，用反引号（`）标识，可以当做普通字符串使用，也可以定义多行字符串，或者在字符串中嵌入变量。
如果在模板字符串中需要使用反引号，则前面要用反斜杠转义。
模板字符串中嵌入变量，需要将变量名写在${}之中。大括号内部可以放入任意的 JavaScript 表达式，可以进行运算，以及引用对象属性。
模板字符串之中还能调用函数。
tag函数的使用，第一个参数是一个数组。
tag可以用于字符转义，多语言转换。
```

## 4.字符串的新增方法
```bash
String.fromCodePoint(0x78, 0x1f680, 0x79) === 'x\uD83D\uDE80y',可以识别大于0xFFFF的字符，与codePointAt()方法相反。
String.raw()用于模板字符串的处理方法。将所有变量替换，对斜杠进行转义。
String.codePointAt()方法会正确返回 32 位的 UTF-16 字符的码点。对于那些两个字节储存的常规字符，它的返回结果与charCodeAt()方法相同。
normalize()方法，用来将字符的不同表示方法统一为同样的形式，这称为 Unicode 正规化。
includes()：返回布尔值，表示是否找到了参数字符串。参数多一个n时，从第n个位置直到字符结束。
startsWith()：返回布尔值，表示参数字符串是否在原字符串的头部。参数多一个n时，从第n个位置直到字符结束。
endsWith()：返回布尔值，表示参数字符串是否在原字符串的尾部。参数多一个n时，针对前n个字符。
repeat方法返回一个新字符串，表示将原字符串重复n次。参数不得为负或Infinity。
padStart()用于头部补全，padEnd()用于尾部补全。
matchAll()方法返回一个正则表达式在当前字符串的所有匹配。
trimStart()消除字符串头部的空格，trimEnd()消除尾部的空格。
```

## 5.正则的扩展
```bash
regexp构造函数，参数为字符串或正则表达式，分别返回正则表达式的修饰符和原有正则表达式的拷贝。
字符串对象共有 4 个方法，可以使用正则表达式：match()、replace()、search()和split()。
Unicode模式（再看）
新增了sticky和flags属性，分别返回（boolean）是否设置了y修饰符和正则表达式的修饰符。
后行断言~~
```
## 6.数值的扩展
```bash
八进制使用前缀0o表示
二进制使用前缀0b表示
Number.isFinite() //检查数值是否为有限的（finite），如果参数不是数值一律返回false
Number.isNaN()//检查一个值是否为NaN，如果参数类型不是NaN，一律返回false
parseInt()，parseFloat()
// ES6的写法
Number.parseInt('12.34') // 12
Number.parseFloat('123.45#') // 123.45
Number.isInteger()//整数判断,数值精度太高会误判，返回true
JavaScript 能够准确表示的整数范围在-2^53到2^53之间,Number.isSafeInteger()则是用来判断一个整数是否落在这个范围之内
Math.trunc方法用于去除一个数的小数部分，返回整数部分。Math.trunc方法用于去除一个数的小数部分，返回整数部分。对于空值和无法截取整数的值，返回NaN。
Math.sign方法用来判断一个数到底是正数、负数、还是零。对于非数值，会先将其转换为数值
参数为正数，返回+1；
参数为负数，返回-1；
参数为 0，返回0；
参数为-0，返回-0;
其他值，返回NaN。
Math.cbrt()方法用于计算一个数的立方根。
Math.hypot方法返回所有参数的平方和的平方根。
Math.expm1(x)返回 ex - 1，即Math.exp(x) - 1。
Math.log1p(x)方法返回1 + x的自然对数，即Math.log(1 + x)。如果x小于-1，返回NaN。
Math.log10(x)返回以 10 为底的x的对数。如果x小于 0，则返回 NaN。//log2通用法
```
## 7.函数的扩展
```bash
函数参数默认值写法 log（x=‘111’） //直接写在参数定义的后面，默认声明后无法再次声明，参数无法同名。
默认参数需要提供参数，否则（参数）默认一个空对象，当函数的参数是一个对象时->可以使用解构赋值传参。
传入undefined会触发默认值，null无法触发。
函数的length属性返回没有指定默认参数的个数。设置了参数默认值，初始化的时候参数有单独的作用域，初始化结束后作用域会消失。
ES6引入rest参数，形式为...变量名，用于获取函数的多余参数，不需要使用ar
guments对象了。rest是一个数组对象。
rest参数过后，不能再有其他参数，只能是最后一个，否则会报错。函数的length不包括rest。
ES2016后函数内部使用严格模式会报错
函数的Name属性返回函数的函数名，const bar = function baz() {}; bar.name // "baz"
箭头函数代码块多于一条语句，就要使用大括号将他们括起来，并使用return返回。
如果箭头函数直接返回一个对象，必须在对象外面加上括号，否则会报错。
注意点：箭头函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。箭头函数没有自己的this，他们的this指向外层。
不可以当做构造函数，不可以使用new命令，否则会抛出错误。
不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用rest参数代替。
不可以使用yield命令，因此函数箭头不用做Generator函数。
尾调用：函数的最后一步操作是调用另一个函数。//函数尾调用在每次执行时，调用帧只有一项，这将大大节省内存，优化。//Chrome和Firefox不支持
类似的还有尾递归，函数尾调用自身。ES6的尾调用只在严格模式下开启。蹦床函数？
toString方法返回函数代码本身。
ES6允许catch省略参数
```

## 8.数组的扩展
```bash
复制数组，只是复制了指针，而不是克隆一个全新的数组，改变该值会导致原值的变化。使用concat函数进行复制。
也可以使用concat进行数组的合并，不过该方法为浅拷贝。
Iterator接口的对象，遍历器，用扩展运算符（...）转为真正的数组。
Array.from（）方法用于将两类对象转为真正的数组：类似数组的对象和可遍历的对象。与数组的Map相似。
Array.of()方法用于将一组值转换为数组。Array.of() // []  ；  Array.of(undefined) // [undefined]
Array.prototype.copyWithin(target, start = 0, end = this.length)
target（必需）：从该位置开始替换数据。如果为负值，表示倒数。
start（可选）：从该位置开始读取数据，默认为 0。如果为负值，表示从末尾开始计算。
end（可选）：到该位置前停止读取数据，默认等于数组长度。如果为负值，表示从末尾开始计算。
数组实例：find（）：用于找出第一个符合条件的数组成员，findIndex（）：返回第一个符合条件的数组成员的位置，如果都不符合条件，返回-1.
fill（）用于补充空数组的初始化，可以选定位置，如果填充的类型为对象，那么赋值的是同一个内存地址的对象，改变其中的值会改变其他赋值的值。
keys()是对键名的遍历、values()是对键值的遍历，entries()是对键值对的遍历。
includes方法返回一个布尔值，表示某个数组是否包含给定的值。
Map 结构的has方法，是用来查找键名的，比如Map.prototype.has(key)、WeakMap.prototype.has(key)、Reflect.has(target, propertyKey)。
Set 结构的has方法，是用来查找值的，比如Set.prototype.has(value)、WeakSet.prototype.has(value)。
flat（）函数，用于拉平数组，如果原数组里包含数组，将子数组成员取出来添加在原来的位置。需要设置层数。flat会跳过空位。
flatMap（）参数为遍历函数，有第二个参数可以绑定遍历函数里面的this。
Array(3) // [, , ,]具有3个空位的数组
forEach(), filter(), reduce(), every() 和some()都会跳过空位。
map()会跳过空位，但会保留这个值
join()和toString()会将空位视为undefined，而undefined和null会被处理成空字符串
自带的sort（）排序不稳定
```
## 9.对象的扩展
```bash
obj['a' + 'bc'] = 123;//abc:123
// 报错
const foo = 'bar';
const bar = 'abc';
const baz = { [foo] };
// 正确
const foo = 'bar';
const baz = { [foo]: 'abc'};
有两种特殊情况：bind方法创造的函数，name属性返回bound加上原函数的名字；Function构造函数创造的函数，name属性返回anonymous。
(new Function()).name // "anonymous"

var doSomething = function() {
  // ...
};
doSomething.bind().name // "bound doSomething"
for...in循环：只遍历对象自身的和继承的可枚举的属性。
Object.keys()：返回对象自身的所有可枚举的属性的键名。
JSON.stringify()：只串行化对象自身的可枚举的属性。
Object.assign()： 忽略enumerable为false的属性，只拷贝对象自身的可枚举的属性。
ES6 又新增了另一个类似的关键字super，指向当前对象的原型对象。//只能用在对象的方法之中。
如果使用解构赋值，扩展运算符后面必须是一个变量名，而不能是一个解构赋值表达式。
扩展运算符后面可以为数组，如果是空对象，无效果。
// 等同于 {...Object(1)}
{...1} // {}不是对象，自动转为对象
如果后面是字符串，它会自动转换成一个类似数组的对象，因此返回的不是空对象。
对象的扩展运算符等同于使用Object.assign()方法。
完整克隆对象的方法：
// 写法一
const clone1 = {
  __proto__: Object.getPrototypeOf(obj),
  ...obj
};

// 写法二
const clone2 = Object.assign(
  Object.create(Object.getPrototypeOf(obj)),
  obj
);

// 写法三
const clone3 = Object.create(
  Object.getPrototypeOf(obj),
  Object.getOwnPropertyDescriptors(obj)
)
扩展运算符可以用于合并两个对象//let ab = { ...a, ...b };
链判断运算符
const firstName = (message//读取message.body.user.firstName
  && message.body//读取对象内部的某个属性，往往需要判断一下对象是否存在(也可以使用三元运算符)
  && message.body.user
  && message.body.user.firstName) || 'default';
引入了？？Null判断运算符，类似||，只有运算符左侧的值为null或undefined时，才会返回右侧的值。
```
## 10.对象的新增方法
```bash
Object.is()比较聊个值是否相同，与===的行为基本一致。
Object.is(+0, -0) // false
Object.is(NaN, NaN) // true
Object.assign()//方法用于对象的合并，将源对象的所有可枚举属性复制到目标对象。
Object.assign(target, source1, source2);//第一个参数是目标对象，后面的参数都是源对象
如果只有一个参数，会直接返回该参数。如果该参数不是对象，则会先转成对象，然后返回。
undefined和null无法转成对象，作为参数会报错。
assign方法实行的是浅拷贝。同名的属性会进行替换。
const target = { a: { b: 'c', d: 'e' } }
const source = { a: { b: 'hello' } }
Object.assign(target, source)
// { a: { b: 'hello' } }
数组处理，assign把数组视为属性名为0,1,2的对象，因此源数组的0号属性4覆盖了目标数组的0号属性1
Object.assign([1, 2, 3], [4, 5])
// [4, 5, 3]
如果复制的值是一个取值函数，那么会求值后再复制。
const source = {
  get foo() { return 1 }
};
const target = {};

Object.assign(target, source)
// { foo: 1 }
assign常见用途：
（1）为对象添加属性
（2）为对象添加方法
（3）克隆对象
（4）合并对象
（5）为属性指定默认值
Object.keys()//返回一个数组，成员是参数对象自身的所有可遍历属性的键名。
Object.values()//返回一个数组，成员是参数对象自身的所有可遍历属性的键值。
Object.entries()//返回一个数组，成员是参数对象自身的所有可遍历属性的键值对数组。
Object.fromEntries()方法是Object.entries()的逆操作，用于将一个键值对数组转为对象
```
## 11.symbol
```bash
symbol值通过Symbol函数生成。Symbol函数前不能使用New命令，否则会报错。因为Symbol是一个原始类型的值，不是对象。
Symbol函数的参数只是表示对当前Symbol值的描述，因此相同参数的Symbol函数的返回值是不相等的。
Symbol值不能与其他类型进行运算，会报错。Symbol可以显式转为字符串，也可以转为布尔值，但不能转为数值。
实例：消除魔术字符串
Object.getOwnPropertySymbols()方法，获取指定对象的所有Symbol属性名。返回一个数组。
Reflect.ownKeys()方法可以返回所有类型的键名，包括常规键名和 Symbol 键名。
```
参考文档：https://es6.ruanyifeng.com/