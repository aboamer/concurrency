# concurrency
### 3 options
- Sync and suffer.
- S/W transactional memory model.
- Actor based.

### 3 ways to avoid problems
- Sync properly.
- Don't share state.
- Don't mutate state.

### 3 concurrent programs problems
- starvation
- deadlock
- race conditions (visibility issues)

### How many threads to create?
- For computation intensive (no blocking), threads = number of available cores
- For IO intensive, threads = number of availble cores/(1-blocking coefficient), where blocking coefficient increases with blocking time increase.
- We can compute this factor using profiling tools.

### How to divide the problems for being handled by threads?
- Always keeps the cores busy - no one is idle, by creating more parts than the number of threads.
- Imagine threads as workers, you need to have them working all time. Some are faster than others, so they need to be busy all time, not just finish and sit idle.

### How to make fair distribution of work load between threads in computationally intensive app?
- Understand the problem and its behavior across the range of inputs (ex: mix small and big numbers to get a uniform distribution)
- OR, divide the problem finer, to have more parts than threads (which are the number of cores in computa.. intensive app)

### Old and new APIs
| Old | problem | New |
| :---: | :---: | :---: |
| Thread class | instead of reusing threads, we create new | Executor service for pools|
| wait,notify,join | require sync | cyclic barrier and countdownlatch |
| synchronized | lacks granularity-no time out - no concurrent multiple readers - difficult to test | Lock interface |

#### random thoughts
- The number of concurrent threads for and app depends on the number of cores associated with its process.
- Multithreading will be useful when we have IO operations that block threads for a while. Otherwise, it will be unnecessary context switching.
- Main advantage for concurrency is to build responsive and fast apps.
- Volatile on a variable makes a thread bypass the cache and read/write the variable directly from memory. However, it could slow the performance.
- Memory barrier is copying from local memory (registers and caches) to main memory (RAM)
- Synchronized makes blocks of code atomic
- Having immutable fields referring ti immutable instances has a better performance than being volatile.
- Try to design around shared mutability: encapsulate mutable state well, and share only immutable data.
- OR, make everything immutable but use function composition.


