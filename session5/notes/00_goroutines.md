## Concurrency

In JavaScript, you can perform asynchronous operations. You typically
give these operations a callback. While the async operation is running,
your code can continue to run. When the async operation is done, your
callback is called.

This an be more performant than synchronous operations. For instance:

```
makeSlowAsyncRequest1(function (result1) {
  console.log(result1);
});
makeSlowAsyncRequest2(function (result2) {
  console.log(result2);
});
```

You don't have to wait for the first request to finish to start the
second request. If we *blocked* for the first request to finish, it
would be wasteful; we would be doing nothing during the waiting time.
Asynchronous operations let us use that waiting time to run other
JavaScript code. A common thing to do is launch additional async
operations, so that the waiting times overlap or "coalesce".

When we do other work while waiting for an operation to finish, this is
called *concurrency*.

## Ruby vs JavaScript

In JavaScript, all operations that could block (for instance, make a
network request, read a file, read user input) are asynchronous.

If you've ever written crazy bubble sort, you know this can make simple
programs complicated to write. Let me give an example. Say I want to
write a sequence of lines to a file, one-by-one. In a synchronous
language like Ruby, I write:

```ruby
lines_to_write = ["abc", "def", "ghi", "jkl"]
lines_to_write.each do |line|
  # this works because we don't write the next line until the current
  # line is written.
  file.puts line
end
```

In JavaScript (before `async`/`await`), this was a pain in the ass.

```js
linesToWrite = ["abc", "def", "ghi", "jkl"];
function writeLine(idx) {
  if (idx === linesToWrite.length) {
    return;
  }

  asyncWriteToFile(file, linesToWrite[idx], function () {
    writeLine(idx + 1);
  });
}
writeLine(0);
```

Of course, we can use `async` and `await` now in JavaScript, which are
the best things in the world:

```js
linesToWrite = ["abc", "def", "ghi", "jkl"]
for (line of linesToWrite) {
  await promiseReturningWritetoFile(file, line);
}
```

Let's go back to Ruby though. Because `file.puts` is blocking, and
freezes our program, how can Ruby concurrently write to separate files?
For instance, in JavaScript I can write:

```js
async function writeLinesToFile(file, linesToWrite) {
  for (line of linesToWrite) {
    await promiseReturningWritetoFile(file, line);
  }
}

writeLinesToFile(fileA, ["abc", "def"]);
writeLinesToFile(fileB, ["ghi", "jkl"]);
```

In this case, even though I must wait to write `"def"` after `"abc"`, I
can write `"ghi"` without waiting for either because this is an
unrelated write to a separate file. This is efficient; work that *can*
be performed concurrently will be, while work that *must* be
*serialized* (each task run one after another) will be.

We haven't seen how to do concurrency in Ruby. For instance:

```ruby
def write_lines_to_file(file, lines_to_write)
  lines_to_write.each do |line|
    # this works because we don't write the next line until the current
    # line is written.
    file.puts line
end

write_lines_to_file(fileA, ["abc", "def"])
write_lines_to_file(fileB, ["ghi", "jkl"])
```

## Threads

In Ruby (and many other programming languages), we use *threads* to
perform operations concurrently.

```ruby
def write_lines_to_file(file, lines_to_write)
  lines_to_write.each do |line|
    # this works because we don't write the next line until the current
    # line is written.
    file.puts line
end

# Thread.new creates a new worker, and doesn't wait for the worker to
# finish its task.
Thread.new do
  write_lines_to_file(fileA, ["abc", "def"])
  puts "fileA is finished being written!"
end
Thread.new do
  write_lines_to_file(fileB, ["ghi", "jkl"])
  puts "fileB is finished being written!"
end
```

Creating a new thread splits off a worker in the Ruby program. The
worker can run their code independently. If a worker is blocked (for
instance, blocked waiting for a line to be written to a file), other
workers can continue to move forward, because they are independent.

## Threading vs asynchronous models

Serial execution is easier to reason about and understand, because you
know that operation `n+1` will be performed only after operation `n` is
complete. It is convenient that Ruby makes this the default.

In the limited scenarios when we need concurrent execution, Ruby lets us
easily opt in by using threads. For instance, we can write
`write_lines_to_file` in a simple, serial, synchronous style. When we
want to make concurrent calls to `write_lines_to_file`, we can use
`Thread.new`. The desire to use concurrency *outside*
`write_lines_to_file` does not need to infect the definition of
`write_line_to_file` with additional complexity.

JavaScript, especially before `await`, imposes a lot of burden on us
whenever we wanted to perform a series of blocking operations in serial.
This is frustrating if 95% of our code is serial, and only 5% is
concurrent. To enable the 5% of the code to run concurrently, JavaScript
forces us to write the entire 100% in a complicated asynchronous style.

## Parallelism

We've been talking about how both JavaScript and Ruby can use "waiting
time" while an operation is blocked to run other code concurrently. This
is particularly useful if you start one API request, then immediately
start a second so that the two operations' wait times can overlap. This
form of multitasking is called *concurrency*.

You may be aware that in JavaScript, only one line of JavaScript is
running at any single time. Some JavaScript code can be running while an
async operation is waiting to be completed, but no two lines of
JavaScript code will ever be running simultaneously. For instance:

```js
function doUselessWork() {
  function (var idx = 0; idx < 10000; idx++) {
    // Do useless work
  }
  console.log("USELESS WORK COMPLETED!")
}

setTimeout(doUselessWork, 1);
setTimeout(doUselessWork, 2);
```

It might be nice if JavaScript ran the two calls of `doUselessWork` *in
parallel*. But that is not how things work. Assume `doUselessWork` takes
100 milliseconds to run. The first call will start after 1 millisecond,
but will still be running at 2 milliseconds. JavaScript can't start the
second call yet, because JavaScript has no *parallelism*. At 101
milliseconds, the first call to `doUselessWork` completes. This clears
the event loop. Now we can run the second call of `doUselessWork`, which
was waiting to run since 2 milliseconds. The overall time to run is 201
milliseconds.

In most languages, threads let us run code in parallel. For instance:

```ruby
def do_useless_work
  10000.times do
    # Do useless work
  end
  puts "USELESS WORK COMPLETED!"
end

Thread.new { do_useless_work }
Thread.new { do_useless_work }
```

could, in principle, take only 100 milliseconds. The way this can happen
is that different threads can be excuted on different CPU cores. Each
CPU core in your computer can run different threads simultaneously. The
simultaneous execution of your code on different cores is called
*parallelism*.

Parallelism is subtly different than concurrency. Concurrency is like
"multitasking"; one worker works on multiple tasks concurrently by doing
work on task2 while task1 is blocked. Parallelism is like employing
multiple workers: worker1 can be actively working on task1 even while
worker2 is actively working on task2.

## Web applications are concurrency heavy

Parallelism is more powerful than concurrency. If you give tasks to
separate workers, then by definition a block on task1 doesn't stop
worker2 from making progress on task2. So parallelism implies
concurrency.

On the other hand, languages can be concurrent but not parallel if they
don't use other cores to perform simultaneous work.

So why doesn't JavaScript allow code to be executed in parallel?

First, it turns out that opportunities for concurrency are very common,
but opportunities for true parallelism are much rarer.

Web applications have a lot of opportunity for concurrency. When user1
makes a web request, the application needs to make several DB requests
to satisfy the request. DB requests are blocking. In the meantime, the
application wants to accept a request from user2 so that it can begin
the DB requests for user2 without waiting for the DB requests for user1
to complete. This is concurrency.

The other major form of web application logic is template rendering. In
a single-threaded environment like Node, I cannot in parallel render a
template for user1 while I am rendering a template for user2. That's
because template rendering is just running JavaScript code. Luckily,
template rendering is typically many many times faster than waiting for
DB requests. One reason is that DB requests typically need to read data
from the hard disk, which is thousands of times slower than rendering a
template, which can be done entirely in RAM.

Therefore, even though Node misses an opportunity for parallelism with
the template rendering, it has already achieved a major win through
concurrent execution of the DB requests.

## Other tasks are parallelism heavy

Imagine that I have stolen your password hash, and I want to recover
your password. To do so, I will work my way through a dictionary of
common passwords. I need to hash each password to see if any of them
match your password hash.

If I were hashing passwords by hand, I would want to get 15 of my
friends together, split up the dictionary of common passwords 16-ways,
and have us each hash our 1/16th in parallel. This would be 16x faster
than doing it alone. We say that the task is *embarassingly parallel*,
because it is so easy to parallelize.

Password hashing doesn't involve any blocking or waiting or "down time."
Therefore, there is no opportunity for concurrency. However, parallel
execution by many workers is helpful.

Password hashing is an example of something that Node would be very
poorly suited for, because it cannot take advantage of multiple cores.

## Parallel template rendering with multiple Node applications

Even though I achieve a huge efficiency win in Node by exploiting
concurrent execution of DB queries, can I also perform template
rendering in parallel? That would give me a further speedup.

The answer is: not inside one Node process. However, I can start up
multiple copies of my Node web application on one machine. If I have 16
cores, I can start 16 copies of my Node application. The 16 copies can
process requests in parallel, because they are entirely separate
programs. Your operating system will try to run programs in parallel
whenever it can.

If requests A and B both go to app copy 1, then template rendering for
requests A and B cannot be performed in parallel (though their DB
requests can be performed concurrently). But if requests C and D are
sent to app copy 2, then the rendering for template C can be performed
by app copy 2 in paralell to the rendering for template A performed by
app copy 1.

To use my many application copies, I stick a *load balancer* in front of
of my 16 copies of the Node web application. Typically you are already
load balancing requests across multiple application copies running on
separate machines for *redundancy* (so that if one machine fails,
requests can still be processed by the other machine). Therefore, it is
not particularly burdensome to run multiple copies of your application
on a single machine.

This approach does have some limitations. If requests A and B are both
sent to app copy 1, but the template rendering for A takes an unusually
long time, it is not possible to transfer the work of rendering template
B to another app copy, even if other app copies have no current work.
Request B is stuck waiting for the template rendering of request A to
finish.

## Why not threads?

Since threading seems easier (most execution is serial), why not use it
all the time? We've seen that single-threaded Node code takes advantage
of concurrency, and can even do parallelism if you spin up multiple app
copies. But that seems like a bit of a hack. For instance, we saw that
work cannot be transfered between app copies. So even though in
principle every app copy gets 1/16th of the work, slow requests can
cause unbalancing.

Another approach would be to start one thread per web request. Then, if
rendering for request A was taking a long time, it wouldn't stop request
B from getting its rendering done on another thread. Why not do this?

* Virtual memory means that you can make a 2MB stack without allocating
  all that memory (since only a page of 4KB will be allocated).
* Faster synchronization by not allowing rescheduling during a critical
  section.
* Doesn't have to go through the kernel to reschedule. No kernel
  objects.

## Parallelism and coordination
