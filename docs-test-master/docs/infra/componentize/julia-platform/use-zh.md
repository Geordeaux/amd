---
id: use-zh
title: 使用
---

[English Version](use)

## 产品说明

### AddProject

要创建新项目，需要projectId。通常，需要产品来配置projectId。如图所示，您需要单击➕添加新项目页面的图标

![AddPreject](https://static.devfdg.net/image/julia/julia-doc/project-add.png)

### SwicthProject

如图所示，您可以单击按钮切换相关项目页面

![SwitchPreject](https://static.devfdg.net/image/julia/julia-doc/project-switch.png)

### EditView Or Preview

编辑页面后，如果要查看预览效果，可以在此处切换

![Switch Edit Or Preview](https://static.devfdg.net/image/julia/julia-doc/project-pre.png)

### viewConfig

##### 配置导入

您可以在这里直接上传一个页面json文件，然后julia会根据这个json文件显示相应的页面

##### 配置导出

下载当前页面的json文件。通常，在部署修改后的页面时，需要下载页面的json文件，然后将其同步到生产服务器

![ViewConfig](https://static.devfdg.net/image/julia/julia-doc/project-view.png)

### GlobalEdit

 在此处设置一些与页面相关的配置，例如“withDefaultHeader”、“withDefaultFooter”。默认情况下，这些配置处于启用状态。该页面带有页眉和页脚小部件。如果您的页面不需要它，您可以关闭它

### ActionEdit

设置页面组件action的配置

![ActionEdit](https://static.devfdg.net/image/julia/julia-doc/action-edit.png)


###  Publish  UnPublish

创建页面后，用户可以单击“发布”按钮发布页面。也可以单击“取消发布”按钮下线页面

该操作修改的是真实数据

### Save Update

如果页面是新页面，在更改页面后，需要单击“保存”按钮保存页面信息，否则将不保存。

如果页面是旧页面，在更改页面后，需要单击“更新”按钮更新页面信息，否则将不会更新。

该操作影响的是草稿数据


### Widget相关功能
 * 移动
 * 编辑
 * 移除

如果您想调整小部件的上下两级，可以单击这两个图标

单击每个小部件的编辑图标，您可以设置和更改小部件的属性

单击每个小部件的删除图标，可以删除小部件

![Widget related feature](https://static.devfdg.net/image/julia/julia-doc/project-action.png)


## 如何创建页面
1. 要配置projectId的产品
2. R&D基于projectId创建一个页面，并配置该页面
3. 已验证开发环境
4. 请测试人员测试页面

## 编辑和更新页面 
> 有关详细信息，请[参见](#save-update)

## 如何发布页面
1. 首先，开发环境测试通过了
2. 通过viewConfig的导入和导出功能将配置同步到生产
    > 有关详细信息，请[参见](#配置预览)
3. 生产灰度验证正常
4. 生产验证正常

## 业务如何使用julia系统
1. 在您自己的项目中创建一个新页面以承载该页面
2. 获取页面中的Julia页面配置，然后使用 `SSRRender` 呈现Julia页面

julia页面的业务访问非常简单。您只需要引用 `SSRRender`，然后在页面的静态方法 `getInitialProps` 中获取页面的配置数据，并稍微处理数据

#### Example [样本](https://git.toolsfdg.net/mono/mono/blob/master/web/apps/template-ui/src/pages/index.js) 
``` typescript jsx
import { SSRRender } from '@binance/simple-widget'
import getInitialProps from '../initialProps/home'
const { analysis } = SSRRender

const HomePage = (props)=><SSRRender {...props}/>

HomePage.getInitialProps = async ctx => {
  const config = await getInitialProps(ctx)
  return analysis({
    ctx,
    config,
    projectId: 'homepage',
    pagename: 'binance_home',
  })
}
export default HomePage
```

## 开发者如何对接julia系统

