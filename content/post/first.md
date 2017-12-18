---
title: "转移静态BLOG"
date: 2017-12-15T13:06:01+08:00
draft: true
---
## 转移原因

由于考虑到平时写文档都是markdown
使用django后台动态生成比较慢。而且github还有page服务。直接使用静态网站

***

## 选择hugo的原因

谷歌当家的go语言编写的静态网站生成器,上手简单,速度比Jekyll快

## 搭建过程

- 首先下载安装go。
    1. mac 直接brew,debian apt-get.
    2. 其他系统参考github
- 安装hugo。
    去官网下载即可

- 上github创建page库

- 选择hugo主题
    选个自己喜欢的即可

- 配置config.toml文件
    直接按着主题的配置文件复制更改即可。

## 自动build
搭建好githubpage后如果每次更新md文章还要自己手动build跟commit就太low了。
按照官方文档，可以使用wercker进行自动build
还有一个方法，使用github的webhook功能来进行自动化build(需要写脚本)

- 编写`wercker.yml`文件进行配置。
直接贴我自己的

```
box: debian
build:
    # Steps make up the actions in your pipeline
    # Read more about steps on our dev center:
    # http://devcenter.wercker.com/docs/steps/index.html
  steps:
    - arjen/hugo-build:
        theme: hugo-strata-theme
        flags: --buildDrafts=true

deploy:
    steps:
        - install-packages:
            packages: git ssh-client
        - lukevivier/gh-pages@0.2.1:
            token: $GIT_TOKEN
            repo: xerxesnoPT/xerxesnoPT.github.io
            branch: master
            domain: xerxesnopt.github.io
            basedir: public

```
- 注意：这里要配置GIT_TOKEN 在wercker环境变量中。

- 上传`wercker.yml`到github库中.接下来配置wercker的工作流.顺序为先build在deploy.

![屏幕快照 2017-12-18 上午11.53.45.png](http://upload-images.jianshu.io/upload_images/6865906-c1d6b946726476f2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 完成.
每次更新静态网站,新增博客只需commit到wercker关联仓库即可.会自动build然后上传到githubpage对应的仓库中.
![屏幕快照 2017-12-18 上午11.53.33.png](http://upload-images.jianshu.io/upload_images/6865906-55f13855639a3a04.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
<br/>
<br/>
<br/>
![屏幕快照 2017-12-18 上午11.54.20.png](http://upload-images.jianshu.io/upload_images/6865906-c1e6ce3c377171a9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 附注：
我选择的这个主题集成了nodejs的项目formspree.解决了静态网站收集表单信息的不便。
<br/>
<br/>
![image.png](http://upload-images.jianshu.io/upload_images/6865906-f91b42accf9bdc9c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

