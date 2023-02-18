---
id: metrics
title: Metrics
---

Historically, many performance metrics are defined to help web developers to measure the web performance. Right now, there are some certain metrics are popular for developers.

## Largest Contentful Paint 最大内容绘制 (LCP)

The Largest Contentful Paint (LCP) measures when a user perceives that the largest content of a page is visible when the page first started loading. The metric value for LCP represents the time duration between the user initiating the page load and the page rendering its primary content.

To provide a good user experience, sites should strive to have Largest Contentful Paint of 2.5 seconds or less. To ensure you're hitting this target for most of your users, a good threshold to measure is the 75th percentile of page loads, segmented across mobile and desktop devices.

![ LCP score](./img/LCP_score.svg  "What is good LCP score")

## Cumulative Layout Shift 累积布局偏移 (CLS)

The Cumulative Layout Shift (CLS) is an important, user-centric metric for measuring visual stability because it helps quantify how often users experience unexpected layout shifts—a low CLS helps ensure that the page is delightful

To provide a good user experience, sites should strive to have a CLS score of 0.1 or less. To ensure you're hitting this target for most of your users, a good threshold to measure is the 75th percentile of page loads, segmented across mobile and desktop devices.

![ CLS score](./img/CLS_score.svg  "What is good CLS score")

## First Input Delay 首次输入延迟 (FID)

The First Input Delay (FID) metric helps measure your user's first impression of your site's interactivity and responsiveness. 

FID measures the time from when a user first interacts with a page (i.e. when they click a link, tap on a button, or use a custom, JavaScript-powered control) to the time when the browser is actually able to begin processing event handlers in response to that interaction. It quantifies the experience users feel when trying to interact with unresponsive pages—a low FID helps ensure that the page is usable.
 
To provide a good user experience, sites should strive to have a First Input Delay of 100 milliseconds or less. To ensure you're hitting this target for most of your users, a good threshold to measure is the 75th percentile of page loads, segmented across mobile and desktop devices.

![ FID score](./img/FID_score.svg  "What is good FID score")

## UI Blocking

### Frozen Time
The UI Freeze event indicates time intervals where the application was unable to respond to user input. More specifically, these are time intervals where window messages were not pumped for more than 200 ms or processing of a particular message took more than 200 ms.

### Slow Render
UI Rendering is the act of generating a frame from your app and displaying it on the screen. To ensure that a user's interaction with your app is smooth, your app should render frames in under 16ms to achieve 60 frames per second [reason](https://www.youtube.com/watch?v=CaMTIgxCSqU)

## Crashes
A app crashes whenever there’s an unexpected exit caused by an unhandled exception or signal.
There are many situations that can cause a crash in your app. Some reasons are obvious, like checking for a null value or empty string, but others are more subtle, like passing invalid arguments to an API or even complex multithreaded interactions.

## First Contentful Paint 首次内容绘制 (FCP)

The First Contentful Paint (FCP) is an important, user-centric metric for measuring perceived load speed because it marks the first point in the page load timeline where the user can see anything on the screen—a fast FCP helps reassure the user that something is happening.

The First Contentful Paint (FCP) metric measures the time from when the page starts loading to when any part of the page's content is rendered on the screen. For this metric, "content" refers to text, images (including background images), `<svg>` elements, or non-white `<canvas>` elements.

This is an important distinction to make between First Contentful Paint (FCP) and Largest Contentful Paint (LCP) —which aims to measure when the page's main contents have finished loading.
- For FID, the content has rendered, not all of it has rendered
- For LCP, the page's main contents have finished loading

To provide a good user experience, sites should strive to have a First Contentful Paint of 1.8 seconds or less. To ensure you're hitting this target for most of your users, a good threshold to measure is the 75th percentile of page loads, segmented across mobile and desktop devices.

![ FCP score](./img/FCP_score.svg  "What is good FCP score")
