---
layout: post
title:  "开始学习 Tensorflow"
date:   2016-07-21 12:33:05 +0800
categories: Tensorflow
---

tensorflow  张量流

- 用 graph 表示计算任务
- 用 Session 执行图
- 用 tensor 表示数据
- 用 Variable 维护状态
- 用 feed 赋值和用 fetch 从中获取数据

但如果使用 IPython 交互环境这类事时，用 InteractiveSession 来替代 Session类，并用 Tensor.eval() 和Operation.run() 替代 Session.run() 

*疑问* InteractiveSession需要使用 InteractiveSession 来释放资源吗？

