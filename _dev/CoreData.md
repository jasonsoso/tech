---
layout: note
title: Core Data
---
## Concurrency with Core Data

### Use Thread Confinement to Support Concurrency

The pattern recommended for concurrent programming with Core Data is thread confinement: each thread must have its own entirely private managed object context.

There are two possible ways to adopt the pattern:

1. Create a separate managed object context for each thread and share a single persistent store coordinator.
This is the typically-recommended approach.

2. Create a separate managed object context and persistent store coordinator for each thread.
This approach provides for greater concurrency at the expense of greater complexity (particularly if you need to communicate changes between different contexts) and increased memory usage.
