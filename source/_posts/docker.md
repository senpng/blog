title: Docker
date: 2016-6-28
---

## 什么是`Docker`?

简单的说Docker是一个构建在LXC(Linux Container, 内核虚拟技术)之上的,基于进程容器(Processcontainer)的轻量级VM解决方案

![](/images/1.jpg)
拿现实世界中货物的运输作类比, 为了解决各种型号规格尺寸的货物在各种运输工具上进行运输的问题,我们发明了集装箱。
Docker的初衷也就是将各种应用程序和他们所依赖的运行环境打包成标准的container/image,进而发布到不同的平台上运行

## 常用命令
```bash
docker search {name} //搜索docker镜像
docker pull {name} //下载指定镜像
docker run {name} -d -t -i -rm //运行一个容器
```
 
## Dockerfile
编写完成Dockerfile之后，可以通过`docker build`命令来创建镜像。
基本的格式为 `docker build [选项] 路径`，该命令将读取指定路径下（包括子目录）的Dockerfile，并将该路径下所有内容发送给Docker服务端，由服务端来创建镜像。因此一般建议放置Dockerfile的目录为空目录。也可以通过`.dockerignore`文件（每一行添加一条匹配模式）来让Docker忽略路径下的目录和文件。
要指定镜像的标签信息，可以通过`-t`选项，例如
```bash
$ sudo docker build -t myrepo/myapp /tmp/test1/
```

###FROM
格式为`FROM <image>`或`FROM <image>:<tag>`。

第一条指令必须为`FROM`指令。并且，如果在同一个Dockerfile中创建多个镜像时，可以使用多个 `FROM` 指令（每个镜像一次）。

### MAINTAINER
格式为`MAINTAINER <name>`, 指定维护者信息

### RUN
格式为`RUN <command>`或`RUN ["executable", "param1", "param2"]`

### CMD
支持三种格式
* `CMD ["executable", "param1", "param2"]`
* `CMD cmmand param1 param2`
* `CMD ["param1", "param2"]`
每个Dockerfile只能有一条`CMD`命令

### EXPOSR
格式为 `EXPOSE <port> [<port>...]`
告诉 `Docker` 服务端容器暴露的端口号，供互联系统使用。在启动容器时需要通过 `-P`, `Docker` 主机会自动分配一个端口转发到指定的端口。

### ENV
格式为 `ENV <key> <value>`
指定一个环境变量，可被后续 `RUN` 指令使用，并在容器运行保持

### ADD
格式为 `ADD <src> <dest>`
该命令复制指定的 `<src>` 到容器中的 `<dest>`。其中 `<src>` 可以是Dockerfile所在目录的一个相对路径，也可以是一个URL，还可以是一个tar文件（自动解压为目录）

### COPY
格式为 `COPY <src> <dest>`
复制本地主机的 `<src>` （为Dockerfile所在目录的相对路径）到容器中的 `dest`。
当使用本地目录为源目录时，推荐使用 `COPY`。

### ENTRYPOINT
两种格式：
* `ENTRYPOINT ["executable", "param1", "param2"]`
* `ENTRYPOINT command param1 param2`
配置容器启动后执行的命令，并且不可被 `docker run` 提供参数覆盖。
每个Dockerfile中只能有一个`ENTRYPOINT`，当指定多个时，只有最后一个起效。

### VOLUME
格式为 `VOLUME ["/data"]`
创建一个可以从本地主机或者其它容器挂载的挂载点，一般用来存放数据库和需要保持的数据等

### USER
格式为 `USER daemon`
指定运行容器时的用户名或UID，后续的 `RUN` 也会使用指定用户
当服务不需要管理员权限时，可以通过该命令指定运行用户。并且可以在之前创建所需要的用户，例如：
`RUN groupadd -r postgres && useradd -r -g postgres postgres`。要临时获取管理员权限可以使用 `gosu`，而不推荐 `sudo`。

### WORKDIR
格式为 `WORKDIR /path/to/workdir`
为后续的 `RUN`、`CMD`、`ENTRYPOINT`指令配置工作目录。
可以使用多个 `WORKDIR` 指令，后续命令如果参数是相对路径，则会基于之前命令指定的路径，例如：
```bash
WORKDIR /a
WORKDIR b
WORKDIR c
RUN pwd
```
则最终路径为 `/a/b/c`

### ONBUILD
格式为 `ONBUILD [INSTRUCTION]`
配置当前所创建的镜像作为其它新创镜像的基础镜像时，所执行的操作指令。
例如，Dockerfile使用如下的内容创建了镜像`image-A`。
```bash
[...]
ONBUILD ADD . /app/src
ONBUILD RUN /usr/local/bin/python-build --dir /app/src
[...]
```
如果基于`image-A`创建新的镜像时，新的Dockerfile中使用 `FROM image-A` 指定基础镜像时，会自动执行 `ONBUILD` 指定内容，等价于在后面添加了两条指令。
```bash
FROM image-A

#Automatically run the following
ADD . /app/src
RUN /usr/local/bin/python-build --dir /app/src
```
使用 `ONBUILD` 指令的镜像，推荐在标签中注明，例如 `ruby:1.9-onbuild`。


## 导出镜像
```bash
#save 用于持久化镜像,保存的是所有这个镜像的版本记录，包括一些历史数据.
docker save -o <save_name>.tar.gz <image_name>

#export 用于持久化容器,导出的是容器当中所用的镜像内容.
docker export <container_id> > ./<save_name>.tar.gz
```