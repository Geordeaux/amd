---
id: lcp_uiblocking_optimization
title: LCP/UIBlocking optimization experience

hide_title: true
---

# LCP

## Layout optimization

1. Use ConstraintLayout to flatten the layout and reduce nesting
2. When using ConstraintLayout, if you have higher performance requirements, you should pay attention to less use of the `<Group />` tag
3. Use ViewStub/merge tags
4. ViewPager upgrade ViewPager2
5. Optimize overdraw
6. Delay loading for views with lower display priority
7. Use AsyncLayoutInflater to preload the layout
8. Try not to use multiple fragments nested
9. Custom high-performance components (such as OrderBook) replace RecyclerView to reduce layout and measurement time

## Data optimization

1. Interface request in advance
2. Time-consuming operations should be placed in sub-threads or coroutines, such as format, formula calculation, etc.
3. Objects are initialized on demand, do not create in advance
4. Use deepCopy with caution
5. Algorithm optimization
6. Avoid unnecessary thread switching

# UIBlocking

1.Time-consuming operations in the main thread are placed in sub-threads or coroutines
2.Remove unnecessary animation
3.When using rxjava zip operator, pay attention to thread switching, subscribeOn is not effective
4.Avoid unnecessary refreshes, such as updating the Open Order list in ws miniticker
5.Do not refresh the page when the page is not visible, you can add lifecycle.currentState.isAtLeast(Lifecycle.State.RESUMED) to determine
6.autoSizeMaxTextSize/autoSizeMinTextSize is time-consuming, you can use AutoSizeTextView instead
7.RecyclerView optimization
- RecyclerView needs to set setHasFixedSize(true) to improve performance on high-brush pages
- ViewPager2 also implements multiple page functions through RecyclerView. You can also set setHasFixedSize(true) for this RecyclerView to improve performance.But ViewPage2 does not directly provide a method to expose RecyclerView, we can get RecyclerView by way of ViewPage2.getChildAt(0)
- Use RecycledViewPool and DiffUtil to optimize
- Remove RecyclerView animation
- Time-consuming operations such as binding data in the Adapter can be placed in asynchronous threads
- RecyclerView does not update UI when sliding
- Limit refresh rate
- ConstraintLayout is not recommended for the layout of items in RecyclerView


