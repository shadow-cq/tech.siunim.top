---
title: Opt 4 Next theme
date: 2019-10-29 12:11:47
tags: [Hexo,Next Theme]
categories: [study,blog]
---


# 博客主题优化 （ Next ）

## 添加点击小红心效果

在`/themes/next/source/js/src`下新建文件` clicklove.js` ，接着把下面的代码拷贝粘贴到 `clicklove.js` 文件中：
```
!function (e, t, a) {
    function n() {
        c(".heart{width: 10px;height: 10px;position: fixed;background: #f00;transform: rotate(45deg);-webkit-transform: rotate(45deg);-moz-transform: rotate(45deg);}.heart:after,.heart:before{content: '';width: inherit;height: inherit;background: inherit;border-radius: 50%;-webkit-border-radius: 50%;-moz-border-radius: 50%;position: fixed;}.heart:after{top: -5px;}.heart:before{left: -5px;}"), o(), r()
    } function r() {
        for (var e = 0; e < d.length; e++)d[e].alpha <= 0 ? (t.body.removeChild(d[e].el), d.splice(e, 1)) : (d[e].y-- , d[e].scale += .004, d[e].alpha -= .013, d[e].el.style.cssText = "left:" + d[e].x + "px;top:" + d[e].y + "px;opacity:" + d[e].alpha + ";transform:scale(" + d[e].scale + "," + d[e].scale + ") rotate(45deg);background:" + d[e].color + ";z-index:99999");
        requestAnimationFrame(r)
    } function o() {
        var t = "function" == typeof e.onclick && e.onclick; e.onclick = function (e) { t && t(), i(e) }
    } function i(e) {
        var a = t.createElement("div");
        a.className = "heart", d.push({ el: a, x: e.clientX - 5, y: e.clientY - 5, scale: 1, alpha: 1, color: s() }), t.body.appendChild(a)
    } function c(e) {
        var a = t.createElement("style"); a.type = "text/css"; try { a.appendChild(t.createTextNode(e)) } catch (t) { a.styleSheet.cssText = e } t.getElementsByTagName("head")[0].appendChild(a)
    } function s() {
        return "rgb(" + ~~(255 * Math.random()) + "," + ~~(255 * Math.random()) + "," + ~~(255 * Math.random()) + ")"
    } var d = [];
    e.requestAnimationFrame = function () {
        return e.requestAnimationFrame || e.webkitRequestAnimationFrame || e.mozRequestAnimationFrame || e.oRequestAnimationFrame || e.msRequestAnimationFrame || function (e) { setTimeout(e, 1e3 / 60) }
    }(), n()
}(window, document);
```
在`\themes\next\layout\_layout.swig`文件末尾添加：
```
<script type="text/javascript" src="/js/src/clicklove.js"></script>
```
预览一下，大功告成
```
hexo s
```

## 动态背景：变换线


Enable module in **NexT** `_config.yml` file:

```yml
canvas_nest:
  enable: true
  onmobile: true # display on mobile or not
  color: '0,0,255' # RGB values, use ',' to separate
  opacity: 0.5 # the opacity of line: 0~1
  zIndex: -1 # z-index property of the background
  count: 99 # the number of lines
```

**And, if you wants to use the CDN, then need to set:**

```yml
vendors:
  ...
  canvas_nest: //cdn.jsdelivr.net/gh/theme-next/theme-next-canvas-nest@latest/canvas-nest.min.js
  canvas_nest_nomobile: //cdn.jsdelivr.net/gh/theme-next/theme-next-canvas-nest@latest/canvas-nest-nomobile.min.js
```

## 作者头像设置


如何设置头像呢?

你可以把头像文件放在博客主目录`/themes/next/source/images`目录下，然后在主题配置文件中:


```avatar:
  # in theme directory(source/images): /images/avatar.gif
  # in site  directory(source/uploads): /uploads/avatar.gif
  # You can also use other linking images.
  url: /images/author.jpg  #在这里填写我们刚放的图片~
  # If true, the avatar would be dispalyed in circle.
  rounded: true #开启鼠标进入时的旋转特效~
  # The value of opacity should be choose from 0 to 1 to set the opacity of the avatar.
  opacity: 1 #这个是缩放,,可以不设置
  # If true, the avatar would be rotated with the cursor.
  rotated: true #这个用来设置头像是否为圆形显示,true为圆形哦
```
然后我们`hexo s`一下, 就能看见我们的头像出现啦! 现在把鼠标移上去, 是不是就能看见头像顺溜的转了一圈啦?

## 文章结束标志
在路径 `\themes\next\layout\_macro` 中新建一个叫 `passage-end-tag.swig `文件, 注意后缀要正确哦, 然后把下面的代码复制进去:


```<div>
    {% if not is_index %}
        <div style="text-align:center;color: #ccc;font-size:14px;">你想在文章末尾对读者说的话</div>
    {% endif %}
</div>

```
接着我们打开`\themes\next\layout\_macro\post.swig`文件, 在`post-body `之后你会看见一个`END POST BODY`, 然后继续往下翻, 最后在` post-footer `之前把这些代码粘贴进去:


```
<div>
  {% if not is_index %}
    {% include 'passage-end-tag.swig' %}
  {% endif %}
</div>
```
最后我们打开主题配置文件`_config.yml`,在末尾添加下面的代码：


```passage_end_tag:
  enabled: true
```
大功告成啦!!!


## Social 信息（sidebar）

OK, 现在我们可以设置一下主题配置文件, 其中social表示社交信息, 我们可以填入我们相关的链接, 格式为链接名: 链接地址 || 链接图标, 其中链接图标必须是FontAwesome网站中存在的图标名哦, 否则在页面中是无法显示的!



## 更改favicon

favicon是网站图标(favorites icon)的缩写

主题配置文件：

```
favicon:
  small: /images/favicon-16x16-next.png
  medium: /images/favicon-32x32-next.png
  apple_touch_icon: /images/apple-touch-icon-next.png
  safari_pinned_tab: /images/logo.svg
  #android_manifest: /images/manifest.json
  #ms_browserconfig: /images/browserconfig.xml
```
于是我们打开source/images/很容易就能找到以上的几个文件，将他们修改成你的网站图标，png可以用ps等软件，svg是矢量图，推荐用ai，不会做矢量图的话直接将配置文件里的后缀名改掉就行了

## rss订阅
将主题配置文件中的`rss`留空即可

在博客目录中安装`hexo-generator-feed`

```
npm install hexo-generator-feed --save
```
他会自动生成`atom.xml`

## 网页底部
### 显示版权信息和年份
主题配置文件：


```
footer:
  # 这里填网站建立的时间
  # 如果不写默认为当前年份
  since: 2018

```
版权信息：


```
# ©️年份后面的版权信息
copyright: RyTinSelver

 powered:
   # Hexo链接(Powered by Hexo).
   enable: true
   # Hexo版本信息(vX.X.X)
   version: true

 theme:
	# 主题信息(比如：Theme - NexT.scheme).
	enable: true
	# 主题版本信息(vX.X.X).
	version: true

```
自定义版权信息：


```
# 随便写什么都可以
 custom_text: Hosted by <a href="https://pages.coding.me" class="theme-link" rel="noopener" targ
```
### 年份和版权信息中间显示跳动的爱心
主题配置文件：


```
# 显示在年份和版权信息中间
  icon:
    # 红色的心(heart)(#ff0000)推荐使用动画.
    name: heart
    # 给图标添加动画
    animated: true
    # 16进制RGB颜色
    color: "#ff0000"
```
如果要使用其它图案，可以到[fontawesome](https://fontawesome.com/v4.7.0/icons/)官网查找

### 备案信息
在网页底部显示网站备案信息

主题配置文件：

```
beian:
   enable: true
   icp: XICP备XXXXXXXX号
```
### 文章知识版权
主题配置文件：

```
creative_commons:
  license: by-nc-sa
  sidebar: true #是否在侧边栏显示图标
  post: true 	#是否在文章底部显示版权信息
  language: deed.zh #语言，可用的参数有deed.zh, deed.fr, deed.de 等
```
支持的版权组合：by | by-nc | by-nc-nd | by-nc-sa | by-nd | by-sa | zero

1. 署名（Attribution，简写为BY）：必须提到原作者。

2. 非商业用途（Noncommercial，简写为NC）：不得用于盈利性目的。

3. 禁止演绎（No Derivative Works，简写为ND）：不得修改原作品, 不得再创作。

4. 相同方式共享（Share Alike，简写为SA）：允许修改原作品，但必须使用相同的许可证发布。
## SEO优化
主题配置文件：

```
# 整合重复的页面，在你的hexo中设置一个权威标签，可以优化博客的seo
# 请参阅：https://support.google.com/webmasters/answer/139066
# 提示：打开此选项之前，请确保在网站配置文件中设置了URL（例如：url:http://yourdomain.com）
canonical: true

# 更改网站副标题（网站描述）和所有文章/页面标题的结构，以优化SEO
seo: true

# 如果为true，则会在标签页显示副标题，副标题在网站配置文件中设置
# subtitle: Subtitle
index_with_subtitle: true

# 自动添加带有base64加密和解密的外部URL
exturl: true
```

## 导航栏菜单
主题配置文件：

```
# '||'后面是fontawesome图标名称，可以到https://fontawesome.com/v4.7.0/icons/查找
# '/'代表根目录，即网站配置文件中的URL（例如：url:http://yourdomain.com）
menu:
  home: / || home
  about: /about/ || user
  tags: /tags/ || tags
  categories: /categories/ || th
  archives: /archives/ || archive
  minecraft: https://mc.rytinselver.com/ || cube # 外链前必须有http://或https://
  #schedule: /schedule/ || calendar
  #sitemap: /sitemap.xml || sitemap
  #commonweal: /404/ || heartbeat
  # 自定义页面如下，使用时去掉#注释
  #页面名: /页面名/ ||图标名

menu_settings:
  icons: true # 显示图标
  badges: true # 显示标签

```
自定义页面：

命令行进入博客目录

```
$ hexo new page 页面名
```
然后就可以在主题配置文件中添加了

这样还不够，如果你的网站是中文或者其他语言的话，我们需要修改语言文件，让他适配我们需要的语言

打开`themes/next/languages`，选择你需要的语言文件，默认语言文件为`en.yml`，简体中文是`zh-CN.yml`，其他语言请自行查找

修改这部分即可

```
menu:
  home: 首页
  archives: 归档
  categories: 分类
  tags: 标签
  about: 关于
  search: 搜索
  schedule: 日程表
  sitemap: 站点地图
  commonweal: 公益 404
  minecraft: Minecraft
```
