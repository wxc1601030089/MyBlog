---
title: Vue开发
date: 2020-05-26 14:50
tags:
 - 前端框架
categories: Web前端
---
This is my vue develop exprience~
---
## 2020/5/7 16:02 Write

过往项目经验总结

关键词：Vue，CSS，SVN，WebPack，IconFont，Flex，Jeecg，Iview，ElementUI

```bash
使用Vue开发页面，环境配置，导入包，Package，使用IView组件进行页面开发。
（使用开源组件一定需要查看开发文档！！！！！！）
CSS排版布局，flex学习并使用，各种定位的深入了解。
router路由在Vue当中的使用，熟练使用组件进行开发。
熟练使用SVN进行提交和更新代码，提交需要带钩子，如果本地有更改过的文件，在更新时需要将有改动的文件删除再更新，否则会出现乱码
父子组件间的传值通信，Props的使用，对话框的熟练使用。
分析需求并设计页面，使用Axure进行页面的设计，使用开源的移动端组件和阿里的IconFont，必要时可以自行设计组件。
熟悉WebPack框架和Cli脚手架。
```
---

```bash
使用ant-design-jeecg-vue进行开发，代码自动生成。文档链接：[jeecg开发文档]("http://doc.jeecg.com/1273969")
该框架生成的代码由ElementUI组件组成。
考虑页面的逻辑和合理性。
弹框嵌套，需要注意层级。
```

---


## 2020/5/7 10:33 Write

使用Vue进行对应页面开发

```bash
使用elementUI进行页面的开发，使用Upload上传文件，自带判断文件后缀。
使用POST方法，带FormData参数，使用Append添加序列，需要New一个FormData对象，然后进行传参。
使用Dialog组件，显示设备详情，点击跳出，准备参析路由。
GET方法带的参数只有Params对象。跟在URL后面。
POST和GET不同：//都为TCP链接，并无差别。
参数格式不同以外，参数的大小也不同。因为服务器的处理方式不同，不一定能接受到数据（request body的数据）。
由于HTTP的规定和浏览器/服务器的限制，应用过程体型出不同。
GET产生一个TCP数据包，POST产生两个TCP数据包。
GET，header和data一起发过去，响应200。（返回数据）
POST，先发送header，服务器响应continue，再发送data，响应200。（ 返回数据）
```

---

## 2020/5/7 15:53 Write

使用Element组件遇到的问题~

```bash
使用elementUI的UpLoad组件上传xls文件，需要注意文件格式和文件获取
组件自带上传，可以设置header和数据的name，需要注意Token或者Cookie
组件获取文件的方式   this.$refs.{upload}（这是关联upload组件的ref名称）.$data.uploadFiles
注意dialog组件的visible显示值设置
详细查看开发文档！！！！！必要时进行测试获取信息！！
```

---

## 2020/5/8 9:38 Write

使用SVN合并代码，并开发模式遇到问题

```bash
将文件checkout在其他文件夹里，然后修改原代码文件。
多注意NetWork中请求的格式，请求的参数格式！！！！
其中GET方式的URL会随参数改变，POST方式不会。
使用POSTMan测试。
接口文档管理有使用到SWAGGER和小幺鸡~
```

---

## 2020/5/9 9:53 Write

总结工作经验，编写简历

---

```bash
Vue的两种路由模式：Hash和History模式
hash：默认模式
常用NewURL和OldURL来改变链接，动态页面数据。hash值发生变化的时候，会触发hashchange这个时间，通过该事件监听hashchange来实现更新页面部分内容。
History模式：
切换历史状态，back，forward，go三个方法，刷新页面会请求服务器。History模式需要避免刷新。刷新时如果没有相应的响应或者资源，会刷新出404。
```
---

```bash
同事开发地图，使用蜂鸟地图插件。经过询问得知，正在研究。
```
---
```bash
回温Vue文档。查看生命周期。
beforeCreate()->created()->beforeMount()->mounted()->beforeUpdate()->updated()->beforeDestroy()->destoryed()//函数调用顺序，中间各有过程
周期如下图
```
![vue](vue.png)
```bash
在使用ElementUI的Upload上传文件时，action的动态响应是在上传后执行的，所以需要给上传方法设置延时器，并且用到this上下文的时候，需要将this传到该函数内使用，否则会报错。
上传可以设置header的值，在初始化的时候获取token：this.header = {token:`${this.$cookie.get('token')}`}
设置的header是一个对象，给header的token属性赋值，要求上传成功后自动关闭对话框，然后清除选择文件数据：this.$refs.upload.clearFiles();
需要考虑功能全面，合理
```
---
```bash
vue中数据异步渲染，数据响应式为Vue 的重要核心，在数据变更后，可能DOM还未更新，所以可以调用nextTick（callback）函数，这样回调函数将在DOM更新完后被调用。并且this会绑定到当前的vue实例上。
同域：域名、端口、协议均相同，缺一不可。
跨域：浏览器从一个域名的网页去请求另一个域名的资源时，不同域，就是跨域。//可能会被另一个域保存Cookie或session用来对同域网站发起非法操作，为CSRF攻击，盗用身份。
解决方法：JSONP，CORS。//https://www.jianshu.com/p/f880878c1398(参考)
使用FileZilla Client进行网页部署，连接到服务器，将右边框内dist文件删除，从左边框内选择本体文件，然后右键上传。
```
---

```bash
Vue加载本地图片失败，无法使用相对路径。
//使用绝对路径 :src="'https://fuss10.elemecdn.com/e/5d/4a731a90594a4af544c0c25941171jpeg.jpeg'"
//使用require :src="require('@/assets/images/icon/file/jpg.png')"
//使用import   import jpg from '@/assets/images/icon/file/jpg.png'
Vue使用vue-awesome
//执行指令 npm install vue-awesome
//在main.js文件下 import 'vue-awesome' || import Icon from 'vue-awesome/components/Icon'
//Vue.component('icon',Icon)  然后正常使用  <icon name=”beer”></icon>
Vue使用Iconfont
//选择Icon加入购物车，并且下载源码
//执行命令npm install css-loader -D(否则会报错）
//在main.js文件中引入 import '@/assets/Iconfont/iconfont.css'!!!!!注意这里使用@，绝对路径
//直接使用<i class="iconfont icon-MV" style="color: white"></i>  在iconfont.json文件中查看各个icon的fontclass
Vue行内写hover样式
//onmouseover="this.style.color='#57EDED'"!!由于无法实现hover，使用js事件实现，并且在参数边要加引号''
//改变光标形状为小手:cursor='pointer'
Vue点击跳转链接
//如果不写协议，会自动在链接前加上localhost:8080(相当于一个相对路径)
CSS3 animaiton 实现图片旋转
//animation: 15s linear 0s normal none infinite rotate;
//animation-play-state: running;
Vue导入本地音频文件
//文件放入static文件里，无需改变任何Webpack配置，直接引入
//引入写法：mp3Url:"static/attack音D.mp3"
Vue js将小数转为整数值
//toFixed(2) 后面的数值代表保留两位小数
Vue 将字符串转化为数值
//parseInt(str) 如果是浮点数，将Int改成float即可
Vue 实现进度条
// element UI 放置 el-progress   :percentage="mp3width" 设置percentage为数值变量
// 实现函数LoadProgress 通过setInterval改变mp3width的值（浮点数）
//使用el-slide,可以实现点击和拖拽
//需要注意函数运行速度，存在未取到值而后直接进行计算
Vue注意事项
//开发到后发现，需要通过数据关联来通过子组件控制父组件。
//还有一些加载速度，值得优化和注意。
Vue遇到组件通信问题
//尽量减少组件层数，如果在同一层父组件时，子组件可以一起和父组件通信，获得数据共享。
```
---
```bash
深入响应式原理，vue的核心。
当一个对象在实例化中的data选项，Vue将遍历此对象所有的property，转为getter和setter。
每个组件实例都对应有一个watcher实例，在渲染过程中把数据property记录为依赖，之后当setter触发时，会通知watcher，使关联的组件重新渲染。
注意：当数据在data选项外时，为非响应式！！！
数组的长度和直接利用索引直接设置的数据都不是响应性的，这时可以使用set方法，使用index和数组的子项更新数据。
vue提前声明所有的响应式property。
Vue在更新DOM时是异步执行的，侦听到数据变化时，将开启一个队列，缓冲在同一事件循环中发生的所有数据变更。
使用原生html，js，css开发文书模板。
body{padding:0px;margin:0px} //适应浏览器，无白边
使用占位符，空格（&nbsp），后用正则表达式替换。
text-align:justify;// 文字两端对齐
margin:0 auto;// 左右对齐
white-space:normal// 文字换行,元素空白处理
div的使用和flex的合理使用
```
---
```bash
项目路线图：
项目评估确认阶段
项目立项
需求开发
设计阶段
研发及生产
测试及缺陷修复
生产环境实施部署
系统验收
项目结项
~~~以上为项目完整流程。
```
### 持续进行编写和开发，经验保存
``` bash
 后续将CSDN博客中的经验总结并转移，并学习更多HEXO使用方式和编写方式
```