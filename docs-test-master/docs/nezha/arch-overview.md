---
id: arch-overview
title: Overview
sidebar_label: Overview
---

## Overview

## Native Framework Architecture and System Design

![Architecture](https://static.devfdg.net/static/mono-static/docs-ui/img/android_arch.jpg)

#### WebView 
* Worker: execute primary logic, include app.js and page.js. Android use a webView as worker, iOS use a JSCore as worker
* View: render the user interface, send the alternative events to worker and reload the UI, both android and ios use the WebView as View
* Only single worker and multiple View  in run-time, create a new webView to load a  page every time and push it to navigation stack 

#### Bridge
* Bridge connects the web view context and native, receives the messages from JavaScript and sends messages to the JavaScript from native. 
* Message from JavaScript contains three parameters: functionName, args and callbackId
* Messages sent to JavaScript also contain three parameters: functionName, args and callbackId, this callbackId is the same as the callbackId in 2.

#### Plugin
* PluginManager processes and dispatches messages to suitable plugin by the  functionName, and plugin implements the specific logic to process messages
* PluginManager manages plugins, it implements register & unregister methods, We should register all plugin in the framework initialization
* Plugins: Login, Network(http request), payment, share and so on

#### Core
* NavigateManager: manage the Mini-App navigation stack, 
maximum stack size: 5
* WebViewPool: reuse the webview, because initializing the new web view costs extra resources in both Android and iOS 
* CacheManager: [Dynamic update strategy](https://docs.google.com/document/d/120IjhYXdK4TcBB_uXGyRbMpMAe8h358Am37xcAzYYZY/)

## Nezha Framework Startup Flow
![Architecture](https://static.devfdg.net/static/mono-static/docs-ui/img/android_arch_2.png)

![Architecture](https://static.devfdg.net/static/mono-static/docs-ui/img/android_arch_1.jpg)

## Navigation

#### Description

The navigation in the application is represented by the pages stack.

The first page in the navigation stack is defined by .

#### Navigation Actions List

- [navigation-push](rpc-action#navigation-push) - navigate to the new page by adding it on top of the existing pages stack, `url` is required parameter

- [navigation-pop](rpc-action#navigation-pop) - navigate to the previously opened page, extra parameter `delta` - the amount of steps back, 1 is default

- [navigation-replace](rpc-action#navigation-replace) - navigate to the new page by replacing the top page in the existing pages stack, `url` is required parameter

#### Diagram

![Architecture](https://static.devfdg.net/static/mono-static/docs-ui/img/native_navigation.jpg)

## Mini program appearance

Mini program can change it's appearance using different RPC action:

- [set-navigation-bar-title](rpc-action#set-navigation-bar-title)
- hide-navigation-back-button
- set-navigation-bar-color
