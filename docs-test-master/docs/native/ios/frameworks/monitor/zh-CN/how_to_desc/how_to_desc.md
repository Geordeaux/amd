---
id: how_to_desc
title: 性能指标数据文档模版
hide_title: true
---


这是一篇关于指标描述文档的模版，用于所有描述性能指标的文档具有统一格式，从而方便后续的维护和梳理工作。

## 内容分块

对于一篇指标描述文档，我们需要做到以下内容分块：

- **项目背景**及**指标定义**：用来描述这个指标解决了什么问题，量化数据的定义，数据的权威性等信息；
- **打点上报字段展示** ：当前指标上报了哪些关键性的字段和数据，及其对应的类型和备注描述；
- **数据处理和分数量化公式**：如果该指标涉及到了服务端消费时候的复杂计算，需要提供完整的计算公式；
- JSON 数据格式：需要提供一份完整的请求信息；
- (Optional) **上报策略**：如果其策略会对性能开销有一些影响，也需要写清；
- (Optional) **端核心代码块讲解**：为了更好的维护，也为了让其他同学提优化 PR，可以对一些核心对代码原理进行一次说明；

## 内容生产的工具推荐

### 公式编辑

建议大家使用 LaTeX 来描述公式和算法文稿，有助于后续的修改和维护。（*Mono* 暂时不兼容 LaTeX 的渲染，需要使用软件完成渲染后截图展示）

![语雀支持 LaTeX 渲染](https://static.devfdg.net/static/mono-static/docs-ui/img/c1.png)

### 思维导图和工程结构图

推荐大家使用正版的 Xmind 进行编辑，所产出的图较为美观、清晰、明了。

![](https://static.devfdg.net/static/mono-static/docs-ui/img/c2.png)

