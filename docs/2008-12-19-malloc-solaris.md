---
layout: post
author: opcoder0
---

## Multi-threading and malloc

While I was involved in developing a IM server load balancer on Solaris 10, multiple threads in the load balancer required to allocate and de-allocate blocks / chunks of memory at a high rate. This led to decrease in performance of the load balancer and also led to crashing of the application. The core file generated during the crash always pointed to malloc(). It took me a while to figure out the root cause of the issue.

I figured later that the cause of the problem was because I had linked “libmalloc.so” to the application. This version of malloc implementation is meant for simple single threaded applications. Its because this implementation uses only one mutex to lock while allocating memory. And this leads to a serious bottleneck while multiple threads are allocating memory almost simultaneously. The malloc’s man page on Solaris also talks about this. The solution to this problem was to re-link the application to “libmtmalloc.so”. This is implementation of malloc uses multiple mutexes for locking and meant for multi-threaded applications. This change resolved all the crashing and performance issue.
