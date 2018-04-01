## Concurrency

* There's a section in the Little Go Book.
* https://github.com/golang/go/wiki/LearnConcurrency

## Tour

* https://tour.golang.org/list

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
