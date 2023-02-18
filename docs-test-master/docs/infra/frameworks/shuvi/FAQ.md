---
id: FAQ
title: FAQ
---

#### 1. `mono start -p xxx-ui` failed. What should I do?

Run `yarn clean:mono && bazel clean --expunge && yarn`
  
#### 2. My electron development is unable to start:

Use hashtag as a alias could be a problem:

```diff
// webpack.js
- config.resolve.alias.set('#', path.resolve('./src/pages/electron'))
+ config.resolve.alias.set('_', path.resolve('./src/pages/electron'))
```

#### 3. How do I use `POST` and `redirect` in server middleware:

```js
// src/api/middleware.js
export default function middleware(ctx, next) {
   // 1. Limit to POST method
   if (ctx.method !== 'POST') return await next()
   
   // 2. Use koa ctx.redirect api
   ctx.redirect('/path')
}
```