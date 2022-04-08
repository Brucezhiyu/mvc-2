# 《浅析 MVC》
本文章初步介绍了MVC的功能和作用，方便后续学习框架做好铺垫。\
本文内容:
##### 1. MVC 三个对象分别做什么
##### 2. EventBus 有哪些 API，是做什么用的
##### 3. 表驱动编程是做什么的
##### 4. 我是如何理解模块化的
## MVC 三个对象分别做什么
MVC 是软件工程中一种经典设计模式。该模式包含三个基本部分：
1. M：Model。模型用于封装数据，一般包含数据本身和处理数据的方法。
```js
Model = {
  data: { 该模块使用到的数据 },
  create(){ 增加数据 },
  delete(){ 删除数据 },
  update(){ 更新数据 },
  get(){ 获取数据 }
}
```
2. V：View。视图负责用户界面，UI 交互 和 HTML 渲染。
```js
View = {
  el: 需要刷新的元素,
  html: ` html 内容 `,
  init(){ 初始化页面 },
  render(){ 刷新页面 }
}
```
3. C：Controller。控制器处理来自视图委托的事件，当模型数据改变时控制器把新的数据传递给视图。简而言之，Controller 的主要功能是连接 View 和 Model。在前端里，控制器和试图很难分离。
```js
Controller = {
init(){
   v.init() // View初始化
   v.render() // 第一次渲染
   c.autoBindEvents() // 自动事件绑定
   eventBus.on('m:update', () => { v.render() }) // 当eventBus触发'm:update'时View刷新
},
events:{ 事件哈希表 },
method() {
   data = 执行后的新数据
   m.update(data)
},
autoBindEvents() { 根据事件哈希表自动绑定事件 }
}
```
---
## EventBus 有哪些 API，是做什么用的
EventBus是一个基于发布者/订阅者模式的事件总线框架。在EventBus中，当事件被触发，所有监听该事件的事件处理方法被调用。目前接触到的API：

EventBus.on（）监听事件。\
EventBus.trigger（）触发事件。
---
## 表驱动编程是做什么的
表驱动编程的作用主要是分离数据和逻辑代码。通常用来避免代码中大量的 if-else 和 switch-case。
```js
const controller = {
    events:{
        'click #add1':'add',
        'click #minus1':'minus',
        'click #mul2':'mul',
        'click #divide2':'div'
    },
    autoBindEvents(){
        for(let key in c.events){
            const value = c[c.events[key]]
            const spaceIndex = key.indexOf(' ')
            const part1 = key.slice(0, spaceIndex)
            const part2 = key.slice(spaceIndex + 1)
            v.el.on(part1,part2,value)
        }
    }
}

```
## 我是如何理解模块化的
模块化一般指将代码分割为若干部分，将重复的代码进行封装，想要哪个功能就单独加载什么模块。
对于初学者来说模块化只会增加代码复杂度，浪费精力。但对于有经验的程序员，模块化可以有效降低代码耦合度，减少重复代码，提高代码重用性，并且在项目结构上更加清晰，便于维护。
