title: React Native学习笔记
date: 2015-12-17 16:41:39
tags: [iOS,Android,React]
---

## 踩过的一些坑

### React 使用ES6+语法时 事件绑定疑惑

在JavaScript中经常会遇到`this`的问题，非ES6语法写React大多时候是不用担心`this`的问题，因为React自动绑定了。
>Autobinding: When creating callbacks in JavaScript, you usually need to explicitly bind a method to its instance such that the value of this is correct. With React, every method is automatically bound to its component instance. React caches the bound method such that it's extremely CPU and memory efficient. It's also less typing!

但是在ES6中就没有自动绑定了。需要自己手动绑定，使用`.bind(this)`或者使用箭头函数`=>`
>No Autobinding
Methods follow the same semantics as regular ES6 classes,meaning that they don't automatically bind `this` to the instance.You'll have to explicitly use `.bind(this)` or arrow functions `=>`


第一种

```javascript
_handleClick(e) {
    console.log(this);
}
render() {
    return (
        <div>
            <h1 onClick={this._handleClick.bind(this)}>点击</h1>
        </div>
    );
}
```
第二种

```javascript
constructor(props) {
    super(props);
    this._handleClick = this._handleClick.bind(this)
}
_handleClick(e) {
    console.log(this);
}
render() {
    return (
        <div>
            <h1 onClick={this._handleClick}>点击</h1>
        </div>
    );
}
```
第三种
```javascript
// 使用箭头函数(arrow function)
_handleClick = (e) => {
    console.log(this);
}
render() {
    return (
        <div>
            <h1 onClick={this._handleClick}>点击</h1>
        </div>
    );
}
```

### 关于`sendInputEventWithName`事件名自定义。
第一种
需要重写`customDirectEventTypes` 或者 `customBubblingEventTypes`添加自定义事件名称

第二种
```object-c
RCT_CUSTOM_VIEW_PROPERTY(自定义事件名称, RCTDirectEventBlock, 自定义Manager)
{
}
or
RCT_CUSTOM_VIEW_PROPERTY(自定义事件名称, RCTBubblingEventBlock, 自定义Manager)
{
}
```
`RCTDirectEventBlock`:直接事件,这种事件类型作为不影响UI的一些事件，比如“图片加载失败”。
`RCTBubblingEventBlock`:冒泡事件,就和操作DOM一样，影响UI的事件，比如“点击按钮事件”。

## 未完待续