---
title: Github Pages + Jekyll搭建博客
author: bjkdtc
date: 2017-06-14T15:16:52+00:00

categories:
  - 野记
  - 野路子
tags:
  - WeekDiary
  - 技术

---
## 背景

之前在搭建自己的Wordpress的时候，就看过Github有静态Pages发布的功能，当时简单的以为可以把Wordpress发布在Github pages上面，这次替女朋友搭建她的个人小站的时候想想内容不多可以通过这种方式快速搭建，就来尝试一下。当然首先就摔了一个坑，Github pages只是静态页面发布，没有php，没有SQL，并不能搭建Wordpress环境。

## 开始

不能简单的使用Wordpress，只能按照Github Pages的推荐配置利用Github Pages+Jekyll来搭建博客。当然查阅网上Jekyll并不是唯一的选择，其他方式也很多，不过介于Jekyll是官方推荐的方式，我也选择这个搭配作为这次尝试的方案。

### 注册Github Pages

首先我到https://pages.github.com去查看了Github pages的相关向导内容。根据Pages的介绍其实可以直接通过Github pages的向导来创建Pages内容，不过因为是给女朋友（0IT背景）弄，我就重新从Github上注册了一个新的账户。之后利用Git Bash创建了一个新的项目，并上传到了Github空间，这里唯一需要注意的是项目名称的起始必须和Github的用户名一致，例如用户名是username，那么这个Github的项目名必须为username.github.io。  
通过点击项目名，再点击Settings，在项目设置中首先确认项目名是否和username一致，再往下进入Github Pages选项。如果配置正确的话则会显示  
`your site is published at https://username.github.io`  
理论上此时应该已经可以通过github pages看到自己的页面了。

### 注册自己的域名

我之前在`https://www.namecheap.com`已经购买了自己的域名，所以这次女朋友的域名也买在了同一个账户上。 买完域名后需要绑定DNS。这里搞了半天的一级域名和二级域名。简单的理解一级域名是没有www的，二级域名是带有www或者在域名前方有一层内容，三级域名则是前面有两层。

  * 只需要一级域名：在github pages的设置中，通过github pages直接有 `custom domain`可以把域名填进去。而对DNS的设置，参考`https://help.github.com/articles/setting-up-an-apex-domain/`，配置一个&#8221;A&#8221;记录。而和“A”记录对应的IP地址有两个192.30.252.153和192.30.252.154。

  * 只需要二级域名：理论上需要配置一个单独的文件CNAME，但是如果通过前一步已经配置了github pages的custom domain则github pages会为你创建好这个文件。此时就参考`https://help.github.com/articles/setting-up-a-www-subdomain/`，在`www.namecheap.com`上配置好CNAME 记录。

截止到这里github pages的相关发布都已经完成。接下来要开始做Jekyll

### 使用Jekyll

Jekyll是一个专门将纯文本转换成静态网站和博客的包。目前有完整的官网 `http://jekyll.com.cn/`。引用官网的介绍:

`Jekyll 是一个简单的博客形态的静态站点生产机器。它有一个模版目录，其中包含原始文本格式的文档，通过 Markdown （或者 Textile） 以及 Liquid 转化成一个完整的可发布的静态网站，你可以发布在任何你喜爱的服务器上。Jekyll 也可以运行在 GitHub Page 上，也就是说，你可以使用 GitHub 的服务来搭建你的项目页面、博客或者网站，而且是完全免费的。`

由于整个Jekyll是基于Ruby的所以必须配置Ruby环境（包括RubyGems)。很不幸的是Ruby原生不支持Windows环境，且第三方支持Ruby的环境配置并不简单，所以最好的办法是用MacOS搞。在我自己的操作过程中，由于自己的脑残行为把Mac放在了单位，只能硬着头皮在家里的WIndows电脑上搞了一台Ubuntu的虚拟机（是的，我就是这么作）来配置。

具体安装Ruby,RubyGems和Jekyll的过程可以操作参考官网 `http://jekyll.com.cn/docs/installation/`上面有非常详细的过程。当然我相信大部分的用户并不想从零搞起，那么可以采用`http://jekyllthemes.org/`里面的模板。

下载完自己喜欢的模板后，在模板中调用 `jekyll serve` 就可以在 `http://localhost:4000/`中看到原有模板的样子。

Jekyll最牛的地方就是所有的配置都集中在了_config.yim中。通过配置这个文件可以设置全局变量，具体的配置参考 `http://jekyll.com.cn/docs/configuration/`。 如果基于配置那么应该可以发布博客了，但是如果需要做一些个性化配置，那么建议先从文档结构`http://jekyll.com.cn/docs/structure/`了解\_includes、\_layouts和index.html代码，修改相关的图片、颜色、CSS等内容。

基本上在不需要大幅调整的情况下使用Jekyll，根据官网的内容就已经足够。

### Jekyll模板遇到的问题

在使用Jekyll模板的时候我遇到最多的问题来自于模板使用的Ruby模块过期。由于很多模板的创建时间远早于现在，而且他们并不会时刻更新自己的模板。那么此时最好的办法是先用gem来更新一下源包。  
首先 `gem sources --add https://gems.ruby-china.org/ --remove https://rubygems.org/` 将gem源变位中国源  
然后用 `gem update` 更新所有包或者用它更新对应的包。那么基本能解决所有问题。