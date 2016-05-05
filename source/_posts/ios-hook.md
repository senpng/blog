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

## 未完待续