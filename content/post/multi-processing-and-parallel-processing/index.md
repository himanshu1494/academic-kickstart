---
title: Multi Processing and Parallel Processing
subtitle: What is the difference between ?
date: 2020-09-21T17:44:55.932Z
summary: >-
  <!--StartFragment-->


  Multi-Processing can take place in both parallel and concurrent environments. Parallelism really means the ability to run two or more tasks “simultaneously” at the same time. However, concurrency is different.


  Concurrency implies the ability to run two or more tasks in a time-shared manner by switching between one task to another task. Consider a uni-processor machine. It is still capable of running multiple processes concurrently but \_not\_ parallely. Once the time slice/quanta of a process expires, CPU is given to another “ready to run” process.


  However, on a multi-processor/multi-core machine, two or more processes or threads can run at the same time — parallelism and this subsumes concurrency. Therefore, parallelism implies concurrency but vice-versa is not true. I think this should answer the question. Parallel processing is by definition related to parallelism whereas multi-processing can be talked about in the context of both parallelism and concurrency.


  <!--EndFragment-->
draft: false
featured: false
authors:
  - ""
tags:
  - Multiprocessing
categories:
  - blog
image:
  filename: what-is-the-difference-between-a-multicore-system-and-a-multiprocessor-system-banner.png
  focal_point: Smart
  preview_only: false
---
<!--StartFragment-->

One way I see it as

**Multiprocessing**: Way to run more then one process at a given time. Think of it as being able to run 10 different programs on windows at the same time. Or having 10 apps open on your phone at a given time. Running more than one process or program on SINGLE processor.

<!--StartFragment-->

1. Each process allocates separate memory area.
2. Process is heavyweight.
3. Cost of communication is high.

<!--EndFragment-->

**Parallel Processing**: Being able to run MULTIPLE processor at a given time. Think of it as running 10 different processor to compute answer to a single complex problem quickly. (Like a problem that can take 10 hours to solve on single processor, can be solved in 1 hour using 10 processor working Parallel. )

<!--StartFragment-->

1. Thread shares same address space.
2. Thread are lightweight.
3. Cost of communication between thread is low.

<!--EndFragment-->

<!--EndFragment-->