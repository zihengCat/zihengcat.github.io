---
title: Hexo博客添加网易云音乐外链
date: 2017-06-13
tags: Hexo
---

# 网易云音乐外链测试

测试博客正文添加网易云音乐外链。

<!-- Spirits - KOKIA -->
<!-- Begin -->
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width="330" height="86" src="//music.163.com/outchain/player?type=2&id=32069326&auto=0&height=66"></iframe>
<!-- End -->

成功!

# 实现

> 网易云音乐提供单曲、专辑、歌单、电台节目的外链播放器，将外链播放器放在论坛、网站上，都可以免费播放。
>
> 如何使用外链播放器？
> 1. 在网页版（music.163.com）进入单曲、歌单、专辑、电台节目页面后，点击 “生成外链播放器” 链接。
> 2. 歌单和专辑外链播放器可以选择大中小三种尺寸，单曲和电台节目可以选择中小两种尺寸。你可以选择最适合你网站设计的尺寸。
> 3. 还可以选择是否要自动播放，打上勾后，别人访问网站时播放器会自动开始播放。
> 4. 最后将播放器的代码黏贴到你的网站上，大功告成！

其实就在你的网页上插入一个`iframe`...

Markdown 可以直接内联HTML代码，只要博客系统支持`iframe`调用即可。
比如本文的测试用例：
```
...

<!-- Spirits - KOKIA -->
<!-- Begin -->
<iframe frameborder="no" border="0" marginwidth="0" marginheight="0" width="330" height="86" src="//music.163.com/outchain/player?type=2&id=32069326&auto=0&height=66"></iframe>
<!-- End -->

...
```

相关参数:

| 参数 | 含义 |
| ---- | ---- |
| width |  宽度 |
| height |  高度 |
| type | 歌曲(1), 歌单(2), 电台(3) |
| id | 歌曲ID号 |
| auto | 自动播放(1), 0手动播放(0) |

