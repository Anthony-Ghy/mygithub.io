---
title: Hexo+GitHubPages搭博踩坑记录
tags:
- 博客搭建
- 前端小白
---
## 
> 本文主要介绍下Hexo和GitHub Pages之间是如何联系及运作的，同时记录我在搭建过程中踩过的坑以及解决方案。

### 前言

​       本文首先讲述个人博客各个模块。然后会解释Hexo到底做了什么，即Hexo在个人博客中起到的作用，GitHub Pages又在博客中扮演一个什么角色。在理解了以上内容后，搭建博客就变成了一件非常容易的事情，最后以操作手册式的描述引导你一步步搭建出一个博客。

<!-- more -->

​       作为一名程序员，拥有一个自己的博客是十分重要的一件事，程序员需要将自己的知识进行沉淀并进行记录是一个十分好的习惯。日常读别人的博客时，看到一些很优秀的博客经常会很羡慕。内心也有着自己想做一个自己博客的冲动。关于创建博客的好处这里就不用多说了，首先秀一下[我的博客](https://anthony-ghy.github.io/)。

![](https://img14.360buyimg.com/imagetools/jfs/t1/135155/34/16611/966821/5fb663f5Eb28a8bdd/23e36f484947905d.jpg)

接下来我们先了解一下博客是如何运作的。

### 静态网页运作流程

我们首先确认一件事就是我们的博客本质上就是一个Web网站，所以我们先介绍下整个的web运行流程：

![](https://img11.360buyimg.com/imagetools/jfs/t1/136311/35/16659/90809/5fb62584Efd8e3bbc/83f059e6329097da.png)



​       大致画了个流程图，其实里面的运行流程复杂的多，这里我们对比下web渲染流程，我们就能分析出博客是如何运作的了。用户在浏览器输入一个url并按下搜索键以后，客户端会首先处理这个请求，但是客户端需要根据ip地址查找资源，并在获取资源文件以后将内容显示到当前的浏览器上面。

**DNS查询**

我们输入域名并不是真正的IP地址，也就是资源存储在互联网的具体位置。要知道这个域名背后指向的IP地址是多少，然后按照此IP进行寻址，找到服务器并通过www服务获取所需要的资源。从图上看就是DNS查询过程，这里省略了DNS查询过程的细节，有兴趣的同学可以去计算机网络课本中了解具体的流程。简单地说这一过程就是通过输入你的URL地址去DNS服务器查询到当前域名的IP地址。

**http请求**

通过TCP协议的三次握手等一系列操作，客户端会使用HTTP/HTTPS协议构建一个HTTP请求包，并发送给相应IP地址下的服务器。

**服务器处理请求**

收到请求以后，发现是一个www服务的请求，如果发现是index.html资源文件，就会交由后台程序去配置html资源文件，最终由服务器返回给客户端。

**浏览器解析HTML**

客户端拿到html资源文件后交由浏览器去解析处理，浏览器需要解析的不仅是HTML文件，还需要处理css、js等文件，并加载图片视频等媒体资源。通过一系列的处理后浏览器将资源展现给用户。 

哎呀，说的这些放佛想起了上大学的时候学的计算机网络相关的知识哈哈哈哈！

**服务器**

GitHub Pages是GitHub公司提供的免费静态网站托管服务。GitHub Pages严格意义上来说并不是一个服务器，只是可以提供类似服务器功能的一种服务。当我们把HTML等资源文件存放到GitHub指定的位置时，也就是一个GitHub Pages仓库下，GitHub Pages服务会对这些文件进行处理并把它展示为一个网站。所以说可以将GitHub Pages提供的功能替代Web服务器的功能。我们只需要把编写好的HTML等资源文件存到GitHub指定的位置，那么就实现了类似的Web服务器的功能，可以响应请求，并把相应的资源文件发送给客户端。

**Hexo博客框架**

Hexo其实就是做这个工作的。Hexo是一款快速、简洁且高效的博客框架，可以将我们使用Markdown编辑的md格式的文件生成html资源文件。将这些文件上传到GitHub Pages上，理论上别人就能访问到我们的博客了。至于为什么强调使用markdown，作为一名程序员写文档就是需要markdown，只要加深对markdown语法的理解，很快就能写一篇精美的技术文章。

综上所述，Hexo+GithubPages完全能满足我目前写博客的需求。

### 搭建流程介绍

#### GitHubPages设置

这里我默认我们都在自己的电脑上面有git，我们需要获取SSH Key公钥

​       查看当前用户的目录下是否存在.ssh目录，如果存在进入到此目录下检查是否存在id_rsa和id_rsa.pub两个文件，这两个文件分别对应的是公钥和私钥，如果存在直接跳过此步，否则输入下面的命令：

```
$ ssh-keygen -t rsa -C “your_github_email”
```

一直点回车，最终会生成一个矩形图案，记录下生成的一段字符，这段字符就是我们需要的公钥。

登录github,配置SSH,在下方截图那里新建一个title,同时将生成的公钥复制到Key输入框里面，确认无误以后点击add

![](https://img10.360buyimg.com/imagetools/jfs/t1/144837/37/14858/219068/5fb64118E445d54d9/6cb883bb2ee101af.jpg)

如果添加成功就会生成下方的界面，大功告成。

![](https://img12.360buyimg.com/imagetools/jfs/t1/153973/16/6549/273576/5fb6415bEb4df2f72/3ec1d7752009914c.jpg)

#### 创建Github Pages仓库

<img src="https://img13.360buyimg.com/imagetools/jfs/t1/144427/21/15050/652205/5fb642f6E57c54e14/ff4c81afcd93051f.jpg" style="zoom:40%;" />

这里有个坑！！！一定要在Repository name输入框中你需要填入你的【Github用户名】.github.io，这是十分重要的,这样的目的是为了保证你建立的是GithubPages页面而不是其他的代码仓库,如果不这样设计，你以后不能在https://your_github_name.github.io看到你的页面，如果创建以后能看到一个简陋的页面说明创建成功。

#### 安装hexo并初始化博客

这里我默认我们的电脑已经安装了Node.js

#### 安装Hexo

```
$ npm install -g hexo-cli
```

#### 创建博客

创建一个博客，首先需要创建一个博客文件夹，这个文件夹我们可以称为站点根目录，进入到这个目录运行命令

```
$ hexo init  #使用Hexo初始化站点根目录
$ npm install  #安装npm所依赖的文件 
$ npm run server  #本地起服务 
```

服务起来以后，在浏览器输入http://localhost:4000，先看下此时博客是什么样子的。

![](https://img14.360buyimg.com/imagetools/jfs/t1/146657/9/14872/1459131/5fb663eaEdc76e591/e74ddfe6c4592441.jpg)

项目目录为：

<img src="https://img13.360buyimg.com/imagetools/jfs/t1/145706/5/15810/121071/5fbb67d1E57fa9477/601b6a70735011e6.jpg" style="zoom:50%;" />

### config.yml

Config.yml是整个博客的配置文件，我们需要在这里面进行一些关于博客的配置，具体我们可以参考[Hexo配置官网](https://hexo.io/zh-cn/docs/configuration)进行配置。

<img src="https://img10.360buyimg.com/imagetools/jfs/t1/122111/17/19605/500162/5fbb688dE78d572c1/86e9c8dd2b963941.jpg" style="zoom:50%;" />

目前我们的博客在本地可以看到，但是还不能在GitHubPages上看到，这时候我们需要上传到GitHubPages，方法就是通过git将本地博客与Github远程仓库进行关联,并且把本地文件提交到远程仓库中，在Hexo中，我们可以在_config.yml中做相应的配置，最终通过命令行就可以很方便的将本地博客提交到远程仓库中。打开站点配置文件 _config.yml，在deploy字段中配置如下参数：

```
deploy:
    type: git
    repo:git@github.com:yourname/yourname.github.io.git
    branch:master
```

安装插件：

```
$ npm install hexo-deployer-git —save
```

执行命令： npm run deploy

在浏览器中输入GitHubPages的地址：[your_github_name.github.io](https://your_github_name.github.io/)，会发现我们的博客已经提交到GitHubPages。

### 主题配置

​      根据官网介绍创建或更换Hexo主题十分容易，只需要在themes文件夹内新增一个以theme名称命名的文件夹，并站点根目录下的_config.yml博客配置文件中的theme字段中修改成对应的theme名称即可切换主题。

​       主题可以直接从 [Hexo主题官网](https://hexo.io/themes/)选择自己喜欢的主题样式。个人建议找一些star数比较高的主题，同时近期也在维护的主题，这样在构建博客的时候如果出现问题还能提issues,找到主题以后我们可以直接clone保存到themes文件夹下，结合我上面的博客展示,我采用的是比较多人使用的Next主题。

```
$ git clone https://github.com/iissnan/hexo-theme-next themes/next
```

在站点 _config.yml中配置：

```
# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: next
```

执行命令：

```
hexo clean
hexo server
```

打开以后网址以后，主题已经更换。

接下来介绍一些我在我博客中的一些配置，每个人的配置方案或者主题选择不同，这里只是提供下我个人的一些配置方案留作参考。

##### 1.增加一个图标

![](https://img14.360buyimg.com/imagetools/jfs/t1/136239/18/17053/23217/5fbb19f0E89b65860/bd9e07c7e39fac9b.jpg)

在主题目录下_config.yml文件查询 favicon，只需要修改前两个：small和medium，图片的像素得为16像素和32像素,图标下载我是在[iconFont](https://www.iconfont.cn/)中进行查找的，大家可以根据自己的喜好进行选择下载。

![](https://img11.360buyimg.com/imagetools/jfs/t1/154905/11/6852/245628/5fbb1aa8E1ebf3500/72cbdf839e6e9a42.jpg)

一定记住按照路径引入你的图标文件😯～

##### 2.开启菜单栏跳转

我的设置结果如下：

<img src="https://img12.360buyimg.com/imagetools/jfs/t1/120743/8/19488/87526/5fbb1b39E14f4f9dd/ef6681b920600f22.jpg" style="zoom:50%;" />

主题目录下的_config.yml中，找到menu字段,可以针对自己的喜好进行将感兴趣的前面的#去掉即可,这些页面需要我们自己创建，例如关于，

```
hexo new page categories
```

去站点目录的source/categories的index.md 中，将下面的复制进去即可。

```
---
title: 分类
type: categories 
comments: true 
---
```

 这里记住需要变得就是type,根据不用menu字段进行配置。

##### 3.头像设置

<img src="https://img14.360buyimg.com/imagetools/jfs/t1/149120/2/15376/241859/5fbb5355E17051768/82181e3c6deda438.jpg" style="zoom:50%;" />

图片相对应的路径引入自己的图片即可使用。

##### 4.社交添加

<img src="https://img11.360buyimg.com/imagetools/jfs/t1/154615/36/6887/129332/5fbb5415E020cc7ac/fe74d18ad3f847a6.jpg" style="zoom:50%;" />

配置如下，在主题目录下的_config.yml中，找到social字段，根据自己的喜好进行配置即可。

<img src="https://img10.360buyimg.com/imagetools/jfs/t1/134889/27/16969/568122/5fbb559fEa524a8ca/33e60faccae0523c.jpg" style="zoom:50%;" />

##### 5.站内搜索添加

```
npm install hexo-generator-search --save
```

在站点目录的_config.yml文件把下面代码添加进去

```
# 搜索
search:
  path: search.xml
  field: post
  content: true
```

在主题_config.yml文件中添加下方代码

```
# Local Search
# Dependencies: https://github.com/theme-next/hexo-generator-searchdb
local_search:
  enable: true # 开启站内搜索
  # 如果自动，则通过更改输入触发搜索。
  # 如果是手动，则按回车键或搜索按钮触发搜索。
  trigger: auto
  # 显示每篇文章的前n个结果，通过设置-1显示所有结果
  top_n_per_article: 1
  # 将html字符串转换为可读字符串。
  unescape: false
  # 加载页面时预加载搜索数据。
  preload: false
```

##### 6.阅读全文按钮

<img src="https://img14.360buyimg.com/imagetools/jfs/t1/139405/15/15453/256222/5fbb5742E9c0eb1b8/2e1f4addee23a349.jpg" style="zoom:40%;" />

这里有个坑，当时看到有几种解决方案，有的已经失效了，有的不太兼容所有文章，如果想在不同的文章不同的地方添加的话只需添加

`<!-- more -->`即可，它可以在你想省略的地方进行添加。

##### 7.实现文章统计功能

安装

```
npm install hexo-symbols-count-time --save
```

在站点目录的config.yml文件中添加：

```
symbols_count_time:
  symbols: true                # 文章字数统计
  time: true                   # 文章阅读时长
  total_symbols: true          # 站点总字数统计
  total_time: true             # 站点总阅读时长
  exclude_codeblock: false     # 排除代码字数统计
```

最终效果如下：

<img src="https://img10.360buyimg.com/imagetools/jfs/t1/125510/18/20044/70057/5fbb5988E35562d75/7e469713027b60f6.jpg" style="zoom:50%;" />

##### 8.头部添加进度条

效果如下：

<img src="https://img13.360buyimg.com/imagetools/jfs/t1/147178/23/15562/454545/5fbb5aceE9ae48a68/954be16cbebe607d.jpg" style="zoom:50%;" />

实现方式：

主题目录的_config.yml查询back2top字段，配置如下：

```
back2top:
  enable: true
  # Back to top in sidebar.
  sidebar: false
  # Scroll percent label in b2t button.
  scrollpercent: true # 开启
```

##### 9.添加Live 2D 模型

```
npm install --save hexo-helper-live2d
```

在站点目录_config.yml添加如下配置

```
live2d:
  enable: true
  scriptFrom: local
  pluginRootPath: live2dw/
  pluginJsPath: lib/
  pluginModelPath: assets/
  tagMode: false
  debug: false
  model:
    use: live2d-widget-model-hibiki    // 这里选择你喜欢的动漫名称嗷～
  display:
    position: left
    width: 150
    height: 350
    hOffset: 40
    vOffset: 0
  mobile:
    show: true
  react:
    opacity: 0.7
```

模型挑选可以直接从 [live2d-widget-models](https://github.com/xiazeyu/live2d-widget-models)进行挑选。

最终效果图如下：

<img src="https://img10.360buyimg.com/imagetools/jfs/t1/153300/16/6986/2594942/5fbb5e5fEa20b9b55/eec4db749d7cb426.gif" style="zoom:50%;" />

反正我是觉得挺可爱的～

### 总结

​       这片文章主要介绍了用hexo+GithubPages搭建博客的流程，结合一些web应用基本原理，hexo基本原理进行展开，后面补充了一些基本的博客配置，介绍了我在搭建博客中遇到的一些坑以及如何解决的，希望给有兴趣搭博的人提供一些参考。

### 参考文献

[Hexo框架](https://hexo.io/zh-cn/)

[Next使用文档](https://theme-next.iissnan.com/)