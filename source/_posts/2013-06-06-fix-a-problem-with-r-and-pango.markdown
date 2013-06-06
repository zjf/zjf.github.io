---
layout: post
title: "fix a problem with R and Pango"
date: 2013-06-06 13:11
comments: true
categories: [R, X11]
---

这几天换了新集群，遇到了一些问题。

ssh登录后，在R里测试一下`plot(1)`的输出，发现坐标数字和坐标轴的标签都变成了小方块，同时还有如下警告：

    (process:5586): Pango-WARNING **: No builtin or dynamically 
    loaded modules were found. Pango will not work correctly.
    This probably means there was an error in the creation of: 
    '/etc/pango/pango.modules'
    You should create this file by running pango-querymodules.

    (process:5586): Pango-WARNING **: pango_shape called with bad font, expect ugly output

    (process:5586): Pango-WARNING **: pango_font_get_glyph_extents called with bad font, expect ugly output

    (process:5586): Pango-WARNING **: _pango_cairo_font_install called with bad font, expect ugly output
Google的时候发现了cos上的一个[老贴](http://cos.name/cn/topic/17737)，大家最后也没讨论出个所以然来。最后在看X11的[帮助](http://www.inside-r.org/r-doc/grDevices/X11)时找到了原因：当X11的`type`被设置为`cairo`的时候，Pango会负责选择字体，这一步失败了。从警告信息上推测，失败的原因可能是普通用户没有权限写`/etc/pango/pango.modules`。

暂时的解决办法是，把X11的`type`设置为`Xlib`，用默认的字体就好了。把下面的代码加入`~/.Rprofile`（如果没有就新建一个），这样每次R启动的时候就会自动设置。
``` r
setHook(packageEvent("grDevices", "onLoad"),
        function(...) grDevices::X11.options(width=7, height=7, xpos=0,
                                             pointsize=10, type="Xlib"))
```
