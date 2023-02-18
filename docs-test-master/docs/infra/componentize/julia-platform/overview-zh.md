---
id: overview-zh
title: Julia平台概述
---

[English Version](overview)

> `Julia`是一个页面可视化构建平台。它允许开发人员根据特定的规范开发widget，将widget提交到widget库，然后产品或业务人员可以在编辑页面上定制构建页面并轻松发布页面。

## 核心

### 组件库(Widget library module)

小部件是julia系统的基石。每个页面由不同的小部件组成。julia的小部件存储在julia小部件中。开发人员需要根据特定的需求开发小部件。小部件开发完成后，启动项目中的julia ui，小部件库中的所有小部件都将加载到小部件列表中。

### 编辑器(Edit module)

编辑模块用于拼接页面。业务人员可以选择现有的小部件，将它们添加到页面中，然后配置每个小部件的配置属性。编辑后，页面将生成json配置。如果是新页面，则需要将其保存到数据库中。如果是对数据库的现有页面更新。

### 渲染器(Render module)

呈现模块根据页面的json数据加载相应的小部件。在呈现页面时，它将根据每个小部件的定义动作加载API数据，将其更新到模型中，并注入每个小部件的配置数据。转到小部件道具配置

### 系统架构图(System architecture diagram)

![Widget Vim](https://static.devfdg.net/image/julia/julia-doc/julia-sys.png)
