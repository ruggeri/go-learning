**TODO**: Write the channels/goroutines lecture.

1. Goroutines invoked by `go f()`
2. Channels `ch := make(chan int)` Now you can `ch <- 123` and `<-ch`.
3. Can give an argument to decide how much to buffer
4. Can `close(ch)` when done. Then someone else can range over
   it. Only needed if someone is ranging; otherwise it's unnecessary
   to close. You can check if `val, ok := <-ch` if needed
5. `select`:

    select {
    case c <- 123:
      fmt.Println("Sent value!")
    case <-quit:
      fmt.Println("Someone sent us a value to quit!")
    }

6. You can use a `default` case to be non-blocking.
7. They show us how to use `sync.Mutex` which is simple.

https://github.com/golang/go/wiki/LearnConcurrency

* Concurrency
    * https://blog.golang.org/pipelines
    * https://blog.golang.org/go-concurrency-patterns-timing-out-and
    * https://blog.golang.org/share-memory-by-communicating

## Goroutine and Channel Usecases

* Concurrent independent processing.
* If you need to wait until a bunch of subtasks are done, use channel.
  * Your channel can return nothing or a result.
  * Could be you saved your file to enough people. You may decide you
    don't have to wait anymore.
* You may want to cancel as soon as you get a good enough answer. To do
  so, you can also pass the worker a cancelation channel.
  * Can be in form of timeout.
  * Can be timeout if you have a good enough answer.
* Channels can be useful for many-to-many. A common scenario is a worker
  pool.
* Your worker pool can enforce rate-limiting.
  * Basically use as a counting semaphore.
  * You can also can just kill a task if over the limit.
* Actor model style synchronization. Say only one thread can use a
  resource. You can just start a goroutine for that resource.
  * I'm thinking of a TCP chat server.

Useful resource: https://go101.org/article/channel-use-cases.html
