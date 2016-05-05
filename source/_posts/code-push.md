title: CodePush
date: 2016-03-23 11:24:22
tags: [iOS,ReactNative]
---

## 简介
[CodePush](http://codepush.tools)是提供给 React Native 和 Cordova 开发者直接部署移动应用更新给用户设备的云服务。CodePush 作为一个中央仓库，开发者可以推送更新到 (JS, HTML, CSS and images)，应用可以从客户端 SDKs 里面查询更新。CodePush 可以让应用有更多的可确定性，也可以让你直接接触用户群。在修复一些小问题和添加新特性的时候，不需要经过二进制打包，可以直接推送代码进行实时更新。
[Git](https://github.com/Microsoft/code-push)

## 博主肺腑
比如一个ReactNative项目，发布后，根据产品需求我们需要修改一些东西，这时候需要更新App中的jsbundle文件（也有可能是一些图片资源）。起初我是自己处理的，程序启动取得jsbundle的md5，然后发给服务器，服务器再取出服务器中的jsbundle文件的md5进行对比，如不同则App后台下载jsbundle文件，下载完成后将路径写入UserDefault中以备下次载入的时候使用该路径的新文件。这里我们先不考虑加密压缩的问题，虽然满足正常的需求，不过当我遇见了CodePush，我就把我之前写的这些处理都注释掉了。
这里我说几点CodePush给我带来的惊喜：
1.CodePush集成到项目非常简单，推荐用RNPM，或者CocoaPods。
2.有History，就比如Git的log。
3.可以创建多个部署，可以按照需求针对不同的部署发布版本更新。
4.有回滚功能，并且有记录哪些用户更新了，哪些用户回滚了。
5.合作者功能。

## [CodePush CLI(终端工具)](https://github.com/Microsoft/code-push/blob/master/cli/README.md)
### 安装
```bash
npm install -g code-push-cli
```
### 注册账号/登录（Register/Login）
```bash
code-push register
```
终端输入以上命令会自动打开浏览器，进入注册界面，我使用的GitHub账号注册的。
注册成功后。终端`Ctrl`+`C`
```bash
Usage: code-push login [--accessKey <accessKey>]

示例:
    code-push login
    code-push login --acccessKey 7YHPoQSNBGkaEYTh3ln6HUPJz8EBVyB4C3Fal
```
终端输入`code-push login`命令还是会自动打开浏览器，但是会显示Access Key。这时候我们需要复制这个Key。
回到终端，可以看到提示你输入这个Access Key。复制粘贴进去，回车即可。
这里我解释下Access Key：可以简单的理解为Token（如果你们不知道Token，可以百度搜下）。CodePush CLI是允许你在多个设备登录你的账号，每个设备登录都会有一个唯一的Access Key，并且你可以管理这些Access Key，控制其它设备是否可以登录。
管理Access Key的命令：
```bash
Usage: code-push access-key <command>

命令：
  add     添加一个新的Access Key
  remove  删除一个已经存在的Access key
  rm      删除一个已经存在的Access key
  list    列出与你账户关联的所有Access key
  ls      列出与你账户关联的所有Access key
```
#### 合作者（Collaborator）
这个功能还是很不错的。一个项目的开发肯定有很多人，也许有的人会负责一些部署的发布更新工作。你的账号又不可能都给他们吧，你可以通过这个功能让其他人也能参与到这个项目中。
命令:
```bash
Usage: code-push collaborator <command>

命令：
  add     对指定的项目添加一个新的合作者
  remove  删除指定的项目中的合作者
  rm      删除指定的项目中的合作者
  list    列出指定项目中的所有合作者
  ls      列出指定项目中的所有合作者
示例:
    code-push collaborator add AppDemo foo@bar.com 添加一个合作者foo@bar.com到AppDemo这个App中
```

### 在CodePush里创建App
```bash
Usage: code-push app add <AppName>

示例：
    code-push app add AppDemo
```
其它命令：
```bash
Usage: code-push app <command>

命令：
  add       创建一个新的App
  remove    删除App
  rm        删除App
  rename    重命名已经存在App
  list      列出与你账户关联的所有App
  ls        列出与你账户关联的所有App
  transfer  将一个App的所有权转让给另一个帐户
```

### 部署（Deployment）
App创建成功后会默认显示两个部署:`Production`和`Staging`。
这里解释下什么是部署：简单的说就是环境（如果你懂Git，你可以理解成Branch），比如ReactNative的jsbundle，iOS和Android是不可以共用一个的，所以我们需要生成两个jsbundle，而我们可以通过部署这个功能，创建两个部署：`AppDemo-iOS`和`AppDemo-Android`，并且App中分别使用这两个部署的Key，之后我们上传jsbundle只要分别上传到这两个部署中就可以了。
命令：
```bash
Usage: code-push deployment <command>

命令：
  add      在已存在的App中创建一个部署
  clear    清除与部署相关的发布历史记录
  remove   在App中删除一个部署
  rm       在App中删除一个部署
  rename   重命名一个已存在的部署
  list     列出App中的所有部署
  ls       列出App中的所有部署
  history  列出一个部署的发布历史记录
  h        列出一个部署的发布历史记录
```
每个部署都有一个对应的`Deployment Key`，需要在项目中使用对应的Key。

#### 创建部署
```bash
Usage: code-push deployment add <appName> <deploymentName>

示例：
  deployment add MyApp MyDeployment  添加一个部署"MyDeployment"到App"MyApp"中

```
#### 列出App中所有部署
```bash
Usage: code-push deployment ls <appName> [--format <format>] [--displayKeys]

选项：
  --format           终端打印格式 ("json" or "table")  [string] [默认值: "table"]
  --displayKeys, -k  是否显示Deployment Key  [boolean] [默认值: false]

示例：
  code-push deployment ls MyApp 
  code-push deployment ls MyApp --format json
```

#### 查看部署的发布历史
```bash
Usage: code-push deployment h <appName> <deploymentName> [--format <format>] [--displayAuthor]

选项：
  --format             终端打印格式 ("json" or "table")  [string] [默认值: "table"]
  --displayAuthor, -a  是否显示谁发布的  [boolean] [默认值: false]

示例：
  code-push deployment h MyApp MyDeployment                
  code-push deployment h MyApp MyDeployment --format json
```

### 发布（Release）
官方的文档里写了三种：General，ReactNative，Cordova。
我这里就先将一个通用的，如果有时间再补上其它的。
#### 通用发布
```bash
Usage: code-push release <appName> <updateContentsPath> <targetBinaryVersion> [--deploymentName <deploymentName>] [--description <description>] [--mandatory] [--rollout <rolloutPercentage>]

选项：
  --deploymentName, -d  部署名  [string] [默认值: "Staging"]
  --description, --des  描述  [string] [默认值: null]
  --disabled, -x        是否禁用该更新  [boolean] [默认值: false]
  --mandatory, -m       是否强制更新  [boolean] [默认值: false]
  --rollout, -r         此更新推送的的用户百分比  [string] [默认值: null]

示例：
  code-push release MyApp app.js "*"                                     发布 "app.js"文件到"MyApp"的"Staging"部署中,针对任何二进制版本，它使用"*"通配符范围语法。
  code-push release MyApp ./platforms/ios/www 1.0.3 -d Production        发布 "./platforms/ios/www"文件夹里的所有内容到"MyApp"的"Production"部署中, 只针对1.0.3的二进制版本
  code-push release MyApp ./platforms/ios/www 1.0.3 -d Production -r 20  发布 "./platforms/ios/www"文件夹里的所有内容到"MyApp"的"Production"部署中, 只针对1.0.3的二进制版本，并且只推给20%的用户
```
说明下关于二进制版本的问题。

范围表达式|哪些会被更新
---|---
`1.2.3`|仅仅只有`1.2.3`的版本
`*`|所有版本
`1.2.x`|主要版本`1`，次要版本`2`的任何修补程序版本
`1.2.3 - 1.2.7`|`1.2.3`版本到`1.2.7`版本
`>=1.2.3 <1.2.7`|大于等于`1.2.3`版本小于`1.2.7`的版本
`~1.2.3`|大于等于`1.2.3`版本小于`1.3.0`的版本
`^1.2.3`|大于等于`1.2.3`版本小于`2.0.0`的版本

#### 修改更新（Patching Updates）
```bash
Usage: code-push patch <appName> <deploymentName> [--label <label>] [--description <description>] [--disabled] [--mandatory] [--rollout <rolloutPercentage>]

选项：
  --label, -l           指定标签版本更新，默认最新版本 [string] [默认值: null]
  --description, --des  描述  [string] [默认值: null]
  --disabled, -x        是否禁用该更新  [boolean] [默认值: null]
  --mandatory, -m       是否强制更新  [boolean] [默认值: null]
  --rollout, -r         此更新推送用户的百分比，此值仅可以从先前的值增加。  [string] [默认值: null]

示例：
  code-push patch MyApp Production --des "Updated description" -r 50         修改"MyApp"的"Production"部署中最新更新的描述 ，并且更新推送范围为50％
  code-push patch MyApp Production -l v3 --des "Updated description for v3"  修改"MyApp"的"Production"部署中标签为v3的更新的描述
```

#### 促进更新（Promoting Updates）
也就是把其它部署中的最新更新发布到其它的部署中
```bash
Usage: code-push promote <appName> <sourceDeploymentName> <destDeploymentName> [--description <description>] [--mandatory] [--rollout <rolloutPercentage>]

选项：
  --description, --des  描述  [string] [默认值: null]
  --disabled, -x        是否禁用该更新  [boolean] [默认值: null]
  --mandatory, -m       是否强制更新  [boolean] [默认值: null]
  --rollout, -r         此促进更新推送用户的百分比  [string] [默认值: null]

示例：
  code-push promote MyApp Staging Production                                   "MyApp"中"Staging"部署的最新更新发布到"Production"部署中
  code-push promote MyApp Staging Production --des "Production rollout" -r 25  "MyApp"中"Staging"部署的最新更新发布到"Production"部署中, 并且只推送25%的用户
```
#### 回滚更新（Rolling Back Updates）
```bash
Usage: code-push rollback <appName> <deploymentName> [--targetRelease <releaseLabel>]

选项：
  --targetRelease, -r  指定回归到哪个标签，默认是回滚到上一个更新  [string] [默认值: null]

示例：
  code-push rollback MyApp Production                     "MyApp"中"Production"部署执行回滚
  code-push rollback MyApp Production --targetRelease v4  "MyApp"中"Production"部署执行回滚，回滚到v4这个标签版本
```
官方的列子：

Release|Description|Mandatory
---|---|---
v1|Initial release!|Yes
v2|Added new feature|No
v3|Bug fixes|Yes

v3这个更新出了点问题，需要回滚操作，回滚到v2.
执行回滚操作后History就变成这个样子了

Release|Description|Mandatory
---|---|---
v1|Initial release!|Yes
v2|Added new feature|No
v3|Bug fixes|Yes
v4 (Rollback from v3 to v2)|Added new feature|No

## 未完待续