+++
title = '如何优雅地整理附属文档'
date = 2024-08-29T23:36:56+08:00
tags = ['Hugo']
draft = false
hidden = true
+++

在编辑一篇总纲式文章时，可能出现这样的问题：有些技术细节不想被呈现在主干里，而是希望有个链接点击后可以跳到新的文章，但我们又不希望这篇新文章也作为独立的文章被索引到Archive里。

由于Hugo默认渲染文件名为`index.md`，如果直接把附属文档和主文档`index.md`放在同一目录下，那么附属文档无法被渲染，访问时会显示404。

一个比较优雅地解决办法是新建目录 `content/chapters`，然后把附属文档放在 `content/chapters/sub_article.md` 这样的路径下，这样子就可以被渲染了。

具体操作方法是

```shell
cd blog
hugo new chapters/sub_article.md
```

保持头部配置 `draft = true` 即可，使得附属文档不会被渲染到主页，但仍然可以被访问到。

在总纲文章中插入链接，建议使用绝对路径

```
[标题](/blog/chapters/sub_article/)
```

比如：[GRS检验](/blog/chapters/grs)
