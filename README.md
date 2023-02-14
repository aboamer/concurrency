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

### Immutability
#### Isolated immutability
- vars are mutable, but never seen by more than one thread
- ex: receive an immutable message OR change a local var that is well encapsulated

#### Pure immutability
- receive a value .. create a new one from the old

#### Immutable DS
- cannot be modified after creation, exception will be thrown when trying to call a modifying method

#### Persistent DS
- Preserve the previous version of itself when being modified (effectively immutable).
- Allow updates and queries on any version.

### Executor service
- represents thread pool.
- Could be single threaded - cached - priority based - scheduled - fixed size
#### methods
- invoke all --> schedule a list of tasks
- invoke any --> schedule for one of the tasks to complete (ex: search for a value in a list)
- execute/submit --> schedule a task
- shut down --> complete current tasks and kill the pool
- shut down now --> depends on the tasks to respond well to interruption

### Exchanging data
| data sharing type | class|
| :---: | :---: |
| single shared data | atomic classes|
| 2 threads | exchanger class (synchronizatoin point, the faster is blocked until the slower catches up)|
| bunch of data betw threads | Blocking queue (sync - priority - linked and array)|
|Fork join pool | dynamically manages threads based on available processors and task demand (work stealing)|

#### Fork join pool
- To schedule tasks, provide instances of forkJoinTask to methods of forkJoinPool.
- Very useful for probs that can be broken down recursively until small enough to run sequentially.
- Tasks can be recursiveTask (return results), and recursiveAction (don't return results)

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
- Try to design for quick threads.
- Countdownlatch --> coordination for taks have no results to return.
- 
