title: iOS 自动打包发布持续集成系统
date: 2016-9-13
---

## iOS 自动打包发布持续集成系统

### 使用 xcodebuild 和 xctool

安装`xctool`:
```bash
brew install xctool
xctool -workspace "./ios/AppDemo.xcworkspace" -scheme "AppDemo" -sdk iphoneos archive -archivePath "./build/AppDemo.xcarchive" -configuration Release
```
导出IPA:
```bash
xcodebuild -exportArchive -archivePath "./build/AppDemo.xcarchive" -exportPath "./build/AppDemo.ipa" -exportOptionsPlist "./scripts/ExportOptions.plist" CODE_SIGN_IDENTITY="iPhone Distribution: xxxxxxxx" PROVISIONING_PROFILE="xxxx-xxxx-xxxx-xxxx-xxxx"
```





