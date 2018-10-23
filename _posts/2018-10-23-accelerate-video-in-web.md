---
layout: post
title:  "在浏览器中倍速播放百度云视频"
categories: 黑科技
tags:  黑科技 小技巧 
author: a605605
---

* content
{:toc}

## 残酷的现实

现在很多学习视频资源都存在百度云中，但是如果一个一个正常速度地看下去得看到猴年马月。为了在有限的时间里看更多的视频，倍速播放是最好的选择。不过，百度云对下载文件的速度做了限制，一般只有几K每秒，下完视频也得到猴年马月，而且经常会下载失败。所以，如果能实现在线对百度云的视频加速，那是再好不过了。

## 实现过程

1、首先，用Google浏览器打开百度云，进去之后随便点开一个视频，然后按`F12`进入开发者模式。

2、接着，按`Ctrl` + `Shift` + `c`，然后将鼠标移到视频框中点一下，就会自动定位到视频播放器对应的HTML代码。

<div style="text-align:center;">
    <img src="https://note.youdao.com/yws/public/resource/caa62601cfb1124c33413b88decdc00d/xmlnote/WEBRESOURCE444f21bc33d6a1034f79104b2348bb34/61" />
    <div>查看百度云网页视频播放器对应的HTML源码</div>
</div>

3、从图中可以看出百度云用的网页视频播放器对应的`id`和`class`都是`video-player`，这个`video-player`的子节点对应的`class`包括`video-js`因此很明显百度云使用了`video.js`作为视频播放的脚本，其`id`是`html5player`因此又能知道这是H5播放器

4、既然知道了百度云视频播放使用了`video-js`，那么便可以尝试调用`video-js`库里的相关命令来达到倍速播放的效果。

5、在控制台输入如下代码

```js
videojs.getPlayers("video-player").html5player.tech_.setPlaybackRate(2)
```

其中`setPlaybackRate(2)`表示将`video-player`的播放速度变为原来的两倍，当然也可以改成其他值。

输入代码后，单击`Properties`可以看到`playbackrate`属性值变为了`2`

<div style="text-align:center;">
    <img src="https://note.youdao.com/yws/public/resource/caa62601cfb1124c33413b88decdc00d/xmlnote/WEBRESOURCE76d72eb7d120233ff7c9eeb78f59beb6/70" />
</div>

6、这时候，就能发现视频已经可以倍速播放了。
