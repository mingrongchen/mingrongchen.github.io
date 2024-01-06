---
title: Hexo基本使用
categories: 
  - 杂
tags:
  - 杂
about: 无
date: 2022-03-02 23:48:51
---

# Hexo基本使用

<!--more-->

### 安装

前提：

Node.js、Git

```shell
npm install -g hexo-cli
```

### 建站



```shell

```

- _config.yml ：网站配置信息
- package.json ：应用程序信息
- scaffolds：模板文件夹，创建文章时根据该模板建立文件
- source：资源文件夹，存放用户资源。以_（下划线）的文件、文件夹会被忽略，其他文件会被解析放到public文件夹下。
- themes：主题文件夹，根据主题来生成静态页面。

### 配置

_config.yml 

```shell
#网站参数
title： MingRong's Blog  #网站标题
subtitle: #网站副标题
description:    #网站描述
author:    #你的名字
language:    #网站语言
timezone:    #网站时区
#网址
url:    #网址
root:    #网站根目录
permalink: ’：year/:month/:day/:tile/‘    #文章永久链接格式
permalink_defaults:    #永久链接各部分默认值
# 网站存放在子目录
# 目录
source_dir:source    #资源文件夹
public_dir:public    #公共文件夹
tag_dir:tags    # 标签文件夹
archibe_dir:archives    #归档文件夹
category_dir:categories #分类文件夹
code_dir:downloads/code #include code文件夹
i18n_dir: ':lang' #国际化（i18n）文件夹
skip_render：#跳过指定文件夹的渲染可以使用glob表达式匹配路径
# 文章设置
new_post_name:':title.md'#新文章的文件名称
default_layout:post  #预设布局
auto_spacing:false  #在中英文之间加入空格
titlecase:false  #将标题转换为title case
external_lik：true #在新标签中打开链接
filename_case:0#把文件名称转换为1：小写，2：大写
render_drafts:false#显示草稿
post_asset_folder:false#启动asset文件夹
relative_link:flase  #把链接改为根目录的相对位址
future：true#显示未来的文章
highlight： #代码块的设置
# 分类&标签
default_category：#默认分类
category_map: #分类别名
tag_map:  #标签别名
#日期、时间格式
#分页
# 拓展
theme:next    #当前主题名称
deploy： #部署设置

```

### hexo指令

常用hexo指令

| 指令                            | 作用                                             |
| ------------------------------- | ------------------------------------------------ |
| hexo init [folder]              | 新建立一个网站                                   |
| hexo new [layout]<title>        | 新建一篇文章，若没有layout则为default_layout的值 |
| hexo generate \| hexo g         | 生成静态文件                                     |
| hexo publish [layout]<filename> | 发表草稿                                         |
| hexo server \| hexo s           | 启动服务器，默认为http://localhost:4000/         |
| hexo deploy \| hexo d           | 部署网站                                         |



### Next 主题使用

### 基础

```shell
#安装
cd <your-hexo-site>
git clone https://github.com/iissnan/hexo-theme-next themes/next

```

安装完成后在主站点配置文件中设置`theme: next`

在主题配置文件中可以选择4种不同外观配置

- Muse - 默认 Scheme，这是 NexT 最初的版本，黑白主调，大量留白
- Mist - Muse 的紧凑版本，整洁有序的单栏外观
- Pisces - 双栏 Scheme，小家碧玉似的清新
- Gemini-github主题

设置语言`language:zh-Hans(中文)\en(英文)`

```shell
#菜单设置
menu:
  home: /    #主页
  archives: /archives    # 归档类
  #about: /about     #关于页面
  #categories: /categories    # 分类页
  tags: /tags    # 标签页
  #commonweal: /404.html
#侧栏设置
sidebar:
  position: right
#设置头像
#放置在 source/images/ 目录下 
#配置为：avatar: /images/avatar.png
#作者昵称
author: <your name>
# 站点描述
description:  <你喜欢的签名>

```





