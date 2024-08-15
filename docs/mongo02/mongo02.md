---
layout: default
title: 2. Memory Management
nav_order: 3
has_children: true
permalink: /docs/mongo02
---
<div style="text-align: center;">
2. MEMORY MANAGMENT
</div>
   {: .fs-10 }

In the rapidly evolving world of data management, choosing the right storage engine can significantly impact the performance and scalability of a database system. This post explores how MongoDB has addressed these challenges by transitioning from the MMAPv1 storage engine to WiredTiger, offering a modern solution that delivers enhanced scalability, concurrency, and data management capabilities.

## Background

CPU speeds have remained relatively constant for the past decade, but servers now have the capability to support more CPUs. Modern servers are equipped with many CPUs or cores, and each core has multiple memory caches that need to maintain cache coherence by "snooping" on writes to ensure consistency.

Traditional data engines face challenges with this architecture because writing to shared memory is slow, and snoopy cache coherence does not scale well. Databases, which manage shared access to data, are particularly affected by these limitations.

MongoDB addressed these challenges by transitioning from the MMAPv1 storage engine to WiredTiger. This decision was driven by the need for lower storage costs, better hardware utilization, and more predictable performance—critical requirements for handling the demands of modern, multi-core servers.

## WiredTiger vs. MMAPv1

WiredTiger offers several advantages that make it well-suited for these environments:

| **Feature**      | **WiredTiger**                                                                                                      | **MMAPv1**                                                                                                                 |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------- |
| **Scalability**  | Designed to perform better on `multicore systems`, making it more scalable in modern server environments.               | Not optimized for scaling with `multiple cores`; adding CPU cores does not significantly improve performance.                  |
| **Concurrency**  | Uses `MVCC (Multi-Version Concurrency Control)` for `document-level concurrency`, allowing simultaneous transactions. | Uses `collection-level locking`, limiting concurrency and potentially leading to performance bottlenecks under high workloads. |
| **Data Storage** | Utilizes a `B-tree layout` for data storage, offering efficient data organization and retrieval.                        | Uses `memory-mapped files`, which are less efficient in terms of data organization and retrieval.                              |
| **Compression**  | Supports `gzip and snappy` (default) compression for indexes and collections, leading to `smaller collection sizes`.  | Does not support compression, resulting in `larger collection sizes`.                                                          |
|                        | Also supports `index-prefix compression`, reducing index size both on disk and in memory.                               |                                                                                                                                  |

By adopting WiredTiger, MongoDB provides a more robust solution for the challenges of modern server architectures. WiredTiger's design supports the scalability and performance demands of high-scale applications, making it the preferred choice over the previous MMAPv1 engine. The use of locking mechanisms further helps manage data access and consistency in multi-core environments, ensuring MongoDB can efficiently handle concurrent operations.
