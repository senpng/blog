title: 其它
date: 2016-03-30 14:15:53
tags:
---

## iphone 抓包方式（个人觉得最好之一*2）

### rvictl
```bash
rvictl [-h][-l][-s <udid1> ... <udidN>][-x <udid1> ... <udidN>]

Remote Virtual Interface Tool starts and stops a remote packet capture instance 
for any set of attached mobile devices. It can also provide feedback on any attached 
devices that are currently relaying packets back to this host. 

Options:
    -l, -L      List currently active devices
    -s, -S      Start a device or set of devices
    -x, -X      Stop a device or set of devices
```
通过该命令在pc上添加一个虚拟网卡。这个网卡的数据就是iphone的流量数据，包括2、3、4G以及wifi。
可以使用wireshark来抓取该虚拟网卡上的数据;

### Charles
著名的‘花瓶’抓包工具。这里个人也破解了。。如果有需要可以网上下载，也可以发邮件给我。我会将最新版本破解发给你。
主要步骤：
1.pc共享一个wifi，供手机端连接。
2.打开`Charles`。
3.回到手机端，设置代理，代理IP为共享wifi的这台pc的IP，代理端口默认是`8888`。
4.这个时候回到`Charles`就可以看到提示有设备访问了，需要点击同意。就能看到手机上的数据了。
