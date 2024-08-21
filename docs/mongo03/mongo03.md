---
layout: default

title: 3. Block Manager

nav_order: 4

has_children: true

permalink: /docs/mongo03
---

### 3. BLOCK MANAGER
{: .fs-10 .text-center }


<div style="text-align: center;">
  <img src="/assets/images/blockmanager.png" alt="blockmanager" width="400"/>
</div>

WiredTiger's storage architecture is optimized for multi-core CPUs and large memory. It consists of two key components: **in-memory cache** and **disk block manager**. Here's a brief overview:

- **In-Memory Cache**: Implemented using a B-tree structure with hazard pointers. B-tree nodes are organized at the page level, and the cache eviction policy follows the _LRU_ (Least Recently Used) algorithm.
- **Disk Block Manager**: Handles raw block I/O operations, including:
  - Reading and Writing Pages
  - Checksum/Verification
  - Space Management
  - Compression
  - Checkpoints


### In-Memory Page Format
<div style="text-align: center;">
  <img src="/assets/images/btree.png" alt="btree" width="400"/>
</div>

WiredTiger represents database tables using a **B-Tree** data structure (`WT_BTREE` in `btree.h`). The B-tree is composed of nodes, which are page structures:

- **Root and Internal Pages**: Store keys and references to other pages.
- **Leaf Pages**: Store keys and values. As users insert data, records are kept in sorted order. When a page reaches its limit, it splits, causing the B-tree to expand.

### On-Disk Page Format
<div style="text-align: center;">
  <img src="/assets/images/extent.png" alt="extent"/>
</div>

**Extents** represent ranges of contiguous blocks on disk.

<div style="text-align: center;">
  <img src="/assets/images/memory-disk.png" alt="memory-disk"/>
</div>

This structure lays the foundation for how WiredTiger manages data both in memory and on disk. In the subsequent sections, we will delve deeper into how the **block manager** operates within this structure, including how it handles I/O operations, manages space, and ensures data integrity through checkpoints and other mechanisms.
