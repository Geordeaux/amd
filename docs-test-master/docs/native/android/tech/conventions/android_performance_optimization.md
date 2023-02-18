---
id: android_performance_optimization
title: Android Performance Optimization
hide_title: true
---

# Android Performance Optimization

## LCP

### Create page(Activity/fragment) → Fetch data → Render page

1. View optimization
    1. View hierarchy
    2. Avoid overdraw
    3. Use viewstub
    4. Use custom view that's been optimized.
    5. Don't update state if view is invisible or gone.
    6. **Pre-load or load layout asynchronously( you could apply AsyncInflater)**
    7. More appropriate, proper animation.
    8. RecyclerView optimization.
2. Data fetch
    1. Establish the connection first if it's real-time data; otherwise if it's not real-time data, you could pre-fetch data.
    2. Optimize network request processes
    3. Optimize the processes of persistent storage.
    4. All pre-fetch data instead of concurrently fetch data.
    5. Optimize business logic, asynchronous task, filter and reduct reflection operations.
3. Render page
    1. Render page accordingly or partial update like refresh mechanism(**diff**)  in recyclerview
    2. Simplify the operations before it renders and it's not necessary to delay executions

---

## UIblock

It loses frames when it comes to rendering

Check the operations in main thread, then you could asynchronously load and pre-load those data in order to reduce I/O operations and computive operations.

---

## Monitor tools

There' are some rules for the performance of monitor tools and the size of apk in production environment.

Therefore, Don't break down the performance or the size of apk just for attching those monitor tools .

Currently, It's plaing to optimize and clear up monitor tools in current project.

1. Lean our monitor tools.
2. Adjust the way We currently use for monitor. For example, Using firebase for monitoring ui blocking.

## Contacts

hongyang.hua@binance.com