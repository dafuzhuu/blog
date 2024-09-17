+++
title = '如何在Hugo上添加本地视频'
date = 2024-09-17T21:04:06+08:00
tags = ["Hugo"]
draft = false
+++

首先，你需要知道html是可以承载视频的，代码为

```html
<video src="video.mp4" controls="controls" width="640" height="480">
  <source src="/videos/example.mp4" type="video/mp4">
</video>
```

其中，src属性指定了视频文件的路径，controls属性可以让用户控制视频的播放。视频需要放在 `/static/videos` 目录下，可以放在子文件夹中。

如果把html代码直接放进Hugo的markdown文件中，视频是可以播放的，因此这里不存在技术上无法实现的问题，而是说怎样写的更好看。

这就引出 shortcode 的作用了，它的本质是传参，在html文件中提前写好逻辑，然后markdown中传参调用即可。

这里的html文件需要放在 `layouts/shortcodes` 目录下，文件名随意，比如 `video.html`，但必须是 `shortcodes` 目录，不能自定义名称。内容如下：

```html
<video width=100% controls style="border-radius: 10px; overflow: hidden;">
    <source src="/videos/{{.Get `src`}}" type="video/{{.Get `type`}}">
</video>
```

接着在markdown文件中用如下代码调用shortcode

```
{{< video src="lec3/3. The goals of statistics.mp4" type="mp4">}}
```

这样，视频就能在Hugo页面中播放了。