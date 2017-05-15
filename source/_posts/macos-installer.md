title: Mac OS Installer
date: 2016-05-09 11:12:45
tags: [Mac OS]
---

## 制作安装启动U盘
```shell
sudo /Applications/Install\ OS\ X\ Yosemite.app/Contents/Resources/createinstallmedia --volume /Volumes/OSX --applicationpath /Applications/Install\ OS\ X\ Yosemite.app --nointeraction
```

## Fusion Drive
对于一些使用混合硬盘的Mac OS电脑，为了更好的*体验*，我们需要使用`Fusion Drive`

1. 首先我们需要格式化两块硬盘
2. 合并两块硬盘

  ```bash
  diskutil list   //查看当前磁盘的详细信息
  diskutil cs create GroupName disk0 disk1  //合并磁盘
  //合并成功之后最后一行显示
  Core Storage LVG UUID: 【BBC6AD77-ADF2-40EE-A1D0-68BFE4913966】
  Finished CoreStorage operation
  ```
  需要复制这段`UUID`，之后会用到

3. 建立`Fusion Drive`
  ```bash
  diskutil cs list    //查看逻辑磁盘状态 查看实际可用容量
  diskutil coreStorage createVolume BBC6AD77-ADF2-40EE-A1D0-68BFE4913966 jhfs+ Name 375g //建立Fusion Drive 后面的容量大小不可大于实际可用容量
  ```    
  
## Fix Open Directory Will Not Turn On
```
sudo launchctl unload /System/LIbrary/LaunchDaemons/org.openldap.slapd.plist

RepairPermissions /

sudo db_recover -cv -h /var/db/openldap/openldap-data/

sudo /usr/libexec/slapd -Tt

sudo db_recover -cv -h /var/db/openldap/authdata/

sudo /usr/libexec/slapd -Tt

sudo launchctl load /System/LIbrary/LaunchDaemons/org.openldap.slapd.plist
```


