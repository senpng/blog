title: Swift学习笔记
date: 2015-12-04 22:51:04
tags: [iOS,Swift]
---

## 一些笔记
在swift中，会经常重写变量的get、set方法。
`didSet`，如果你在`init`方法中给变量赋值将不会调用`didSet`。可以使用kvc。
比如：
```swift
setValue(value, forKey: "value");
```


## 未完待续