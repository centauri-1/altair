---
# order: 10
date: 2021-03-07
title: 'Collaborative Editor'
# cover: 
repo: 'https://github.com/siavava/java/tree/main/Problem%20Sets/PS-6'
tech:
  - Java
  - threads
  - mutexes
  - web sockets
featured: false
navigation: false
---

A collaborative editor on shared canvas.
Multiple users can connect, and each sees changes in real-time as they are made.
We use mutexes and locks to enable concurrent access whilst
avoiding :highlight[data races] and deadlock pitfalls
such as :highlight[mutual exclusion], :highlight[hold-and-wait], 
:highlight[circular wait], and :highlight[no preemption].
