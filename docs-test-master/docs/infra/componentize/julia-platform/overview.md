---
id: overview
title: overview
---
# Julia Overview

[中文版](overview-zh)

> `Julia` is a page visualization construction platform. It allows developers to develop widgets according to certain specifications, submit the widgets to the widget library, and then the product or business personnel can customize the construction page on the editing page and easily publish the page.

## Core modules

### Widget library module

Widgets are the cornerstone of the julia system. Each page is made up of different widgets. The widgets of julia are stored in julia-widget. Developers need to develop widgets according to certain requirements. After the widget development is completed, start julia-ui In the project, all the widgets in the widget library will be loaded into the widget list.

### Edit module

The editing module is used to piece together pages. Business personnel can select existing widgets, add them to the page, and then configure the configuration properties of each widget. After editing, the page will generate a json configuration. If it is a new page, it needs to be saved to the database. If it is an existing page update to the database.

### Render module

The rendering module loads the corresponding widgets according to the json data of the page. When rendering the page, it will load the API data according to the defined actions of each widget, update it to the model, and inject the configuration data of each widget. Go to the widget props config

### System architecture diagram

![Widget Vim](https://static.devfdg.net/image/julia/julia-doc/julia-sys.png)
