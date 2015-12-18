title: Xcode
date: 2015-12-18 13:20:22
tags: [IDE,Xcode]
---

## 插件
### [Alcatraz](https://github.com/alcatraz/Alcatraz)（插件管理器）
![Alcatraz](/images/xcode/alcatraz.png)
#### Installation
```bash
curl -fsSL https://raw.github.com/alcatraz/Alcatraz/master/Scripts/install.sh | sh
```
#### Uninstall
```bash
rm -rf ~/Library/Application\ Support/Developer/Shared/Xcode/Plug-ins/Alcatraz.xcplugin
rm -rf ~/Library/Application\ Support/Alcatraz
```

### [SCXcodeMiniMap](https://github.com/stefanceriu/SCXcodeMiniMap)（Sublime的地图）
![SCXcodeMiniMap](/images/xcode/scxcodeminimap.png)
#### Installation
下载源码到目录`~/Library/Application Support/Developer/Shared/Xcode/Plug-ins/`,然后打开项目Build重启Xcode即可
#### Uninstall
删除`~/Library/Application Support/Developer/Shared/Xcode/Plug-ins/SCXcodeMinimap.xcplugin`

### [VVDocumenter](https://github.com/onevcat/VVDocumenter-Xcode)（注释）
![VVDocumenter](/images/xcode/vvdocumenter.gif)
#### Installation
下载源码到目录`~/Library/Application Support/Developer/Shared/Xcode/Plug-ins/`,然后打开项目Build重启Xcode即可
#### Uninstall
删除`~/Library/Application Support/Developer/Shared/Xcode/Plug-ins/VVDocumenter.xcplugin`

### [KSImageNamed](https://github.com/ksuther/KSImageNamed-Xcode)
![KSImageNamed](/images/xcode/ksimagenamed.gif)
#### Installation
下载源码到目录`~/Library/Application Support/Developer/Shared/Xcode/Plug-ins/`,然后打开项目Build重启Xcode即可
#### Uninstall
删除`~/Library/Application Support/Developer/Shared/Xcode/Plug-ins/KSImageNamed.xcplugin`

### [FuzzyAutocompletePlugin](https://github.com/FuzzyAutocomplete/FuzzyAutocompletePlugin)（代码提示）
![FuzzyAutocompletePlugin](/images/xcode/fuzzyautocompleteplugin.gif)
#### Installation
下载源码到目录`~/Library/Application Support/Developer/Shared/Xcode/Plug-ins/`,然后打开项目Build重启Xcode即可
#### Uninstall
删除`~/Library/Application Support/Developer/Shared/Xcode/Plug-ins/FuzzyAutocompletePlugin.xcplugin`

### [XAlign](https://github.com/qfish/XAlign)（格式化）
![XAlign](/images/xcode/xalign.gif)
#### Installation
下载源码到目录`~/Library/Application Support/Developer/Shared/Xcode/Plug-ins/`,然后打开项目Build重启Xcode即可
#### Uninstall
删除`~/Library/Application Support/Developer/Shared/Xcode/Plug-ins/XAlign.xcplugin`

## 升级Xcode插件失效解决办法

1.打开终端，输入以下代码获取到`DVTPlugInCompatibilityUUID`
```bash
defaults read /Applications/Xcode.app/Contents/Info DVTPlugInCompatibilityUUID
```
2.然后输入如下命令【最后一项是获取到的`DVTPlugInCompatibilityUUID`】
```bash
find ~/Library/Application\ Support/Developer/Shared/Xcode/Plug-ins -name Info.plist -maxdepth 3 | xargs -I{} defaults write {} DVTPlugInCompatibilityUUIDs -array-add 0420B86A-AA43-4792-9ED0-6FE0F2B16A13
```

## 快捷键
快捷键|描述
---|---
CMD+control+E|一次性修改一个scope里的变量名
CMD+E,CMD+G 或 shift+CMD+G|快捷搜索,先点亮想要搜索的词
CMD+option+return|切换至双窗口
CMD+return|切换回单窗口
CMD+control+ → 或 ← |前进和后退
CMD+option+ → 或 ←|折叠代码块
option+鼠标左键单击 或 CMD+control+shift+/ 即CMD+control+?） |查看帮助
command+u|还原到保存时状态 
control-F|向右一个字符（forward）
control-B|向左一个字符（backward）
control-P|前一行（previous）
control-N|后一行（next）
control-A|去行首
control-E|到行尾（end）
control-T|调换光标两边的字符（transpose）
control-D|删除光标右侧字符（delete）
control-K|删除本行剩余的字符（kill）
CMD + F|搜索
CMD + G|搜索下一处
Shift + CMD + G|搜索上一处
CMD + [|左缩进
CMD + ]|右缩进
CTRL + .|参数提示
CMD + \|设置或取消断点
CMD + OPT + \|允许或禁用当前断点
CMD + OPT + B|查看全部断点
CMD + RETURN|编译并运行（根据设置决定是否启用断点）
CMD + R|编译并运行（不触发断点）
CMD + Y|编译并调试（触发断点）
CMD + SHIFT + RETURN|终止运行或调试
CMD + B|编译
CMD + SHIFT + K|清理
CMD + SHIFT + B|编译窗口
CMD + SHIFT + Y|调试代码窗口
CMD + SHIFT + R|调试控制台
CMD + SHIFT + E|主编辑窗口调整
CMD + OPT + ?|开发手册
CMD + CTRL + ?|快速帮助