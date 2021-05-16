---
title: Hello Docker
categories:
- General
feature_image: "https://picsum.photos/2560/600?image=872"
---

建站的历程。

最开始建站是因为数据结构需要一个大作业，脑子一抽报了搜索引擎的题目（这确实是乱来的）。用的最简单的`Django+uWSGI+Nginx`，Nginx中域名部分找了好一些文章。

第一版过后，由于懒惰原因，直到2020年初重新去做第二版。`Django+Supervisor+Nginx`，并且加上了HTTPS。

毕业后，又重新用`Docker Compose`重写了一遍，找到对应教程就挺快的了，不用自己从零写起。最近把额外的一个定时任务模块，塞进了容器中。

从实习开始恶补了一阵Linux，然后后面用VS Code结合WSL，再加上Docker Desktop （用WSL2作为内核），实现了丝滑的Linux体验。