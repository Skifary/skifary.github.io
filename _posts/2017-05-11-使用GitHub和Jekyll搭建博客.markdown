---
layout: default
title:  "使用GitHub和Jekyll搭建博客"
date:   2017-05-11 21:04:15
categories: technology
---

# 使用GitHub和Jekyll搭建博客
## 前言

很久以前就想写博客，但是一直没有毅力把这件事做下去，现在开始拒绝眼高手低，开始记录自己的点点滴滴。  

本文介绍如何使用GitHub Pages+Jekyll搭建和使用个人博客，包括GitHub Pages搭建，域名替换，Jekyll本地测试环境搭建等方面，搭建环境为`macOS`。  

## GitHub Pages

### GitHub Pages简介
GitHub Pages是GitHub提供的一项个人站点服务。只需要一个GitHub账号，我们就可以搭建一个属于个人的站点。详情介绍请参考[这里](https://pages.github.com/)

### GitHub Pages搭建
1. 首先需要一个GitHub账号，如果没有点击[这里](https://github.com/)注册。

2. 创建一个新repository，需要注意的是项目名字格式为`username.github.io`，`username`一定要是你的账户名，格式不能错误。如下图所示![](/images/technology/使用GitHub和Jekyll搭建博客/1.jpeg)

3. 进入`设置页`![](/images/technology/使用GitHub和Jekyll搭建博客/2.jpeg)  
  
4. 修改以下两个`属性`![](/images/technology/使用GitHub和Jekyll搭建博客/3.jpeg)

5. 保存后使用你的地址`username.github.io`就可以访问你的页面了。

## 修改域名

`username.github.io`这个域名可能不符合我们的要求，在追求个性的今天我们也应该弄一个个性的域名，这里以阿里云为例。
  
### 申请域名

如果已经拥有域名，请跳过。没有请点击[阿里云域名申请](https://wanwang.aliyun.com/?utm_medium=text&utm_source=bdbrandww&utm_campaign=bdbrand&utm_content=se_103066) 查询和申请自己的个性域名。 
 
### 修改DNS
在域名申请完以后，进入`控制台`，点击刚才申请的域名进入`解析设置`，然后添加如下解析  

![](/images/technology/使用GitHub和Jekyll搭建博客/4.jpeg)  

记录类型为`CNAME`，主机记录为`www`(代表访问`www.username.com`，如果需要修改可以修改为其他，例如`blog`代表你的GitHub Pages页面的访问地址就是`blog.username.com`)，记录值为`username.github.io`，保存下来。  

将GitHub仓库同步到本地，添加一个名为`CNAME`的`纯文本文件`，注意不带任何后缀，里面的内容是上一步修改的域名(`www.username.com`)。完成后将本地修改提交到GitHub上。(如果不会相关操作，关于GitHub同步和提交请参考[这里](http://www.cnblogs.com/tinyphp/p/5025311.html))  

接下来使用你的新域名就可以访问到你之前创建的页面了。

> **注意：**如果访问不成功，**可能需要等待一会或者清除浏览器相关cookie**。

## Jekyll使用

看到别人的GitHub博客都是十分精美的，怎能不心痒呢？GitHub Pages天生支持Jekyll，那么我们就来看看Jekyll是如何使用的吧。

### Jekyll本地环境搭建

搭建Jekyll本地环境的目的是为了在发布前方便测试和查看效果。Jekyll本地环境搭建略微复杂，这里将从拷贝一个模板并成功在本地运行为例。

Jekyll themes有很多，[这里](http://jekyllthemes.org/)有很多人分享出来的模板，本文将以[scribble](https://github.com/muan/scribble)为例。

scribble的readme如下  

```sequence 
1. Fork the repository      
2. Clone the repository: git clone https://github.com/username/scribble
3. Run bundle install
4. Run Jekyll: bundle exec jekyll serve -w
5. Go to http://localhost:4000 for your site.
```
将scribble拷贝到本地以后cd到所在目录，执行命令`bundle install`发现没有这个命令：

```
command not found: bundle
```
经过一番查找发现：

```
Bundle介绍：
Rails 3中引入Bundle来管理项目中所有gem依赖，该命令只能在一个含有Gemfile的目录下执行，如rails 3项目的根目录。
```
于是我们开始安装`bundle`(安装需要`ruby`和`gem`，关于ruby和gem安装请参考[这里](http://www.cnblogs.com/daguo/p/4097263.html))：

```
  ~ gem install bundle
 ERROR:  While executing gem ... (Gem::FilePermissionError)
    You don't have write permissions for the /Library/Ruby/Gems/2.0.0 directory.

```
出现一个错误，很显然，我们没有权限去做这件事，于是我们加上权限：

```
 ~ sudo gem install bundle
 ERROR:  While executing gem ... (Errno::EPERM)
    Operation not permitted - /usr/bin/bundle
```
依然被拒绝了，这是为什么呢？可能我们依然没有权限去在`/usr/bin/`里面安装程序，查看一下本机`环境变量`：

```
 ~ echo $PATH
/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/bin:/usr/local/go/bin:/Users/Skifary/Go/bin:/Users/Skifary/.rvm/bin
```

那我们换一个地方装好了：

```
~ sudo gem install -n /usr/local/bin  bundle
Successfully installed bundler-1.14.6
Fetching: bundle-0.0.1.gem (100%)
Successfully installed bundle-0.0.1
Parsing documentation for bundler-1.14.6
Installing ri documentation for bundler-1.14.6
Parsing documentation for bundle-0.0.1
Installing ri documentation for bundle-0.0.1
2 gems installed
```
好了，这下装成功了。接下来安装`jekyll`，由上同理使用如下命令安装：

```
 ~ sudo gem install -n /usr/local/bin jekyll
```
安装完成后查看是否安装成功：

```
~ jekyll -v
jekyll 3.4.3
```
好了，接下来继续`scribble`中的步骤：

```
~ bundle install
```
安装成功后使用`bundle exec jekyll serve -w`，访问[http://localhost:4000](http://localhost:4000)即可查看效果啦。

### 将模板使用起来

jekyll本地环境搭建好了以后，把模板拷贝到自己的目录下，修改相关配置(主要是`_config.yml`)后使用`jekyll serve`就可以再[http://localhost:4000](http://localhost:4000)查看效果了，这样是不是很酷炫呢？

## 结语

到此为止我们就搭建了好了一个个人域名的博客，并具有了一个简单的样式。哈哈，还是很开心的~  

> 本文只将GitHub和Jekyll结合搭建博客的过程讲解了一下，关于Jekyll样式的修改就不在这里叙述了，有兴趣的同学可以参考[这里](http://www.jokinkuang.info/2016/09/03/how-to-create-the-jekyll-theme.html#github-pages环境本地化)和[中文官网](http://jekyll.com.cn/)详细的介绍。