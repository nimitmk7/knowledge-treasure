
## FAQs

> Is B-tree index stored in Main memory or Disk ?

In a database system, B-tree indexes are primarily stored on disk, not in main memory. Here's why:

1. Persistence:

- Disk storage is persistent, meaning the index survives system restarts
- Main memory is volatile and would lose the index if power is lost

2. Size Considerations:

- B-trees can be very large, often larger than available RAM
- Databases need to handle indexes that are many gigabytes or even terabytes in size

However, for performance optimization:

- Frequently accessed parts of the B-tree are cached in main memory
- Most databases use a buffer pool to keep hot (frequently accessed) pages in RAM
- The buffer manager decides which pages to keep in memory and which to write back to disk

This is why B-trees are specifically designed to:

- Minimize disk I/O operations
- Have a high branching factor to reduce tree height
- Keep the tree balanced to ensure predictable performance

So while portions of a B-tree index may be in memory at any given time, the primary storage location is always on disk. This is different from other structures like hash indexes which can be purely memory-based.

