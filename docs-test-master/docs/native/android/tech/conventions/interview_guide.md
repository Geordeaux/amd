---
id: interview_guide
title: Android Interview Guide
hide_title: true
---

# Android Interview Guide
### Contracts ：ruotong.zhou@binance.com
中文版：https://confluence.toolsfdg.net/display/Technology/Android+Interview+Guide

### 1st Interview
**Android basics**
- Activity Launchmode
- How is the activity created
- Activity life cycle
- View customization
- Implementation of wrapContent in onMeasure
- onDraw avoids new objects. Object reuse
- How does the code written in onDraw take effect for a custom ViewGroup?
- Canvas#save/restore, various transformations
- Paint
- Animation
- Context
- Handler

**communication**
- How to communicate data between two activities?
- How to communicate data between two Fragments? (Look for bridge, activity/viewModel/other tripartite classes)
- How to communicate between two objects?
- How to design a Bus library?
- How to design a library that communicates across modules?
- How to communicate across threads? The main thread sends messages to the child threads?
- How to communicate across processes? 
- RecyclerView caching principle and optimization
- View touch event distribution principle
- How to optimize if else nesting
- How to collect crashes on Android
- How to adapt to mobile phones with different screen resolutions
- Android inter-process communication, Binder mechanism

**Data structure and basic algorithm HashMap**

- LinkedHashMap
- LRU implementation //https://www.jianshu.com/p/80acbd89798b

**Design Patterns**

- Six principles of design patterns
- Template design mode
- Dynamic proxy mode
- Strategy mode
- Chain of Responsibility Model
- Factory mode
- Decorative pattern

**The internet**
- Http format
- Https
- How does SSL/TLS implement handshake
- Network cache
- Socket
**Java basics**
- Memory model
- Object save location
- Timing and impact of object release
- Constant pool
- Reference type
- Jvm
- Dalvik vs ART
- Locking principle (Synchronized VS Volatile)
- Deadlock & how to avoid deadlock
- Java thread pool internal implementation
- JVM class loading mechanism
- GC principle
- Kotlin basics
- Empty safety
- Extension function
- DSL
- Generic

**Version control**

- Difference between merge/rebase?
- What is the difference between reset --hard - and reset --soft?
- Can it be restored after reset --hard? How to recover? reflog
- Flutter
- Component rendering
- How Flutter communicates with Native
 
### 2st interview
**Performance optimization experience**
- ANR principle and how to deal with it
- Leakcanery principle (Leakcanery) and how to deal with it
- How to detect FPS
- Startup and LCP
- CPU stuck detection and resolution
- Layout optimization
- Network optimization (network request, DNS resolution, SSL/TLS handshake optimization)
- App startup optimization
- Package size optimization
- High-performance coding

**The basic principles of common frameworks in the industry**
- Okhttp
- Glide
- Retrofit
- EventBus
- MVP MVC MVVM
- RxJava

**Problem solving and design ability**

- Problem-solving ability
  - Can ask how to deal with related questions about the items described in the resume
  - How to refresh the user token expired
 
- Framework design ability (inspect the depth and breadth of the candidate's thinking on framework design topics)
  - How to design and develop a memory leak monitoring SDK
  - Design and develop an SDK to collect Crash logs
 
 
**Other open questions**

- Talk about the views on the clean code
- How to deal with conflicts at work
- Talk about the most challenging things in work

**Selection criteria (rough strength):**
- Solid technology, technical pursuit and passion for work
  - Basic technology => Android, Java, data structure, algorithm, network foundation, design pattern, componentization and plug-inization
  - Technical Depth👆
  - Technical breadth => Technical vision
  - Other => Technical imagination, technical habits
- problem solving skill
  - Solve the problem Steps: Encountered a problem => Analyze the problem => Solution => Summary
  - Logical thinking ability demonstrated by problem solving

- Good communication and teamwork
  - Modest and prudent, friendly
  - Polite + respect
  - Motivation to do things and the ability to push things to the ground

- Cultural fit

 
 
 

**Algorithm question reference：**
https://git.toolsfdg.net/fe/interview/tree/master/Android/Algorithm 

**Binance Interview Guide:**
https://docs.google.com/document/d/15ZqbpPagBfWovswrJwPKlydVZheDodVxDke9vOC-l3E/edit

**Lever feedback:**
https://docs.google.com/document/d/1PA64GqI509q672YGMiKQddlBIQ2XiqPD/edit

**Reference materials:**
https://github.com/xingshaocheng/architect-awesome
