## Advanced

* Effective Go (I think this is overkill for students)
    * I reviewed this.
* https://golang.org/ref/spec (spec of the implementation)
    * I reviewed this.
* https://golang.org/pkg/ (list of all packages)
    * I already reviewed and pulled out the interesting ones.
* https://golang.org/doc/faq
    * A bunch of trivia questions uninteresting to students I think.
    * I reviewed this.

## Reviewed

* context (https://blog.golang.org/context)
    * Not super interesting. Just used by requests to signal: "hey,
      cancel this request." As in, let's say you make a DB call, and
      hand it to a SQL driver. But then that takes too long. In the
      meantime, you don't need the answer anymore. So you can cancel the
      request by closing a channel.

## Assorted Advanced Packages (unreviewed)

reflect
unsafe
atomic

Also: go test.
