+++
title = '使用Github Pages和Hugo搭建个人博客'
draft = false
toc = true
tocBorder = true
+++

## 一、创建一个Github Pages类型的Repo

在Github上创建一个新的Repository，仓库名称为`xxx.github.io`，其中xxx为自定义名称。仓库设置为Public，并且默认添加一个README文件。
仓库创建好后通过git clone下到本地备用

## 二、本地下载Hugo

到Hugo官网上通过指导下载Hugo https://gohugo.io/installation/
Windows通过winget下载更方便
```
winget install Hugo.Hugo.Extended
```

## 三、挑选并自定义Hugo主题

到Hugo主题网站上挑选喜欢的主题 https://themes.gohugo.io/
大部分主题会在主页上写上安装使用方法，小部分简约型的主题安装比较简单，只需要将你喜欢的主题的Github仓库clone到本地，并覆盖之前新建的Github.io的仓库目录

## 四、本地运行查看效果

进入仓库目录，可以执行`hugo`编译静态网站，并通过`hugo server`在本地上查看生效效果。

## 五、推送Github仓库

本地准备好之后，可以直接推送到github的远端分支上

## 六、使用Github Actions提交后自动发布

1、生成Person access tokens
Github进入Settings -> Developer Settings -> Personal acces tokens -> Token(classic)
点击Generate new token创建一个新的token，Expiration选择`No expiration`，scopes勾选上`repo`、`admin:repo_hook`
复制保存新生成的token

2、生成Actions secrets
Github进入刚创建的仓库，点击Settings -> Security -> Secrets and variables -> Actions
点击New secret，名称自取，Secret粘贴生成的Personal access tokens

3、配置actions文件
在仓库根目录下创建文件夹`.github`以及子文件夹`workflows`，新建yml格式文件，名称自取，详细内容可以参考官网https://gohugo.io/hosting-and-deployment/hosting-on-github/
在yml文件中执行需要发布的文件夹，一般是public，以及需要发布到哪个分支

4、Github pages设置
Github进入创建的仓库，进入Settings -> Pages。设置Branch为yml文件里指定的分支

5、本地修改推送后，可以看到自动化流水线跑起来了，并且跑完之后内容刷新到了指定的分支上，master分支内容为最原始的内容

6、也可以使用Github的CodeSpaces功能快速编辑，这样就不再需要将仓库克隆到本地后再编辑了，更加方便

## 七、References

[如何使用Github Pages + Hugo搭建个人博客](https://krislinzhao.github.io/docs/create-a-wesite-using-github-pages-and-hugo/)



