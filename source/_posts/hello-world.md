---
title: Hello World
abbrlink: 4a17b156
date: 2019-10-22 12:11:47
tags: [blog,tech]
categories: [study,blog]
---

欢迎来到「小念头 Tech」
第一篇文章就记录下如何搭建的本博客吧
<!-- more -->
# GitHub + Netlify + Hexo 自动化部署网站

## Hexo 搭建本地 blog

安装 Hexo，具体就是官网去查看并安装，依赖 Git， Nodejs

生成 blog 目录 
```
hexo init "博客目录" #使用hexo命令在指定的<folder>文件夹下初始化创建一个博客项目
cd "博客目录"         #进入创建好的项目目录
npm install         #使用npm安装所需依赖.
```

修改主题为Next主题
```
git clone https://github.com/theme-next/hexo-theme-next.git themes/next 
rm -rf themes/landscape/
```

修改配置文件`_config.yml`，主题设置为Next
```
theme: next
```
本地预览
```
hexo s 
```

## 部署

当我们使用 `hexo g` 和 `hexo s` 命令生成并开启服务后,我们本地访问的测试域名-`http://localhost:4000` 实际是指向了我们当前目录下的 public 目录,也就是说 `hexo g` 命令会生成 public 目录,这个目录下面装着我们的静态页面文件和相关依赖,部署的过程就是将这个 public 目录下的文件放到我们的服务器上这样就完成了部署.

好了接下来我们来进行部署：


### deploy on Github

先到GitHub新建一个repository
复制你刚刚新建的repository的地址,像这样:
`https://github.com/shadow-cq/tech.siunim.top.git`
回到根目录下，创建 git 本地仓库，并创建 .gitignore 文件,`删除其余目录下的 .git 目录`
```
git init
rm -rf themes/next/.git
```
将不需要同步的文件和目录写到.gitignore:

```
.DS_Store
Thumbs.db
db.json
*.log
node_modules/
public/
.deploy*/
```

回到项目根目录,将你的本地项目和新建的repository联系起来并提交代码推送到 github :
```
git remote add origin https://github.com/shadow-cq/tech.siunim.top.git
git add *
git commit -m "message"
git push -u origin master
```
现在 Github 仓库已经设置完成，接下来就是在 Netlify 上的操作。

首先是在 Netlify 上注册账号，推荐直接使用 GitHub 账号注册。

然后选择创建新的网站，选择要和 Netlify 相连的 GitHub 仓库，这里我要连的是我的博客仓库 github.com/shadow-cq/tech.siunim.top 。

这里会让你设定部署的一些详细信息，Netlify 会识别到你使用的是 Hexo，一般默认即可。但这里需要注意一点，「Build command」中默认的命令是：
```
hexo generate
```
设定完成后，Netlify 就会对你的仓库进行第一次部署，如果出现部署错误，请根据提示内容进行修改。一般来说，如果本地浏览正常就不会出现错误。

部署好之后，Netlify 会自动给你的网站分配一个域名。这个域名是随机生成的，可以进行更改，比如我的网站域名就改成了：[https://tech.siunim.top](https://tech.siunim.top)，当然最开始的时候没有 HTTPS。这时候，你就可以选择自己设定个性域名。在「Deploy setting」中的「Domain managment」添加你要设定的域名。然后根据自己域名的解析服务商那里将该域名解析到 Netlify 上。这里我设定的域名即：tech.siunim.top，将`tech.siunim.top` CNAME 记录解析到那个原有的域名guanqr.netlify.com 中。具体的操作在你设定的时候会有提示。另外，Netlify 也提供了域名解析的服务，如果感兴趣的话可以自己进行尝试。

域名设定完成后，在该页面的末尾即为设定 HTTPS，Netlify 会免费为你的网站提供 HTTPS 证书。如果你自己已经购买过证书，也可设定添加。

## 其他的一些操作
### 实时编辑
NexT 主题已经在主题配置文件中提供了在线编辑的功能。即：

```
post_edit:
enable: true
url: https://github.com/shadow-cq/tech.siunim.top/edit/master/source/

```
这是我的设定，url后面填写的是你的仓库中存放文章的文件夹。设定完成后，可以看到在每一篇文章的右上角有一个「笔」的图标，点击后就可以跳转到你的仓库，进行实时编辑。


### 页面重定向 (暂时没有需求)

Netlify 提供了自定义页面重定向的功能。如果你的域名或者文章结构发生了变化，可以借助重定向功能，将原来的文章 URL 重定向到现在的地址。这时候就需要 Netlify 网站中所说的`netlify.toml`文件。
新建一个`netlify.toml`文件，存放在博客的根目录下。在里面添加以下内容：


```
[[redirects]]
  from = "https://原始域名/*"
  to = "https://自定义域名/:splat"
  force = true

# A redirect rule with all the supported properties
[[redirects]]
  from = "/old-path"
  to = "/new-path"
```

## 参考

[博客通过 Netlify 实现持续集成](https://www.guanqr.com/2019/10/04/deploy-blog-to-netlify/index.html?__WB_REVISION__=eb66cb9a7103f6931702555b22d8aa67)
[https://www.netlify.com/docs/netlify-toml-reference/](https://www.netlify.com/docs/netlify-toml-reference/)
