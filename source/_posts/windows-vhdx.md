title: Windows VHD/VHDX
date: 2016-05-09 11:10:45
tags:
---

## 什么是VHD/VHDX
微软Microsoft Virtual Hard Disk format（虚拟磁盘）格式。
随着虚拟环境的企业工作负荷的增加以及性能要求的提高，虚拟硬盘 (VHD) 格式需要适应这些变化。Windows Server 2012 中的 Hyper-V 引入了一个新版本的 VHD 格式，称为 VHDX，它在设计上可以处理当前以及将来的工作负荷。
与旧的 VHD 格式相比，VHDX 具有更大的存储容量。它还在电源故障期间提供数据损坏保护并且优化动态磁盘和差异磁盘的结构对齐方式，以防止在新的大型扇区物理磁盘上性能降级。

## 创建VHD/VHDX
启动命令行（管理员模式）
```shell
diskpart    //进入磁盘工具
```
```shell
创建虚拟磁盘文件。当前仅支持 VHD 和 VHDX 格式的文件，
    它们是通过虚拟磁盘文件的
    文件扩展名(.vhd 和 .vhdx)指定的。

语法:  CREATE VDISK FILE=<"filename"> MAXIMUM=<N> [TYPE={FIXED|EXPANDABLE}]
             [SD=<SDDL string>] [PARENT=<"filename">] [SOURCE=<"filename">]
             [NOERR]
             FILE=<"filename">

                指定虚拟磁盘文件的完整路径和文件名。
                该文件可以位于网络共享上。

    MAXIMUM=<N> 虚拟磁盘公开的最大空间量，
                以兆字节(MB)为单位。

    TYPE={FIXED|EXPANDABLE}

                FIXED 指定固定大小的虚拟磁盘文件。EXPANDABLE
                指定重新调整大小以适应所分配数据的
                虚拟磁盘文件。默认值为 FIXED。

    [SD=<SDDL string>]

                指定安全描述符定义语言(SDDL)格式
                的安全描述符。默认情况下，
                安全描述符是从父目录中获取的。

                SDDL 字符串可能比较复杂，但很灵活。
                其最简单的格式是保护访问的安全描述符，称为自定义访问控制列表(DACL)。
                其格式为:

                D:<DACL_FLAGS>(<STRING_ACE>)(<STRING_ACE>)...(<STRING_ACE>)

                常用的 DACL_FLAGS 为:

                   "P" - DACL 不应被来自父容器的
                         任何 ACL 所覆盖(保护)。
                         VHD 或 VHDX 文件的容器就是
                         其目录。
                   "AI"- DACL 应该从父容器自动
                         继承。

                STRING_ACE 的格式为

                   <ACE_TYPE>;;<RIGHTS>;;;<ACCOUNT_ID>

                常用的 ACE_TYPE 为:

                   "A" - 允许访问。
                   "D" - 拒绝访问。

                常用的 RIGHTS 为:

                   "GA" - 所有访问权限。
                   "GR" - 读取访问权限。
                   "GW' - 写入访问权限。

                常用的 ACCOUNT_ID 为:

                   "BA" - 内置管理员
                   "AU" - 通过身份验证的用户。
                   "CO" - 创建者所有者。
                   "WD" - 任何人。

                将所有这些放在一起，例如，

                  D:P:(A;;GR;;;AU)

                为所有通过身份验证的用户授予读取访问权限。

                同样，

                  D:P:(A;;GA;;;WD)

                为任何人授予完全访问权限。

                可以在 Microsoft 的 MSDN 网站上
                找到有关 SDDL 的详细信息。

    [PARENT=<"filename">]

                用于创建差异磁盘的现有父虚拟磁盘文件的路径。
                对于 PARENT 参数，不应指定 MAXIMUM，
                因为差异磁盘从其父目录获取大小。
                而且，也不应指定 TYPE，
                因为只能创建 EXPANDABLE 差异磁盘。


    [SOURCE=<"filename">]

                用于预填充新虚拟磁盘文件的现有虚拟磁盘文件的路径。
                当指定 SOURCE 时，
                输入虚拟磁盘文件中的数据
                将从输入虚拟磁盘文件逐块复制到创建的
                虚拟磁盘文件。
                但不建立父子关系。


    NOERR       仅用于脚本。遇到错误时，DiskPart
                会继续处理命令，如同没有出现错误一样。
                如果不使用 NOERR 参数，错误会导致 DiskPart 退出
                并返回错误代码。
示例:

    CREATE VDISK FILE="c:\test\test.vhd" MAXIMUM=1000
    CREATE VDISK FILE="c:\test\child.vhdx" PARENT="c:\test\test.vhdx"
    CREATE VDISK FILE="c:\test\test.vhd" MAXIMUM=1000 SD="D:P(A;;GA;;;WD)"
    CREATE VDISK FILE="c:\test\new.vhdx" SOURCE="c:\test\test.vhd"
```

## 扩展VHDX大小
启动命令行（管理员模式）
```shell
diskpart    //进入磁盘工具
```
```shell
select vdisk file=<path>    //选中指定的vdhx文件
expand vdisk maximum=<N>    //扩展选中的vhdx文件
```
> 扩展后需要进入`磁盘管理`将扩展后的部分`扩展卷`
![磁盘管理](/images/windows-vhdx/disk_manager.png)

## 关于基于母盘下的子盘大小扩展
按照*规矩*只要基于母盘创建子盘后，母盘是不可以有任何修改的，不然会导致子盘无法正常使用。
自己尝试过，将母盘与子盘同时扩展相同大小，是可以扩展大小并正常使用的。我的猜想：可能是因为扩展大小并不是修改母盘内的数据，而是修改的`vhdx`的文件信息