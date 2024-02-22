# Multithreading (udemy)

- Refer this small spring boot documentation [here](https://docs.spring.io/spring-batch/docs/current/reference/html/scalability.html) for scaling and parallel processing.
- why do we need threads ? - 1. for performance 2. Responsiveness
- 1. Responsiveness - we don't ve to wait for the ongoing task to complete, we can simply handle multple req by using the multithreading function, ex - Responsiveness is particularly critical in app with UI
1. concurrency - is the multitasking we can achieve the concurrency with single thread ex node (js)
2. performance - impact. completing task much faster, finish more task in less time. and for high scale services we need fewer s/m (cost efficient)
- but the caveat is that multithreaded programming are different from the traditional single threaded programming.
- as we know all our apps will just reside in the hdd(just like the all other files). when the user runs the app the os takes the app from the disk and creates an instance of the prog at the ram(mem). that instance is called as the process or **context** of the app.
- one thread (called MainThread) contains **Stack** and **instruction pointer**. in a multi threaded app each thread contains its own stack and the own instruction pointer(since each thread is executing its own function and the instructions)
  - as we know the stack is the region in mem, where the vars are stored and passed to the functions, instruction pointer is the address of the next instruction to execute.

- **Context Switch** 
  - the act of stopping one thread and scheduling one thread in and one thread out and start thread 2 is called context switch.
  - the context switch is not cheap and it is the price we need to pay for the concurrency. since each thread consumes resources in the memory, when we switch to different thread (store data for the one thread and restore the data for the other).
  - threads consumes less resources than processes, so context switching b/w the threads from same process is lot cheaper than the context switch b/w different process.
- -**Thread Scheduling**
  - its first come first serve - but the problem with this approach is that if the very long thread comes first it cause starvation for other threads, is particular prob for ui threads. ie why the ui threads are shorter.
  - but now if we keep scheduling the shorter tasks first then our longer tasks will get get executed. these are the challenges that the os has to deal with in the multi-thread.
  - **Epoch** - the os divides the time into moderately sized pieces called epochs, in each epoch the os allocates diff time slice for each thread. not all the thread are get to run or complete  each epoch.
  - the decission on how to allocate the time slice for each thread is based on the **Dynamic Priority** the os maintains for each thread - Dynamic Priority = Static Priority + Bonuses.
- when to prefer the threads (multi-thread) ? - prefers only if the tasks shares lot of data, threads are much faster to create and destroy. Switching b/w the threads of same process is faster(context switch is shorter).
  - security and stability are of higher priority(separate process are separated from each other) where as one thread can bring down the entire app down.
### Chap 2. Thread creation
- Thread creation with the java.lang.Runnable (interface)
- the thread instance or obj itself is empty by default so we ve to pass the Runnable interface ex - Thread thread1 = new Thread(new Runnable) { override the run() }
- we can set the name, priority of the threads by the corresponding methods ex thread.setPriority(Thread.MAX_PRIORITY)// also we can set the priority range from (1 to 10 or any number)
- simply to caught the exception thrown by the thread we can use thread.setUnCaughtException(new Thread.UncaughtExceptionHandler(){})
- to create a thread class we can extends from the Thread (which implements Runnable interface) ex -refer the ex security vault impl and how long it takes the hacker to predict our code and breach our vault, in addition to that there is a police thread if the hacker will not hack the vault within 10s he will arrest em.
  - in that ex - we add all of the threads(3 threads) in the main method and iterate over them and call start(), in that all the threads inherits thread so we can treat em all as threads regardless of their concrete type, in oop this is called polymorphism.
### chap 3. Thread Coordination

- Thread Termination and Daemon thread: - thread Termination - as we know that the thread consumes resources (even if its empty) like the mem and kernel resources, CPU cycle and cache meme resource, so if the thread finishes the work (and the app is still running)and we ve to clean up the thread's resources.
- if thread is misbehaving we want to stop the thread, lastly by default the app will not stop as long as the thread is running/ alive even if the main thread already stopped running
- Thread.interrupt() we can use this method to interrupt the threads. we can use this method when the thread is executing an method that throws a interrupted exception. 2. if the thread code is handling the interrupt signal explicitly 
- "Refer the code (Thread-termination-example.zip)"
- Daemon Thread - is the thread that runs in the background, that do not prevent our app from exiting even if the main thread terminates.(ex file saving thread in the text editor)
  - 2. scenario - Code in the worker thread is not under our control and we do not want it to block our app from terminating. ex - worker thread that uses external lib -
  - just in our prev coding ex - just set the thread.setDaemon(true); (if we don't want to handle the thread interruption gracefully'), now it will not wait for the thread to finish, just our main thread ends it will make our entire app to terminate.
- **Thread Joining** - Thread.join() on other threads (coordination) this will give full control over other thread's execution, so we could run other tasks in parallel and get significant speed up and also be able to safely and correctly aggregate the results.
- as we know we don't ve the control on the order of thread execution ? and what if one thread depends on the execution of another ex thread a calc the vals which is the i/p of the thread B.

### Chap 4. performance Optimization

- introduction to performance and optimizing for Latency - ex - High speed Trading s/m - in this case the performance is normally measured in the latency.. in other words the faster the tx is the more performant the app will be, so the perf metric will be latency measured in the unit of time
- in this we gon see about the latency and the Throughput.. 
- **Latency** - The Time to completion of a task. measured in Time Units.
  - in this if we ve multiple task we can split em into individual tasks and then run em in parallel to each other with separate threads. and we want to achieve Latencey = T/N (time/no of sub task).
  - now if we break that task into many tasks what would be that number. in general that num'd be the no. of cpu cores we ve N = no .of cpu cores.
  - the cores is optimal only if all the threads are runnable and run w/o interruption(no IO/ blocking/ calls / sleep etc ) 2. then the second assumption is nothing is running that consumes lot of cpu resource
  - **HypreThreading** is most s/m can run 2 threads in a single cpu core
  - **inherent cost of parallelization and aggregation** breaking in to multiple tasks, creatin threads and pass the task to each thread, Time b/w thread.start() to thread getting scheduled, time until the last thread finishes and signals, time until the aggregating thread receives the signal and runs, time until aggregation of subresult into a single artifact.
  - **TAsks types** 1. Parallelizable tasks, 2. unbreakable tasks(force to run in single thread) 3. partially parallelizable and partially sequelization taks
- **Throughput** - is the amount of task completed in a given period of time and is measured in tasks/time unit. 
- when we ve a prog that its given the concurrent flow of task, and we want to perform as many tasks as possible as fast as possible, in that case thru put will be the right approach.
- as we seen above breaking and scheduling tasks into multiple task has a cost but in throughput its unnecessary.
- so the second approach is to perform each task on single thread - Throughput = N/T .
- by using some advanced techniques like thread pooling and cache friendly non blocking queues, we completely eliminate all the steps / cost in the above.. and we can achieve almost optimal throughput. there are 2 approaches we can make **1. thread pooling** 
- 1. **Thread Pooing** is just creating bunch of threads at once (pool of threads) and use em for the future tasks. once the threads are created they sit in the pool and the task are distributed for each thread through a **Queue**, and each thread will take the task from the queue whenever the thread is available.
  - JDK Comes with a few implementation of thread pools 1. **Fixed ThreadPool Executor** will create a pool with fixed number of threads. ex - Executor executor = Executors.newFixedThreadPool(4); // this also comes with built in queue, to add the task to the queue - ex Runnable task = executor.execute(task) - Refer the Code for Optimization-for-Throughput.zip file.
  - in that ex - we just built a http server wihich will send a flow of http requests as a i/p - in that ex - where we ve the book and read the vals / strings buffer in the book and return the count ex - localhost:8080/search?word=talk we can see the no. of times that word appears in the book - and for the testing he uses jmeter (load testing tool) 
- **Optimizing for latency (image Processing algo ex)** 
- refer the code optimizing-for latency.zip file - the each px has the A(transparancy) RGB bytes(4 bytes) . in that ex we can see we re shifting the bytes (val) by using >> operator and |= or eq operator
- in the isShadedGray() we use 30 as the arbitrary distance, to determine the proximity 

### Chap 5. Data Sharing b/w threads

- the stack + Instruction ptr = state of each thread's execution .. some of the stack's properties are all the vars belong to the thread executing on that stack(and other threads ve no access to em). the stack
- **Heap** is the shared memory region that belongs to the processor, all the data shared across all threads are stored in the heap and we can access and allocate objs on any moment.
  - all the objs (including new kw) will stored on heap (like string, arrays, collections etc), mem of class vars, static variables are stored on heap.. governed and managed by garbage collector GC.
  - refs are allocated on the stack and their mem vars on the heap.
- **Resource Sharing b/w Threads** - the resources are vars, collections, DS obj, file, msg queues etc. refer the code "Introduction to critical section example.zip"
  - **Atomic Operation** - is an operation or a set of operations it appears to the rest of the s/m as if it occurred once.. single step **All or Nothing** and no intermediate state

### Chap 6 Critical section and synchronization

- Refer the code (critical section example. zip)
- so far we ve seen, but the problem with is non concurrency issues, both threads can read and write which is not **Atomic**.
- we can ve the **thread Lock** to solve this issue.. for ex one thread enters the critical section (which has a lock so only one thread can perform the actions) and performs all the operations and exits the critical section. then the other thread enters the critical section and do the same.
- this we way we can really achieve the atomicity w/o worrying about the concurrency issue. there are few ways we can achieve this 
1. with the **synchronized** kw (locking mechanism) just make the method as the synchronized ex - public synchronized add(){}  lly we can make all the operations with the sync kw. so now only one thread can be able to exec this method.
2. second way is to define the block of code we want to execute in the critical section (instead of making the entire method synchronize) .. for that we ve to create an (some random obj) obj ex - Object obj = new Object(); // public add() { synchronized(obj){}} // sticking to this strategy we will get the best performance of our application.
   - this way gives lot more flexibility, inside one object of the class we can ve separate critical section sync on diff objs, the other benefit of this way is we don't ve to make the entire method sync (when only a small portion of the method contains the critical code)
- Synchronization is happening on an object level, and not Class level. Since thread1 and thread2 are executing methods from different objects, they do not block each other
- The synchronized block or the method is called **ReEntront** - means the thread can't prevent itself from entering a critical section.

- **Atomic operation volatile and metrics**
  - now the question is how do we know which operation is atomic and which is not ? 
  - if we mark every operation as synchronized then the parallelism will be minimized, and also we re paying the cost of context switching and the memory overhead of maintaining multiple threads.
  - Unfortunately most operations are not atomic. 
  - all the reference assignments are atomic means we can get and set the references to objs atomically. 
  - Then all the assignment to the primitive types are safe(except long and double which are 64 bytes java doesn't give any guarantee), that means we can reading and writing to them w/o sync .
    - for this long and double java provides a solution we can use the volatile kw on em, ex - volatile double x = 1.0; // now we can ve the atomic read and write.
  - java has the java.util.concurrent.atomic; package which is more advanced and provides lock free concurrent operations.( we will see in the later section)
  - **Metrics use cases** - the metric aggregation is when we run our app in the prod we ve to declare how long it is (code / piece of ops) gon to take
  - refer the code (application-metrics.zip )
- **Race condns and data races** (refer the code data-race-.zip) the data race is one of the main challenge that the devs may encounter.
- as we know the race condition is when multiple threads try to access the resources at a same time.  and the core of the problem is non atomic operations performed on the shared resource.
- the **data race** problem is when most of the times compiler or cpu may execute the instructions out of order to optimize the performance and utilization.  they will do so while maintaining the logical correctness of the code. out of order execution by the compiler and cpu are important features to speed up the code.
- the first solution to this data race is to use the synchronized keyword. 
- and the second solution or the way to this solution is to use the **volatile keyword** this will reduce the over head of locking and will guarantee the order.

- **Locking Strategy and Dead Lock** - (refer the code locking strategy and dead lock.zip)
  - when we re writing the multi threaded app we need to make choices like Fine grained locking(separate locks for all the resources) or coarse grained locking(single lock for the entire resource).
  - **Dead Lock** is a situation when everyone is tryna make progress but can't succeed ex - if we ve 2 threads, the thread a is scheduled and acquire the lock on resource A, then thread 2 is scheduled and acquire the lock on resource B. now this both threads can't access ex t2 can't access res A and th 1 can't access res B since those resources are already has the locks
  - lets see the conditions that can cause the deadlock so we can use the systematic solution to avoid it. 
  1. **Mutual Exclusion** - only one thread can ve the exclusive access to the resource. 
  2. **Hold and Wait** - At least one thread is holding the resource and is waiting for the resource.
  3. **Non pre-emptive allocation** - A resource is released only after the thread is done using it.
  4. **Circular Wait** A chain of at least 2 threads each one is holding one resource and waiting for the other resource.
- the obvious solution for the dead lock condition is to make sure at least one of those above condition is not met. 
- and the easiest way to avoid the dead lock is to avoid the last condition, avoid the circular wait - enforce a strict order in the lock acquisition(and maintaining this order for the resources in our code)
- the other technique may include is to introduce a **Watch Dog** - deadlock detection technique... and **Thread Interruption** technique (not possible with the synchronized) ... **tryLock operation** (not possible with the synchronized)
- Refer the ex - { dead-Lock.zip }

### Chap 7 Advanced Locking 

- from java.util.concurrent.locks.ReentrantLock ... and to create a new reentrant lock obj ex - Lock lockObj = new ReentrantLock (); then we can use the lock and **unlock()** on this reentrant obj ..
  - but still this method has the potential problem, we may forgot to use the unlock() or if there is any exception thrown during the lock () which will never reach the unlock()... so in that case its better to use the try catch block (put the entire critical section in the try block and then unlock() in the finally block) ... so this gives alot of control over the lock and more lock operations 
  - like the query methods for testing ex - getQueuedThreads() returns a list of threads waiting to acquire a lock. 
  - then getOwner() - returns a thread that currently owns the lock.
  - then isHeldByCurrentThread() - queries if the lock is held by current thread
  - isLocked() - Queries if the lock is held by any thread at this moment. 
  - as for the complex multithreading app needs to be tested thoroughly and methods like these can be very handy.
  - and the other area where the reentrant lock shines is the control over the fairness, by default the reentrant lock and the synchronized kw do not guarantee any fairness 
    - the fairness means if we ve multiple threads and one thread gets to acquire the lock multiple times, and the other threads will wait to starve and acquire the lock later 
  - ReentrantLock.lockInterruptibly() - generally if a particular thread which req an lockObj and other thread is holding the lock the current thread will suspended and not wake up until the lock is released, in this case calling the interrupt() on the lock obj will not do anything... but now if we call this lockInterruptbly() instead then we will get  out of this suspension..(it will forces us to surround with the try and catch block and catch the interrupted exception or some clean up operation and shutdown the lock operation gracefully)
    - some of the use cases of this method is - useful in Watch Dog, for the thread detection and recovery.
  - by far the most powerful lock that the reentrant lock provide us is the tryLock() operation. if the lock is available then the try catch returns true and acquire the lock, if its not available then it returns false and moves on to the next instruction.
  - from the above pt we can see that under no circumstances the tryLock() will block, regardless of the lock state it always returns immediately.
- **reentrant lock ui app ex** - refer the code - reentrantlock-example.zip file .. 
  - this ex we re simulating the price of the crypto and for the ui he uses java FX (cross platform framework), one thread for the price update and one thread for the ui

- **Reentrant Read / write lock** refer the code reentrant-read/write.zip . is a binary tree ex where the keys are the item (qty) and the values are the prices
- this lock has 2 locks in once read lock and write lock.
- as we know the solution to the race condn is complete mutual exclusion, regardless of operation (r/w/both) lock and allow only one thread to critical operation.
- and what about the work loads that mostly involves reading and very less write ops (like cache), we always need to ve the lock to protect from the reader from the writers, but what about the protection from the readers do we really need to protect the reader from the other reader the ans is we don't need any lock on the readers 
- **when to use** but there are cases where the read op is predominant (like cache, or read op are not so fast, or read from a complex DS) then the mutual exclusion of reading threads can negatively impact the performance.. and this is where the reentrant read write lock comes to help 
- ex - ReentrantReadWriteLock rwlock = new ReentrantReadWriteLock(); Lock read = rwlock.readLock(); Lock write = rwlock.writeLock(); //
- the read lock internally keeps the count of how many reader threads are holding at a given moment 

### Chap 8 Inter Thread Communication

- **Semaphore** - is like the permit issuing and enforcing authority, can be used to restrict the no. of users to a particular resource or group of resources, unlike lock (which allow only one user per resource) the semaphore can restrict any given no of users to our resources
- The semaphore is different from the lock in many ways, 1. firsly the semaphore doesn't ve the notion of owner thread(since many thread can acquire the permit) and also the same thread can acquire the semaphore multiple times 
- 2. the semaphore can be released by any thread even if the thread that hasn't acquired it
- the semaphore can be useful in the inter thread communication for the producer and consumer.
- **Conditional var** - the condn var is always associated with the lock. The Lock ensures atomic check and the modification of the shared vars, involved in the condn.

- **obj as condn var, wait(), notify(), notifyAll()** - refer the code wait-notify-ex.zip. This ex is about the read / write the 2d matrix in a file.. 
  - in that ex - if we plot the queue size against the time from the start of the app, we can see how in 6s the queue goes from 50k items and in that point the producer stop producing and it takes about 5s for the consumer to fully drain the queue.. which means our consumer is twice as slow as our producer.. in this rate if the i/p is coming from the more realistic source like n/w stream then our app would run out of mem and crash. 
  - to safe guard that we absolutely need to introduce back pressure in our producer
- wait() - causes the current thread to wait until the another thread wakes it up (in the wait state the thread is not consuming any cpu)
- 2 ways to wake up the waiting thread
- notify() - wakes up a single thread waiting on that obj.
- notifyAll() - wakes up all thread waiting on that obj.
- the one one caveat on using the wait(), notify() and notifyAll() is that we need to synchronized on that obj

### Chap 9 Lock Free algorithm, DS, and Technique

- so far we ve seen locks but now what's wrong with the locks ... well majority of the multi-thread programming can be done with the locks and most of the concurrency problems are easier and safer to solve with locks.
- there are few issues with locks such as 
  1. Deadlocks - are generally unrecoverable (one of the nassiest issues we can ve), the deadlocks can bring our app down/ halt and the more the locks we ve the more the chances of deadlock
  2. Priority Inversion - is the problem when we ve 2 threads sharing the resource and lock for the resource. one thread will be lowest priority than the other determined by the os... but if the lowest priority thread acquires the lock and preempted then the high priority thread can't progress coz of the low priority thread is not scheduled to release the lock, which leads to the performance issue.
  3. Thread Not releasing the lock (Kill Tolerance) - this is the worst problem, the thread dies or interrupted or forget to release the lock. This will leave all the thread hanging forever, unrecoverable just like the deadlock, to avoid dev need to write more complex code.
  4. performance overhead - in having contention over the lock, Thread A acquires a lock and thread B tries to acquire the lock and gets blocked. Thread B is schedule out (context switching), Thread B is scheduled back. additional overhead may not be noticeable for most of the app, but for latency imp app (like trading / stock price ) this overhead may be significant
- Non Atomic operations reasons - non atomic op on one shared resource, A single java op turns into multiple h/w ops ex - the count ++ turns into 3 op (read the val, calc, and store)
- **Lock Free Solution** - so basically the lock free operation boils down to utilize the operation which are guaranteed to be one h/w operation. A single H/w operation is **Atomic** by definition and therefore its thread safe. 
- as we already know some of the atomic operation such as REad / assignment on all primitive type (excl double / long), read / assignment on all refs, r/assignment on all volatile long and double
- then the second group of atomic operations we can perform comes from the java util (atomic packages) java.util.concurrent.atomic package. This internally uses unsafe class which provides access to the low level code, native methods, utilize platform specific impl of atomic operations. Refer the list of classes available in the atomic package, by using them we can design a very powerful lock free algorithm and DS. 

**Atomic Integer Class** - refer the code Atomic-integer.zip file. this is one of the most used atomic class in the atomic package. we can perform atomic operations on the integers. some of the methods we can use are incrementAndGet(), getAndReturn() lly for the decrement.
    - the atomic integer is a great tool for concurrent counting w/o the complexity of using lock. its as most as performant like the regular int but additionally provides the lock as protection.
- and sim advantage as the AtomicLong, and AtomicBoolean provides 

**AtomicReference<T> class** - we can perform atomic operations on the reference ex - AtomicReference(V initialValue); V get() to get the current val, void set(V newValue); to set the new val.
- by far the most important method of all the lock free atomic classes is the **Compare and Set (CAS)** operation, it conditionally assigns the new val if the condition met and ignores the new val if the current val != expected val..
  - and this CAS compiles into an atomic h/w operation, and this CAS is available all the atomic classes, many other atomic methods are internally impl using the CAS.
  - refer the code atomic-reference-example.zip file where we impl the our stack (linked list ex)

### Chap 10 I/O Bound Applications

- Blocking I/O - Negative impact of blocking on locks performance impact can be high.
- ex of i/o bound op is when connecting to the db and waiting for the result is long operation, and it is a blocking io operation during which the thread is blocked  and as we know the cpu will be idle. this type of application is i/o bound application (coz most of the time its spending in io bound as opposed to utilize the cpu). 
- some of the io bound applications are 1. web app 2. ml / big data based app

- **Thread per task / thread per request** - refer the io bound app ex.zip
- in that ex we can see on the positive side of the Thread per task improves the throughput and the h/w utilization(since we can able to process many tasks concurrently and be able to complete em much faster)
- issues - on the cons - as we know the threads are expensive, plus thread consume stack memory and other resources(too many threads leads to app crash lly too limited threads leads to lower throughput and cpu utilization)... 
  - and finally the price of context switching - OS is tryna utilize the cpu usage, as soon as there is a blocking call the os unschedules the thread, too many threads and frequent blocking calls leads to cpu being busy running the os code.

- The situation where most of the cpu is spent on the OS managing the s/m is called **Threshing** - and this situation we wanna avoid.

- **Asynchronous Non Blocking IO with thread per core** - as the name says the non blocking io doesn't block the thread instead it returns immediately so our thread can perform next instructions. 
- this nonBlockingIo() takes a callback, when the result is ready the code inside the callback will be executed. 
- so by making our code non blocking io, it will be immune to any issues close to the devices or app.
- some of the benefits of this model are Thread per core + non blocking io provides optimal performance.
- And the cons are, we already know it, code readability and also leads to callback hell. 
- so in summary both (blocking io and the nonblocking io has their own cons and pros) but what if we ve best of both worlds. (we will see in the next chap)

### Chap 11 Virtual Thread

- whenever we create the new Thread() obj (in jvm) we start the run() and in the start() we ask the os to create and start a os Thread belonging to our app process. and ask the jvm to fix size stack space to store the threads, local vars. 
- from that point on the os is responsible for scheduling and running the task on cpu. so in summary the Thread object inside the jvm is just a thin Wrapper around the OS thread.. and this type of jvm thread is also called as **platform Thread**.
- and as we already seen these platform threads are expensive and heavy as they map one to one with the os Thread(which is a limited resource) and also it will statically tied to the stack mem/space within the jvm.

- **Virtual Threads** are introduced in the jdk 21.
- unlike the platform threads the Virtual thread are fully belonged and managed by the jvm. And is not come with the fixed sized stack mem. and the os has no role or involve the creation of it or not even aware of it.
- and this virtual thread is just like any java obj allocated in the heap and it is reclaimed by the jvm's garbage collector when it is no longer needed
- unlike the platform threads the Virtual thread are vey cheap/faster to create in larger quantities.
- we can think if the jvm threads are just an java obj then how do they run in the cpu. well bts when we create a virtual thread the jvm will create a small internal pool of platform thread and whenever the jvm wants to run the virtual thread it will mount the virtual thread to the platform thread(with in its pool).
- Now when a platform thread is mounted to the virtual thread that platform thread is called **Carrier Thread**, now when the virtual thread is done with the execution the jvm will unmount the carrier from the virtual and makes the carrier available for other virtual threads. now that virtual thread is become garbage and the GC will clear the virtual thread.
- in that ex code if we select like 100 virtual threads then the jvm creates like 7 internal platform/ carrier threads and mounts the virtual/worker thread to em. and lly for the 1000 virtual threads jvm creates like 8(max no of core in his cpu) platform threads internally

- **High performance IO with virtual Threads** - refer the code High-performance-ex.zip file.
- if the virtual thread represents only cpu operations then won't be any performance benefits, however if virtual thread represents operations that requires the thread to wait (blocking ops, io etc) ie where we ve the performance benefits.
- so in summary by using the virtual thread we can achieve the best of both worlds thread per task model(blocking io) and thread per core model(non blocking io)

- **Virtual thread best practices** 
- 1. for the cpu bound tasks virtual threads provide no performance benefits, 2. in the latency related task virtual threads provide no performance benefits(ex db api calls). 3. short and frequent blocking calls are very inefficient.
- we should never create the fixed size pool of virtual threads. and the preferred way to use the virtual threads is Executors.newVirtualThreadPerTaskExecutor().
- the virtual thread is always a Daemon thread. VirtualThread.setDaemon(...)// throws exception.
- the virtual thread always has the default priority and setting the priority makes no difference
- observability and the debugging of virtual threads, by default the carrier threads hide the debugger from us. we can set up debugger or the break point as same as the platform threads 