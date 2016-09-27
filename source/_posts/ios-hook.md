title: iOS-Hook
date: 2016-02-23 16:09:08
tags:
---


### 本文章仅作娱乐交流

### 本文章以一个app中嵌入自定义代码为示范，“破解”app;

#### 真机log

```bash
brew install libimobiledevice
brew install ideviceinstaller
```

```bash
idevicesyslog
```

#### 终端挂在app沙盒目录

```bash
brew install libimobiledevice
brew install ifuse
```


#### 手动签名
```bash
yololib PcwStore hook1.dylib 
codesign -f -s "iPhone Developer: he baizhong (LA92F7PS39)" PcwStore.app/hook1.dylib
```


## macOS App Hook

编译生成`*.dylib`
加载dylib的方法有很多，我们这里用DYLD_INSERT_LIBRARIES

打开目录/Applications/`AppName`.app/Contents/MacOS/
复制`*.dylib`到该目录下
修改`AppName`为`AppName_`
新建`AppName`，文本编辑器输入一下内容：
```bash
#!/bin/bash

UP_PATH="`dirname "${0}"`"
UP_BIN="`dirname "${0}"`"/AppName_

export DYLD_INSERT_LIBRARIES="${UP_PATH}/*.dylib"

"$UP_BIN"
```

保存，添加可执行属性
chmod 755 `AppName`

## Install iOSOpenDev
官网下载[iOSOpenDev]()
安装下载好的pkg文件是无法正常安装成功的。需要进入`/opt/`目录下有安装时生成的文件
```
sudo ./iod-setup base
sudo ./iod-setup sdk -sdk iphoneos
```
执行第二步会出现一个错误就是链接iOS 9.3的private framework失败，主要是在9.3的SDK里去掉了private framework。
```
PrivateFramework directory not found: /Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS9.3.sdk/System/Library/PrivateFrameworks
```
解决办法：
将iOS9.2 SDK中的`PrivateFrameworks`文件夹复制到`iPhoneOS9.3.sdk/System/Library/`下。

还会有其它错误
1、下载附件
附件中三个文件在xcode6里都可以找到，可以在升级xcode7前备份出来，升级xcode7以后再放回原位，也可以用我上传的，附件中文件后面的数字只做区分用，拷贝前需要删掉数字

2、把Specifications1文件夹重命名为Specifications放到/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/Xcode/

3、把Specifications2文件夹重命名为Specifications放到/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/Library/Xcode/

4、把usr3重命名为usr放到/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneSimulator.platform/Developer/


## 未完待续