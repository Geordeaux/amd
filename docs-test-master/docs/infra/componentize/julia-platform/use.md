---
id: use
title: use
---

# Use of Julia

[中文版](use-zh)

## Product description

### AddPreject

To create a new project, a projectId is required. Generally, the product is required to configure the projectId. As shown in the figure, you need to click the ➕icon to add a new project page

![AddPreject](https://static.devfdg.net/image/julia/julia-doc/project-add.png)

### SwitchPreject

As shown in the figure, you can click the button to switch the related project page

![SwitchPreject](https://static.devfdg.net/image/julia/julia-doc/project-switch.png)

###  Switch Edit Or Preview

After editing the page, if you want to see the preview effect, you can switch here

![Switch Edit Or Preview](https://static.devfdg.net/image/julia/julia-doc/project-pre.png)


### ViewConfig

##### Read file

You can directly upload a page json file here, and then julia will display the corresponding page according to this json file

##### Export file

Download the json file of the current page. Generally, when deploying the modified page, you need to download the json file of the page, and then synchronize it to the production server

 ![ViewConfig](https://static.devfdg.net/image/julia/julia-doc/project-view.png)

### GlobalEdit

Set some page-related configuration here, such as `withDefaultHeader`,`withDefaultFooter`。 Them is enabled by default. The page comes with header and footer widgets. If your page does not need it, you can turn off

### ActionEdit

Set the configuration of page component action

![ActionEdit](https://static.devfdg.net/image/julia/julia-doc/action-edit.png)


###   Publish  UnPublish

After the page is created, the user can click the Publish button to publish the page. You can also click the UnPublish button to go offline and change the page

Publish or UnPublish, Data will become official information


### Save Update

If the page is a new page, after changing the page, you need to click the save button to save the page information, otherwise it will not be saved.
If the page is an old page, after changing the page, you need to click the update button to update the page information, otherwise it will not be updated.

Save or Update, The data stored is draft information

### Widget related feature
 * Move up and down
 * Edit properties
 * Remove Widget

If you want to adjust the upper and lower levels of the widget, you can click on these two icons

Click the edit icon of each widget, you can set and change the properties of the widget

Click the delete icon of each widget, you can remove the widget

![Widget related feature](https://static.devfdg.net/image/julia/julia-doc/project-action.png)

## How to create a page
1. Product to configure projectId
2. R&D creates a page based on projectId, and configures the page
3. The dev environment is verified
4. Ask the tester to test the page

## Edit and update page
> For details, please [see](#save-update)

## How to publish a page
1. First, the dev environment test passed
2. Synchronize configuration to production through the import and export function of viewConfig
 > For details, please [see](#viewconfig) 
3. Production grayscale verification OK
4. Production verification OK


## How do business lines use the julia system
1. Create a new page in your own project to host the Julia page
2. Get the Julia page configuration in the page, and then use SSRRender to render the Julia page

Business access to the julia page is very simple. You only need to reference the SSRRender widget, and then obtain the configuration data of the page in the static method getInitialProps of the page, and process the data slightly

#### Example [Sample project](https://git.toolsfdg.net/mono/mono/blob/master/web/apps/template-ui/src/pages/index.js) 
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

## How developers connect to Julia system

