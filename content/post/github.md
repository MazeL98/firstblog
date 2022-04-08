---
title: "如何在github上快速找到需要的开源项目"
date: 2022-03-16T14:56:37+08:00
draft: false
---

| 指令                   | 含义                                                         |
| :--------------------- | ------------------------------------------------------------ |
| in:name mazel98        | repository 标题中带有"mazel98"                               |
| in:readme mazel98      | repository 的 readme.md 文档中带有 "mazel98"                 |
| in:description mazel98 | repository 的描述中带有"mazel98"                             |
| stars:>1000            | star数量大于1000，搜索高质量的项目                           |
| forks:>1000            | fork数大于1000，也是筛选知名度和质量的条件之一               |
| pushed:>2019-12-31     | 2019年12月31日之后仍然有更新的项目，筛选掉那些不再维护的老项目 |
| language:java          | 用Java语言编写的项目                                         |

GitHub 其实也提供了高级搜索的可视页面：

https://github.com/search/advanced

但总归记住几个常用指令更加方便。
