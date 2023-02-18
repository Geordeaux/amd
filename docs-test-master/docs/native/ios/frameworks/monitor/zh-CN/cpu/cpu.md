---
id: cpu
title: CPU
hide_title: true
---

iOS 是基于 Apple Darwin 内核，由 kernel、XNU 和 Runtime 组成，而 XNU 是 Darwin 的内核，它是“X is not UNIX”的缩写，是一个混合内核，由 Mach 微内核和 BSD 组成。Mach 内核是轻量级的平台，只能完成操作系统最基本的职责，比如：进程和线程、虚拟内存管理、任务调度、进程通信和消息传递机制。其他的工作，例如文件操作和设备访问，都由 BSD 层实现。

CPU 占用率的采集原理其实很简单：App 作为进程运行时会有多个线程，每个线程对 CPU 的使用率不同。各个线程对 CPU 使用率的总和，就是当前 App 对 CPU 的占用率。

## 线程

线程定义了 Mach 中最小的执行单元。线程表示的是底层的机器寄存器状态以及各种调度统计数据，其从设计上提供了调度所需要的大量信息。

## 任务

任务是一种容器对象，虚拟内存空间和其他资源都是通过这个容器对象管理的。这些资源包括设备和其他句柄。资源进一步被抽象为端口。因此，资源的共享实际上相当于允许对对应端口进行访问。
严格来说，Mach 的任务并不是hi操作系统中所谓的进程，因为 Mach 作为一个微内核的操作系统，并没有提供“进程”的逻辑，而只提供了最基本的实现。

在 BSD 模型中，这两个概念有一对一的简单映射，每个 BSD 进程（即 OS X 进程）都在底层关联了一个 Mach 任务对象。实现这种映射的方法是指定一个透明的指针 bsd_info，Mach 对 bsd_info 完全无知。Mach 将内核也用任务表示（全局范围称为 kernel_task），尽管该任务没有对应的 PID，但可以想象 PID 为 0。

下图所示为权威著作《OS X Internal: A System Approach》中提供的 Mach OS X 中进程子系统组成的概念图。与 Mac OS X 类似，iOS 的线程技术也是基于 Mach 线程技术实现的。

![](https://static.devfdg.net/static/mono-static/docs-ui/img/mach-task-thread-system.png)

## 上报字段

| 属性 | 类型 | 固定值 | 说明 |
| --- | --- | --- | --- |
| t  | MonitorType | CPU | 类型 |
| d  | TimeStamp |  |  开始时间 |
| si | int |  |  主线程堆栈信息 |
| p | string |  |  上报页面名称 |
| ci | float |  |  CPU 使用量 |

