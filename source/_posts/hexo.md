title: Hexo-创建自己的博客
date: 2015-12-03 14:13:03
tags: [Hexo,NexT]
category: 博客
---

## [Hexo](https://github.com/hexojs/hexo)

hexo是一款基于Node.js的**静态**博客框架。

### 特性
*   风一般的速度
Hexo基于Node.js，支持多进程，几百篇文章也可以秒生成。

*   流畅的撰写
支持GitHub Flavored Markdown和所有Octopress的插件。

*   扩展性
Hexo支持EJS、Swig和Stylus。通过插件支持Haml、Jade和Less.

<!-- more -->

### 安装

前提是必须先安装 Node.js。
    
    npm install hexo -g

### 升级

更新hexo到最新版

    npm update hexo -g

### 初始化

    hexo init <folder>

如果指定 `<folder>`，便会在目前的资料夹建立一个名为 `<folder>` 的新资料夹；否则会在目前资料夹初始化。

### 创建新文章

    hexo new [layout] <name>
    hexo new "Hello World"
    hexo new page "about"
    hexo n page "about"

`[layout]` 为 `scaffolds` 目录下的模版文件名。
如果不指定 `[layout]`，默认生成的md文件放在 `source/_post/` 目录下。

### 生成网站

    hexo generate
    hexo g

### 服务器

    hexo server
    hexo s

伺服器会跑在 http://localhost:port （port 预设为 **4000**，可在 **_config.yml** 设定。

### 部署

这里介绍下git部署

安装**hexo-deployer-git**

    npm install hexo-deployer-git --save

修改`_config.yml`:

    deploy:
      type: git
      repo: <repository url>
      branch: [branch]
      message: [message]

部署
  
    hexo d --generate


### 默认目录结构

*   .deploy：执行hexo deploy命令部署到GitHub上的内容目录
*   public：执行hexo generate命令，输出的静态网页内容目录
*   scaffolds：layout模板文件目录，其中的md文件可以添加编辑
*   scripts：扩展脚本目录，这里可以自定义一些javascript脚本
*   source：文章源码目录，该目录下的markdown和html文件均会被hexo处理。该页面对应repo的根目录，404文件、favicon.ico文件，CNAME文件等都应该放这里，该目录下可新建页面目录。
    *   _drafts：草稿文章
    *   _posts：发布文章
*   themes：主题文件目录
*   _config.yml：全局配置文件，大多数的设置都在这里
*   package.json：应用程序数据，指明hexo的版本等信息，类似于一般软件中的关于按钮

### 站点配置文件 *_config.yml* 简单说明

    # Hexo Configuration
    ## Docs: http://hexo.io/docs/configuration.html
    ## Source: https://github.com/hexojs/hexo/

    # Site #整站的基本信息
    title: SenPng的技术博客 #网站标题
    subtitle: #网站副标题
    description: 技术博客 #网站描述，给搜索引擎用的，在生成html中的head->meta中可看到
    author: SenPng #网站作者，在下方显示
    email: senpng@qq.com #联系邮箱
    language: zh-Hans #语言
    timezone:

    # URL #域名
    ## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
    url: http://senpng.me #你的域名
    root: /
    permalink: :year/:month/:day/:title/
    permalink_defaults:

    # Directory #文件结构
    source_dir: source
    public_dir: public
    tag_dir: tags
    archive_dir: archives
    category_dir: categories
    code_dir: downloads/code
    i18n_dir: :lang
    skip_render:

    # Writing #写文章选项
    new_post_name: :title.md # File name of new posts
    default_layout: post #默认layout方式
    titlecase: false # Transform title into titlecase
    external_link: true # Open external links in new tab
    filename_case: 0
    render_drafts: false
    post_asset_folder: false
    relative_link: false
    future: true
    highlight: #代码高亮
      enable: true #是否启用
      line_number: true #是否显示行号
      auto_detect: true
      tab_replace:

    # Category & Tag #分类与标签
    default_category: uncategorized
    category_map:
    tag_map:

    # Server #本地服务参数
    ## Hexo uses Connect as a server
    ## You can customize the logger format as defined in
    ## http://www.senchalabs.org/connect/logger.html
    port: 4000
    logger: true
    logger_format:

    # Date / Time format #日期显示格式
    ## Hexo uses Moment.js to parse and display date
    ## You can customize the date format as defined in
    ## http://momentjs.com/docs/#/displaying/format/
    date_format: YYYY-MM-DD
    time_format: HH:mm:ss

    # Pagination #分页设置
    ## Set per_page to 0 to disable pagination
    per_page: 10 #每页10篇文章
    pagination_dir: page

    # Extensions
    ## Plugins: http://hexo.io/plugins/
    ## Themes: http://hexo.io/themes/
    theme: next # 主题

    # Deployment #部署
    ## Docs: http://hexo.io/docs/deployment.html
    deploy:
      type: git
      repo: git@github.com:senpng/senpng.github.io.git
      branch: master
      message: Site updated:{.{ now("YYYY-MM-DD HH:mm:ss") }} #这里的 . 需要去掉

### 常见错误
```bash
{ [Error: Cannot find module './build/Release/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
{ [Error: Cannot find module './build/default/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
{ [Error: Cannot find module './build/Debug/DTraceProviderBindings'] code: 'MODULE_NOT_FOUND' }
```

解决方法:
```bash
npm install hexo --no-optional
```

## 主题 [NexT](https://github.com/iissnan/hexo-theme-next)

### 下载 NexT 主题

在终端窗口下，定位到 Hexo 站点目录下

    cd your-hexo-site
    git clone https://github.com/iissnan/hexo-theme-next themes/next

### 启用 NexT 主题

克隆/下载 完成后，打开 站点配置文件，找到 `theme` 字段，并将其值更改为 `next`。

### 验证主题是否启用

运行 `hexo s --debug`，并访问 <http://localhost:4000>，确保站点正确运行。

### 主题设定

#### 选择 Scheme

NexT 通过 Scheme 提供主题中的主题。 Mist 是 NexT 的第一款 Scheme。
启用 Mist 仅需在 主题配置文件 中将 `#scheme: Mist` 前面的 # 注释去掉即可。

#### 语言设置

编辑 站点配置文件，将 `language` 设置成你所需要的语言。
例如选用正体中文，则配置为：

    language: zh-hk

可用的语言如以下表格所示：

语言|代码|设定值
---|---|---
English|en|language: en
简体中文|zh-Hans|language: zh-Hans
French|fr-FR|language: fr-FR
正体中文|zh-hk/zh-tw|language: zh-hk
Russian|ru|language: ru
German|de|language: de

#### 菜单设置

菜单配置在 **主题配置文件** 的 `menu`。 若你的站点运行在子目录中，请将链接前缀的 `/` 去掉。默认支持的菜单项有：

键值|设定值|comment
---|---|---
home|home: /|主页
archives|archives: /archives|归档页
categories|categories: /categories|分类页（需手动创建）
tags|tags: /tags|标签页（需手动创建）
about|about: /about|关于页面 （需手动创建）
commonweal|commonweal: /404.html|公益 404 （需手动创建）

##### 菜单示例配置：

    menu:
      home: /
      archives: /archives
      #about: /about
      #categories: /categories
      tags: /tags
      #commonweal: /404.html

#### 侧栏设置

默认情况下，侧栏仅在文章页面（拥有目录列表）时才显示。可以通过修改 **主题配置文件** 中的  `sidebar` 字段来控制侧栏的行为。


支持的选项有：

*   `post` - 默认行为，在文章页面（拥有目录列表）时显示
*   `always` - 在所有页面中都显示
*   `hide` - 在所有页面中都隐藏（可以手动展开）


#### 侧栏示例配置：

    sidebar: post

#### 头像设置

编辑 站点配置文件，新增字段 `avatar`， 值设置成头像的链接地址。

其中，头像的链接地址可以是：

地址  值
完整的互联网 URL  `https://avatars1.githubusercontent.com/u/32269?v=3&s=460`
站点内的地址  `/uploads/avatar.jpg` - 需要将你的头像图片放置在 站点的 `source/uploads/`（可能需要新建 `uploads` 目录）
`/images/avatar.jpg` - 需要将你的头像图片放置在 主题的 `source/images/` 目录下
头像设置示例：

    avatar: https://avatars1.githubusercontent.com/u/32269?v=3&s=460

#### 作者名称

编辑 站点配置文件，设置 `author` 为你的昵称。

#### 站点描述设置

编辑 站点配置文件，设置 `description`字段为你的站点描述。站点描述可以是你喜欢的一句签名:)

#### 主题配置文件 *_config.yml* 简单说明


    # 菜单配置
    menu:
      home: /
      archives: /archives
      # categories: /categories
      tags: /tags
      about: /about
      #commonweal: /404.html


    # Enable/Disable menu icons.
    # Icon Mapping:
    #   Map a menu item to a specific FontAwesome icon name.
    #   Key is the name of menu item and value is the name of FontAwsome icon.
    #   When an question mask icon presenting up means that the item has no mapping icon.
    menu_icons:
      enable: true
      # Icon Mapping.
      home: home
      about: user
      categories: th
      tags: tags
      archives: archive
      commonweal: heartbeat


    # Mist
    # 使用 Mist 主题
    #scheme: Mist

    # Sidebar 侧栏行为，可选值有
    #  - post   默认值，在文章页面自动展开侧栏
    #  - always 在所有页面自动展开侧栏
    #  - hide   在手动点击侧栏的开关按钮时展开
    #  - remove 
    #sidebar: post
    #sidebar: always
    sidebar: hide
    #sidebar: remove

    # Social Icons
    social_icons:
      enable: true
      # Icon Mappings
      GitHub: github
      Twitter: twitter
      Weibo: weibo


    # Sidebar Avatar ＃头像
    avatar: /images/avatar.png


    # Social links ＃友情链接
    social:
      GitHub: https://github.com/senpng


    # TOC in the Sidebar
    toc:
      enable: true

      # Automatically add list number to toc.
      number: true


    # 代码高亮主题
    # available: normal | night | night eighties | night blue | night bright
    highlight_theme: normal

    # `阅读全文` 按钮跳转之后是否自动滚动页面到设置 `<!-- more -->` 的地方。
    scroll_to_more: true

    # 自动形成摘要
    auto_excerpt:
      enable: true
      length: 150

      默认截取的长度为 150 字符，可以根据需要自行设定

    # Use Lato font
    use_font_lato: true

    # MathJax support
    # 开启数学公式渲染支持，默认关闭。设置为 `true` 开启。
    mathjax: true


    #! ---------------------------------------------------------------
    #! DO NOT EDIT THE FOLLOWING SETTINGS
    #! UNLESS YOU KNOW WHAT YOU ARE DOING
    #! ---------------------------------------------------------------

    # Motion
    use_motion: true

    # Fancybox
    fancybox: true

    # Static files
    vendors: vendors
    css: css
    js: js
    images: images

    # Theme version
    version: 0.4.5.2