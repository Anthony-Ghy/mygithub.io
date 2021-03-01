---
title: fork使用
categories: fork使用
tags:
- 前端
- git
---  
## fork使用

当我们在github上fork别人项目的时候，fork完在我们自己的仓库就可以看到相同的项目。

这个时候我们将自己仓库的这个项目clone下来，可以直接使用。但是如果项目开发周期较长，原项目作者中途还提交了新的代码，我们fork的这个远程仓库代码就不是最新的了。
<!-- more -->
这个时候如何将最新原仓库作者的代码更新到我们fork的自身仓库中呢？

步骤：

1.`git remote -v` 查看远程状态

2.git remote add upstream 远程源仓库qingtong/pinghu链接

例如：git remote add upstream https://xxx.com/Qingtong/pinghu.git

![533365454-5e3fe00eb9cef](https://first-1304891308.cos.ap-beijing.myqcloud.com/blog-picture/065632.png)

3.运行git pull upstream 拉取远程仓库的代码。

4.拉取完成以后本地代码已经是最新的了。

5.这个时候再把代码按照平常的提交流程进行提交就可以直接提交到自己的远端仓库了。

