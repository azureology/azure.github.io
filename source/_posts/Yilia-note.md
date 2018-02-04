---
title: 搭建博客踩过的坑
date: 2018-02-04 12:46:21
tags: [技术,GitHub,Hexo]
---

搭建了属于自己的GitHub Page作为个人博客，上手Hexo+Material感觉比较花哨，后改用Yilia主题并决定长期使用。期间发现并解决了一些问题，贴出值得记录的部分作为参考：
- 维护
- 书写
- 内容
- 标签
- 图床

<!-- more-->
正文开始：

## 维护
在本地安装后Hexo后每次deploy到master branch有很大的局限性，换台电脑更新博客很麻烦。后来借鉴其他网友做法：新建Hexo分支并将repository的默认branch设为Hexo，每次更新将Hexo全部文件commit至Hexo分支，Hexo自带`.gitingore`文件无须担心提交不必要文件。换电脑只需clone一份到本地，需要更新博客时调用`hexo d`命令更新master分支，省时省力。

## 书写
试用了不少支持MarkDown语法的编辑器，最后停留在了全平台兼容的*Visual Studio Code*，据说就是套了壳的*Atom*……不管那么多，反正Mac端和PC体验高度一致，支持的扩展也非常丰富。平时都用[Ayu Dark](https://marketplace.visualstudio.com/items?itemName=teabyii.ayu)主题，实现MarkDown的快速书写和实时预览依赖以下两款插插件：

- [Markdown Preview Enhanced](https://marketplace.visualstudio.com/items?itemName=shd101wyy.markdown-preview-enhanced)
- [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one)

前者有丰富的热键支持，后者可以实现分屏预览。

## 内容
Hexo使用三方主题具体配置需要修改theme对应的`_config.yml`文件，其中包括个人信息、社交账号以及主页个性化等设置。

```
# Content

# 文章太长，截断按钮文字 
excerpt_link: more
# 文章卡片右下角常驻链接，不需要请设置为false
show_all_link: '展开全文'
# 数学公式
mathjax: false
# 是否在新窗口打开链接
open_in_new: false
```
一开始不懂为什么首页会展示全文无法自动折叠，仔细阅读设置文件并参考其他博主写法后明白了。
用法：在书写md博文时 插入`<!-- more--> `后续内容均会被折叠。
像这样：

![效果图](https://s1.ax2x.com/2018/02/04/hSqXR.png)

## 标签
使用`hexo　new <title> `命令建立的md文件默认会带三行注释，可以自定义标题、时间和标签。  
如需多标签写成`tags: [技术,GitHub,Hexo]`形式即可。  
**注意：冒号后面一定要空一格，格式与配置yml相同，不空格会报错无法生成博文！**  

## 图床
作为一个优秀的代码托管网站，个人不建议将博文配图全部上传至GitHub仓库内保存，大量配图请另寻图床。收藏夹给出了两个免费图床，也可以用微博图床（不知道新浪能放任多久）。至于网站图标、用户头像和二维码等个性化图片可存放于`/source/assets/img`下与主题分开，更新主题不会导致覆盖，备份起来也更方便。记得修改主题配置文件~

这里推荐一个图床工具`iPic`支持调用常见图床一键生成Markdown插图代码，不过目前只有Mac版。
