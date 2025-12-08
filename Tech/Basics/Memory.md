##  Memory leak

A **memory leak** occurs when a program or application consumes memory but **fails to release it** back to the operating system or the memory management system (like a garbage collector) when the memory is no longer needed.

**example:**  The **Garbage Collector** can't clean up the object because it believes it might still be needed.
## Out of memory

An **Out of Memory (OOM)** error is the **result** of a critical lack of available memory, often triggered by a severe memory leak or simply an attempt to allocate an extremely large block of memory.

## Memory dump

A **memory dump** (or **heap dump**) is a snapshot of an application's memory taken at a specific point in time. It is the primary tool used by developers to diagnose memory leaks and OOM errors.