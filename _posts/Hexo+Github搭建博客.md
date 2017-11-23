---
title: Hexo+Github搭建博客
date: 2016-09-24 18:44:20
categories: Hexo
---


什么是Hexo？

> Hexo是一款基于Node.js的静态博客框架

# 准备工作
## 安装Git
安装git是为了使用git命令
点击进入[git官网](https://git-scm.com/)进行下载

## 注册github账号
注册一个github账号，并且创建仓库
github是个远程仓库，用git命令可以将我们的项目文件推送到这个仓库

仓库名称必须为以下固定格式：
```bash
[github账户名].github.io
```

## 安装node.js
点击进入[node.js官网](https://nodejs.org/en/)进行下载
ps:在安装node时，会将npm也集成到里面一起安装，npm是个包管理工具

## 安装hexo
```bash
npm install hexo-cli -g//在全局安装hexo
npm install hexo-deployer-git --save//hexo的发布插件
```

# 初始化博客
```bash
hexo init
```

初始化博客以后，我们可以看到博客文件夹里的文件是这样的：
![](http://ok9npmnqk.bkt.clouddn.com/1485158249622.png)

# 启动本地服务
```bash
hexo server
```

访问地址：localhost:4000
如果想要更换端口号：

```bash
hexo server -p 5000
```

在地址栏输入localhost:5000，就可以看到以下页面：
![](http://ok9npmnqk.bkt.clouddn.com/1485160791842.png)

# 打造博客专属域名
在使用HEXO+github搭建了自己的博客后，我的博客网址是：http://webrenee7.github.io，那么如何打造博客的专属域名呢？

## 购买域名
我们可以通过万网申请，查找你想申请的域名，如果可以买，购买就可以了。
比如我输入”xuya”，会列出已经注册和未注册的域名：
![](http://ok9npmnqk.bkt.clouddn.com/1485172829302.png)

购买时需要输入域名所有者信息

## 域名解析
依次点击控制台——>域名与网站（万网）——>域名，跳到如下页面：
![](http://ok9npmnqk.bkt.clouddn.com/1485173030022.png)

然后点击管理–>域名解析
接下来，需要将你购买的域名和ip地址关联起来，那么如何知道你的IP地址呢？很简单，只要在DOS中运行以下命令即可

```bash
ping www.webrenee7.github.io
```

运行结果：
![](http://ok9npmnqk.bkt.clouddn.com/1485173823727.png)

发现我的IP地址是151.101.100.133，只要输入到记录值那里，点击确定就行。
![](http://ok9npmnqk.bkt.clouddn.com/1485174203349.png)

## 绑定域名
在你的博客项目source文件夹下创建一个CNAME文件
![](http://ok9npmnqk.bkt.clouddn.com/1485173936744.png)

在CNAME中输入：www.xuya.site (你购买的域名)
![](http://ok9npmnqk.bkt.clouddn.com/1485174280940.png)

然后重新提交到github即可

OK，现在就可以在地址栏中输入你的域名进行访问了！
![](http://ok9npmnqk.bkt.clouddn.com/1485174049600.png)

以后不用再担心没钱买服务器，github就可以帮我们做到了，虽然有点慢，但还是可以将就用的~

# 配置博客
## 目录结构解读
```bash
|-- _config.yml
|-- package.json
|-- scaffolds
|-- scripts
|-- source
    |-- _drafts
    |-- _posts
|-- themes
```
**_config.yml**：全局配置文件，网站的很多信息都在这里配置，诸如网站名称，副标题，描述，作者，语言，主题，部署等等参数。这个文件下面会做较为详细的介绍。

**package.json**：hexo框架的参数，如果不小心把它删掉了，没关系，新建一个文件，讲内容写入文件，保存就OK了。里面的参数基本上是固定的，如下：
```bash

{
    "name": "hexo",
    "version": "2.4.5",
    "private": true,
    "dependen
}
```

**scaffolds**：scaffolds是“脚手架、骨架”的意思，当你新建一篇文章（hexo new ‘title’）的时候，hexo是根据这个目录下的文件进行构建的。基本不用关心。

**scripts**：脚本目录，此目录下的JavaScript文件会被自动执行。

**source**：这个目录很重要，新建的文章都是在保存在这个目录下的，有两个子目录： _drafts ， _posts 。需要新建的博文都放在 _posts 目录下。
_posts 目录下是一个个 markdown 文件。你应该可以看到一个 hello-world.md 的文件，文章就在这个文件中编辑。
_posts 目录下的md文件，会被编译成html文件，放到 public （此文件现在应该没有，因为你还没有编译过）文件夹下。

**themes**：网站主题目录，hexo有非常好的主题拓展，支持的主题也很丰富。该目录下，每一个子目录就是一个主题。你也可以自己下载主题放到该文件下。
主题目录下我们可以进行很多自定义的操作，诸如，给网站添加微博秀、添加评论组件、添加分享组件、添加统计等等，让自己的博客网站更丰富、有趣、实用。

## 站点配置文件解读
### 网站

```bash
title: 徐娅的个人博客 #网站标题
subtitle: 一枚程序媛的成长之路 #网站副标题
description: 记录成长的点滴 #网站描述
author: Xu Ya #作者
language: zh-CN #网站使用的语言
timezone: #网站时区，hexo默认使用您电脑的时区
```

### 网址

```bash
## 如果您的网站存放在子目录中，例如 http://yoursite.com/blog，则请将您的 url 设为http://yoursite.com/blog 并把 root 设为 /blog/。
root: / #网站根目录
permalink: :year/:month/:day/:title/ #文章的永久链接格式
permalink_defaults: #永久链接中各部分的值
```

### 目录

```bash
source_dir: source #资源文件夹
public_dir: public #公共文件夹
tag_dir: tags #标签文件夹
archive_dir: archives #归档文件夹
category_dir: categories #分类文件夹
code_dir: downloads/code #Include code 文件夹
i18n_dir: :lang #国际化（i18n）文件夹
skip_render: #跳过指定文件的渲染，您可使用 glob 表达式来匹配路径。
```

### 文章

```bash
new_post_name: :title.md #新文章的文件名称
default_layout: post #预设布局
titlecase: false #把标题转换为 title case
external_link: true #在新标签中打开链接
filename_case: 0 #把文件名称转换为 (1) 小写或 (2) 大写
render_drafts: false #显示草稿
post_asset_folder: true #启动Asset文件夹
relative_link: false #把链接改为与根目录的相对位址
future: true #显示未来的文章
highlight: #代码块的设置
enable: true
line_number: true
auto_detect: false
tab_replace:
```

### 分类 & 标签

```bash
default_category: uncategorized #默认分类
category_map: #分类别名
tag_map: #标签别名
```

### 日期/时间 格式化

```bash
## Hexo 使用 Moment.js 来解析和显示时间。
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD #日期格式化
time_format: HH:mm:ss #日期格式化
```

### 分页

```bash
per_page: 10 #每页显示的文章量 (0 = 关闭分页功能)
pagination_dir: page #分页目录
```

### 扩展

```bash
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
theme: hexo-theme-random #当前主题名称。值为false时禁用主题
```

### 部署

```bash
deploy: #部署部分的设置
  type: git
  repo: https://webrenee7:github_xy0827@github.com/webrenee7/webrenee7.github.io.git
  branch: master
  message: push
```

## 我的基本配置
### 修改网站相关信息

```bash
title: 徐娅的个人博客
description: 一枚程序媛的成长之路
author: Xu Ya
```

### 配置个人域名(如果有的话)

```bash
url: http://xuya.site
```

### 配置部署

```bash
deploy:
    type: git
    repo: https://[username]:[pwd]@github.com/webrenee7/webrenee7.github.io.git
    branch: master
    message: push
```

注意：
- 配置文件中不能有注释
- 不能随意删除以上代码中的空格

## 更换主题
如果不喜欢默认的主题，可以到hexo官网找到自己喜欢的主题包，然后下载到themes中进行使用即可

### 克隆主题到themes

```bash
cd themes
git clone "[theme url]"
```

### 修改主题名称

打开根目录下的_config.yml文件修改：

```bash
theme:[theme name]
```

## 更换网页图标
在source目录下替换favicon.ico图片即可

## 新建文章
- 手动创建：
source/_posts目录下，新建note.md文件

- 命令创建(会自动生成文章标题，发表时间，标签信息) 推荐：
在项目根目录下执行：
    ```bash
    hexo new note.md
    ```
## 修改头像
在themes/hexo-theme-next/source/uploads中修改avatar.png图片
如果没有uploads目录，需要我们手动创建
修改主题配置文件：

```bash
avatar: /uploads/avatar.png
```

## 修改导航
编辑主题配置文件
**导航链接**

```bash
menu:
  home: /
  categories: /categories
  about: /about
  archives: /archives
  tags: /tags
  #sitemap: /sitemap.xml
  #commonweal: /404.html
```

需要哪个，把前面的#注释去了
编辑themes/hexo-theme-next/languages/zh-Hans.yml

## 导航文字

```bash
menu:
  home: 首页
  archives: 归档
  categories: 分类
  schedule: 日程
  tags: 标签
  about: 关于
  search: 搜索
  commonweal: 公益404
```

## 修改底部
编辑themes/hexo-theme-next/layout/_partials/下的footer.swig文件

## 图片的使用
```bash
![图片描述](图片地址)
```

- 使用目录下的图片
需要修改站点配置文件：

```bash
post_asset_folder:true
```

- 使用图片外链地址

## 链接的使用
```bash
[链接文字](链接地址)
```

## 新建页面
```bash
new hexo new page about
```

## 侧边栏
编辑主题配置文件：

```bash
toc:
  enable: true #侧边栏是否可用
  number: false #是否自动编号，默认编号
```

## 阅读全文
在文章中使用more手动进行截断(推荐)
在文章的 front-matter 中添加 description，并提供文章摘录
自动形成摘要，在 主题配置文件 中添加：

```bash
auto_excerpt:
  enable: true
  length: 150 #默认截取的长度为 150 字符，可以根据需要自行设定
```

## 阅读次数
- 注册LeanCloud账号
- 创建应用
- 权限配置（新建Class名必须为Counter）
- 在应用Key获取AppID以及AppKey
- 编辑主题配置文件：
    ```bash
    leancloud_visitors:
      enable: true
      app_id: joaeuuc4hsqudUUwx4gIvGF6-gzGzoHsz
      app_key: E9UJsJpw1omCHuS22PdSpKoh
    ```

## 百度统计
- 注册百度统计账号
- 定位到站点代码获取页面，复制代码添加到网站全部页面的标签前
- 复制hm.js? 后面那串统计脚本 id
- 编辑站点配置文件：
```bash
baidu_analytics: 31379905820c98af9f041ba3c2971602
```

## 站点建立时间
编辑主题配置文件：

```bash
since: 2017
```

## 订阅微信公众号
在themes/hexo-theme-next/source/uploads目录下添加wechat-qcode.jpg
wechat-qcode.jpg是你的微信公众号二维码图片
编辑主题配置文件：

```bash
wechat_subscriber:
  enabled: true
  qcode: /uploads/wechat-qcode.jpg
  description: 欢迎您扫一扫上面的微信公众号，订阅我的博客！
```

# 七牛云存储图片
七牛云是什么？

> 可以用它做图片托管，取外链写在文章中。

## 注册七牛云账户
七牛云官网：[http://www.qiniu.com/](http://www.qiniu.com/)
对于新用户，七牛云存储免费赠送10G 的使用空间+10G/月的流量，对于小博客来说，是完全够用了。

## 添加资源
在资源主页，点击对象存储模块的“立即添加”
![](http://ok9npmnqk.bkt.clouddn.com/1485231678200.png)

填写信息：
![](http://ok9npmnqk.bkt.clouddn.com/1485232448200.png)

上传图片
点击内容管理，上传图片：
![](http://ok9npmnqk.bkt.clouddn.com/1485232208195.png)

通过默认域名访问
空间概览里面会有默认的域名，也可以自定义域名，不过需要付费
![](http://ok9npmnqk.bkt.clouddn.com/1485231942836.png)

访问方式：默认域名/图片名称
上传图片的地址：[https://portal.qiniu.com/bucket/sevencattle/resource](https://portal.qiniu.com/bucket/sevencattle/resource)

# 部署博客
```bash
hexo generate//或者hexo g
hexo deploy
```

参考文档：[NexT使用文档](http://theme-next.iissnan.com/)
