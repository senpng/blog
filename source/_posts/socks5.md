title: Linux Socks5搭建
date: 2015-12-10 14:05:24
tags: [Linux,socks5]
---

## 安装socks5
### 安装依赖开发库

    yum install pam-devel openldap-devel openssl-devel

### 安装socks 5

    wget http://downloads.sourceforge.net/project/ss5/ss5/3.8.9-8/ss5-3.8.9-8.tar.gz?r=&ts=1396802581&use_mirror=cznic
    tar -xzvf ss5-3.8.9-8.tar.gz
    cd ss5-3.8.9
    ./configure
    make && make install
    


## socks 5配置

### 修改配置文件

    vim /etc/opt/ss5/ss5.conf
    
    auth 0.0.0.0/0 – -
    改为
    auth 0.0.0.0/0 – u
 
    permit – 0.0.0.0/0 – 0.0.0.0/0 – – – – -
    改成为
    permit u 0.0.0.0/0 – 0.0.0.0/0 – – – – -

### 添加socket 5用户

    cat /etc/opt/ss5/ss5.passwd
    #用户  密码
    senpng 123456

## 启动socket 5

默认情况ss5文件没有执行权限

    chmod u+x /etc/rc.d/init.d/ss5

启动

    sh /etc/rc.d/init.d/ss5 start

如果觉得使用sh来启动麻烦，那么按如下方法：

    chkconfig --add ss5 //可选
    chkconfig ss5 on //可选
    service ss5 start

查看是否启动

    netstat -lntp  | grep ss5
    tcp        0      0 0.0.0.0:1080   0.0.0.0:*      LISTEN      14262/ss5

默认端口1080