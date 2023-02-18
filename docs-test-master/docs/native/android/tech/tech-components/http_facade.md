---
id: http_facade
title: HTTP Facade
hide_title: true
---

# HTTP Facade

## What is HTTP Facade：

The main purpose is to simplify network requests and encapsulate our network lib. 

---

## How to use：

It has lots of http method like get and post and will be inherited at each business side.

For example : In `Margin area`, You could create a class like `MarginFacade` and add some method with parameters and callback for fetching data.

![img](https://static.devfdg.net/static/mono-static/docs-ui/img/android/http_facade_1.png)

### HttpCallback：

It include callbacks for success and failure and ensures that it will not be invoked when the view is destroyed.

![img](https://static.devfdg.net/static/mono-static/docs-ui/img/android/http_facade_2.png)

### HttpProgressCallback：

It extends HttpCallback. 

If you pass base classes like BaseActivity, BaseFragment and BaseDalogFragment, it would show progress dialog when it's loading and disappear before the page is destoryed. 

---

## Editor: xiaoqian.hu@binance.com & jason.j@binance.com

## [Ref](https://confluence.toolsfdg.net/pages/viewpage.action?spaceKey=Technology&title=HttpFacade)