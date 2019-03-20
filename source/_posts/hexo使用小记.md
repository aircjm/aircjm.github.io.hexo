---
title: hexo使用小记
date: 2016-04-17 21:33:05
updated: 2016-06-30 17:38
categories: 教程
tags: 
- 教程
- hexo
- next
---
记录下如何更好的使用Hexo，以及Hexo的一些配置。



## 添加域名绑定功能
直接在github库中添加CNAME文件，发现每次执行`hexo d`之后，CNAME文件就被覆盖了，所有直接在库中添加CNAME文件的方式来指定域名是不行的。
不能直接在库中添加CNAME文件，就只能在`hexo site`我们的hexo项目中想办法了，在我们的`hexo site`中 source文件夹中添加CNAME文件，和在库中添加一样你的域名，之后，执行
```bash
hexo clean 
hexo g
hexo d
```

你就会发现你的CNAME文件已经提交上去了，访问下你的域名看看是不是已经转到你的blog主页了。


## 添加README.md
因为README.md文件会被hexo进行渲染所以会影响我们的使用体验.
由于我的hexo的不是部署在master分支上的,所以默认使用的README.md文件是在默认分支根目录下面的,但是有的部署静态页面的分支使用的是master分支,如何在master分支上面添加一个README.md文件呢,
在source文件夹下面添加README.md文件,找到hexo项目的配置文件`_config.yml`找到`skip_render: `修改为`skip_render: README.md`.
现在重新解析部署提交,登陆远程仓库就能看到READE.md文件的效果了.

## 使用markdown插入本地图片
由于编译后的路径使用的是日期等等,所以为了本地和远程使用都可以使用图片,在source中添加images文件夹,添加一个图片favicon.png,如何使用这个图片呢,
在博客里使用这张图片，markdown格式与使用网络图片的格式相同：
```
![](图片链接)
```

在这里，图片链接写入本地路径，就是在这里出现了一些小问题。
最开始我写的是：
```
../img/favicon.png
```
但是这样的话使用图片的路径为
```java
http://localhost:4000/2016/04/17/images/favicon.png
```
所以使用下面的路径来使用:
```
![](../../../../images/favicon.png)
```
编译部署,在远程库上查看.
再上传之后，发现成功地显示图片了。

### 参考:
[hexo使用markdown插入本地图片](http://hugowen.com/post/hexo-insert-local_pictures.html)



## 遇到的坑-1 页面无法显示，主题文件夹丢失

本来使用git来进行版本控制的，但是由于使用的主题是Next的git进行检出的，所以我在将hexoBlog项目推送到远程库的时候，并没有将themes下的next文件夹推送到远程库，所以在另一台电脑上进行部署时`hexo s`,访问blog是空白页，后台报无法找到index.html
![遇到的坑-1 页面无法显示，主题文件夹丢失图1.png](http://upload-images.jianshu.io/upload_images/1013655-7984585b6921e3a4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

所以建议使用next的时候，去github上下载稳定版本，不进行git checkout拉取代码。



***<u>update 2016-06-30</u>***

## 添加本地搜索（基于Next主题）

添加百度/谷歌/本地 自定义站点内容搜索

1. 安装 `hexo-generator-search`，在站点的根目录下执行以下命令：

   ```yaml
   npm install hexo-generator-search --save
   ```

2. 编辑 站点配置文件`_config.yml`（不是主题文件夹里面的），新增以下内容到任意位置：

   ```yaml
   search:
     path: search.xml
     field: post
   ```

显示效果：
![hexo添加本地搜索.png](http://upload-images.jianshu.io/upload_images/1013655-d9a7138c9db65fa5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 添加字数统计插件

首先在Hexo项目目录下安装：`npm install hexo-wordcount --save`。

在footer.swig文件中加入下面代码

```javascript
<div class="theme-info">  <div class="powered-by"></div>
  <span class="post-count">博客全站共{{ totalcount(site) }}字</span>
</div>
```

## 添加文章末尾版权声明

找到post.swig文件，在footer.post-footer中添加如下代码。

```javascript
<footer class="post-footer">
    {% if not is_index %}
        <div class="copyright" style="clear:both;">
           <p><span>本文标题:</span><a href="{{ url_for(post.path) }}">{{ post.title }}</a></p>
           <p><span>文章作者:</span><a href="/" title="访问 {{ theme.author }} 的个人博客">{{ theme.author }}</a></p>
           <p><span>发布时间:</span>{{ post.date.format("YYYY年M月D日 - HH时MM分") }}</p>
           <p><span>本文字数:</span><span class="page-count">本文一共有{{ wordcount(page.content) }}字</span></p>
           <p><span>原始链接:</span><a href="{{ url_for(post.path) }}" title="{{ post.title }}">{{ post.permalink }}</a></p>
           <p><span>许可协议:</span><i class="fa fa-creative-commons"></i> <a rel="license" href="http://creativecommons.org/licenses/by-nc/4.0/" title="Attribution-NonCommercial 4.0 International (CC BY-NC 4.0)">Attribution-NonCommercial 4.0</a></p>
           <p><span>转载请保留以上信息。</span></p>
        </div>
      {% endif %}
</footer>
```

然后需要修改一下样式，找到`themes\next\source\css_common\components\post\post.styl`,加入如下样式

```javascript
.post-footer .copyright{  padding-top: 1.5em;
  padding-left: 1em;  font-size: 12px;
  line-height: 1em;
  border:1px solid #ccc;

}
```

### 参考:

[Hexo更新日志](http://crossingmay.com/2016/04/20/updatehexo/)

