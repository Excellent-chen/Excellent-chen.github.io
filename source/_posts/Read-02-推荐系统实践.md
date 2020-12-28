---
title: Read-02-推荐系统实践
date: 2020-12-28 19:01:13
categories:
- Read
tags:
- 推荐系统
---

#### Chapter 001

> <!-- Section 001 -->
>
> ※ 推荐系统的基本任务是联系用户和物品，解决信息过载的问题。
>
> ※ 推荐系统是一种自动联系用户和物品的工具，它既能够在信息过载的环境中帮助用户发现令它们感兴趣的工具，也能够将信息推送给对它们感兴趣的用户。
>
> ※ <span style="color:blue">分类目录</span>，<span style="color:blue">搜索引擎</span>，<span style="color:blue">推荐系统</span>。从某种意义上说，搜索引擎和推荐系统是两个互补的工具，前者满足了用户有明确目的时的主动查找需求，后者能够在用户没有明确目的时帮助他们发现感兴趣的新内容。

> <!-- Section 002 -->
>
> ※ 总得来说，几乎所有的推荐系统都是由<span style="color:blue">前台展示页面</span>，<span style="color:blue">后台日志系统</span>和<span style="color:blue">推荐算法</span>构成的。

> <!-- Section 003 -->
>
> ※ 在推荐系统中，主要有三种评测推荐效果的实验方法，即<span style="color:blue">离线实验</span>，<span style="color:blue">用户调查</span>和<span style="color:blue">在线实验</span>。<span title="它们各自有哪些优缺点？">**TODO**</span>
>
> ※ 常见的评测指标包括：用户满意度；预测准确度（评分预测：`RMSE`，`MAE`；`TopN`推荐：`Recall`，`Precision`）；覆盖率（信息熵；基尼系数）；多样性；新颖性；惊喜度；信任度；实时性；健壮性；商业目标。**注**：如果一个系统会增大热门物品和非热门物品的流行度差距，让热门的物品更加热门，不热门的物品更加不热门，那么这个系统就有<span style="color:blue">马太效应</span>。评测推荐系统是否具有马太效应的简单办法就是使用基尼系数。<span title="如何利用基尼系数进行判断？">**TODO**</span>

