---
layout: post
title: "Go Scheduler"
description: ""
category: 
tags: []
---
{% include JB/setup %}


#OS Scheduler

先复习一下操作系统的线程调度：

unix 有两个调度器：high level 和 low level

##low level scheduler

每个 CPU/Core 都有一个自己的 run queue， 里面记录着当前运行的人物，它们按照优先级排序。当一个任务阻塞时候，low level scheduler 调度器会从 run queue 里选一个优先级最高的任务来执行。

##high level scheduler

线程的优先级是动态变化的，根据具体情况它们会被调度到不同的 cpu/core 的 run queue 里，这些任务是由 hight level scheduler 来完成的。


##Run Queue

120 ~ 223 普通线程

48 ~ 79 实时线程

224 ~ 250 Idle

Unix 有 64 个 run queue 

把线程的优先级除以 4，然后放进对应的 run queue 里面

除了当前正在执行的线程，所有创建的线程都在 run queue 里面


##时间分配

线程调度器的职责之一就是使 cpu 效率最大化，通过调度线程去抢占 cpu.

这里面有种情况，当一个线程由于缺少某个资源被调度出去，然后过了一段时间资源准备好了，但原来的 cpu 已经被占用了，于是就被迁移到另一个 cpu 上，这个操作是非常耗时的，这种多任务效率甚至不及单任务。因为迁移线程扔掉了有用的 cache.

传统的调度器通过维护一个全局的 list 来定期更改线程的优先级 


##Go Scheduler


