---
id: state_manager
title: State Manager
hide_title: true
---

# State Manager

### Contracts ：luming@binance.com

# AppForegroundStateManager2

### Q&A

### What's the purpose of the class?

- To monitor your app coming to the foreground or going to the background.

### How to implement the class?

- Define the counter as β that will be added 1 while it reach on "**OnStart**" and be subtracted while it reach on "**OnStop**" in an arbitrary activiy.
- It's in background when β is zero. otherwise it's in foreground when β is greater than zero.

### Theory

- It absolutely reach "onStart" while it opens application unless exited application.
- If it opens another activity, new activity will also reach "onStart" then old activity will go through "onStop"
- ref : [https://developer.android.com/reference/kotlin/android/app/Activity#ActivityLifecycle](https://developer.android.com/reference/kotlin/android/app/Activity#ActivityLifecycle)

### What we put the monitor at application class?

- It cannot count on an arbitrary activiy and should listener the lifecycle in your app, so we put it at application class.

When will it send notifications.

- It send events as soon as user switch the status.