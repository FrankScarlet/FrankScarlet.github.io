---
title: Learning on Graph Part 2
categories:
- Graph
feature_image: "https://picsum.photos/2560/600?image=872"
---

做一个图应用，真难！

之前一篇`Docker`文章里提到，以前只做过`GCN`落地。虽然有一个比较完整的工作流，但是不管是数据处理还是前端展示，都比较朴素。最后计算出的负面结果，直接以表格形式展现，并没有深入到图的交互上。当时有一个组的成果，前端比较牛，有比较强的拖拽功能，帮助分析。不过依稀记得那个主题不太需要特别强的自主图分析能力。

不管怎么样吧，要把图应用做起来，就必然会要涉及到，怎么存储图，以及怎么展现图两个核心问题。分别对应图数据库，与前端图展示的一些框架，我就抛砖引玉，简单介绍一下目前了解的情况。狂一句，我觉得在这个工作上，已经能稍微超越之前的同事了（大概）。

> 我觉得迭代的紧迫感还是有必要的，研究技术，有持续的报告才有动力继续干下去。

## 后端存储

作为所有的基础，图数据库和图计算引擎是所有的基础。是否能处理大数据量、是否开源、是否能分布式部署，是核心问题。一个隐藏的问题，是图查询语言的复杂度问题：对于开发人员，可能没太多区别，但查询语句的可迁移性是之后的关注点；对于业务，尤其是想要自己进行查询的人来说，可能没那么多时间学习两种语法。

如果选`JanusGraph`或`Neo4j`，就会涉及`Gremplin`和`Cpyher`的选择问题。当然我看大厂的一些云图数据库，是两种语言都支持的，不知道背后是什么实现。


## 前端展示

> 前端突出一个眼花缭乱，相对来说后端的技术不会变这么快

借鉴一种分类，把图可视化大致分为两种：一种是图分析，一种是图编辑。

`G6`在最近的版本，侧重于图分析。而`graphin`是对`G6`的`React`组件库。我觉得对于不同的场景，单个`webpack`打包的项目似乎不能满足更加复杂的要求了。

我对`Demo`的设想：
1. `React`开发，简易的多页面
2. 硬编码连接`Neo4j`，中间有一个接口实现数据的转换


## 有关资料

1. [neo4j](https://neo4j.com/)
2. [neovis.js](https://github.com/neo4j-contrib/neovis.js/)
3. [graphin](https://antv-graphin.gitee.io/graphin/quick-start/introduction/)
4. [g6](https://antv-g6.gitee.io/zh/examples/gallery/)
5. [graphin](https://graphin.antv.vision/)
6. [cytoscape.js](https://github.com/cytoscape/cytoscape.js)