---
layout: post
title: "Can't re-attach tmux [Fixed]"
date: 2013-08-13 19:53
comments: true
categories: [tmux]
---
使用[tmux](http://tmux.sourceforge.net)管理session和窗口非常方便，特别是当在一个傲娇的集群上工作的时候。最近两天发现一个问题，有时detach(`prefix+d`)一个session之后，无法再attach上了，试过几种方法都不行：

* `tmux attach`
* `tmux attach -t work` （work是之前的session名字）
* `tmux -L ~/tmp/tmux-1627/defualt attach`
在[superuser](http://superuser.com/questions/279169/tmux-died-and-says-no-sessions-is-there-any-way-to-recover)上找到了答案:
``` bash
kill -10 [tmux pid]
``` 
给tmux发一个信号，但不会杀掉它。虽然不知道原理，但至少能找回之前的session了！
