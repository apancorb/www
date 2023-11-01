---
title: "Building a Concurrent Non-Blocking Cache in Go"
author: "Antonio Pancorbo"
summary: "Learn how to build a high-performance, concurrent, non-blocking cache in Go"
date: 2023-07-23
cover:
    image: /posts/concurrent_non_blocking_cache_go.jpg
    caption: "Image from [Konrad Reiche](https://speakerdeck.com/konradreiche/lets-build-a-concurrent-non-blocking-cache?slide=5)"
tags: ["go", "programming", "cache", "concurrency", "performance"]
draft: false
---

In this post we will build a concurrent non-blocking cache. A concurrent
non-blocking cache with memoization is crucial because it optimizes the
performance of frequently called functions by storing their computed results.
By allowing multiple goroutines to access the cache simultaneously, without
blocking, it avoids redundant computations and reduces processing time, making
it an essential technique for building highly efficient and responsive applications.

To make the distinction clearly, in a *blocking cache* if a cache request results
in a miss, the cache must wait for the result of the slow function, until then
it is blocked. A *non-blocking blocking cache* has the ability to work on other
requests while waiting for the result of the function. Thus, our solution
ensures concurrency safety and mitigates contention issues commonly found in
designs that rely on a single lock for the entire cache.

Here is the first version of the cache:

```go
// Package memo provides a concurrency-unsafe
// memoization of a function of type Func.
package memo

// A Memo caches the results of calling a Func.
type Memo struct {
  f Func
  cache map[string]result
}

// Func is the type of the function to memoize.
type Func func(key string) (interface{}, error)

type result struct {
  value interface{}
  err error
}

func New(f Func) *Memo {
  return &Memo{f: f, cache: make(map[string]result)}
}

// NOTE: not concurrency-safe!
func (memo *Memo) Get(key string) (interface{}, error) {
  res, ok := memo.cache[key]
  if !ok {
    res.value, res.err = memo.f(key)
    memo.cache[key] = res
  }
  return res.value, res.err
}
```

A `Memo` instance consists of the function `f` to memoize, represented as
type `Func`, and the cache, which maps strings to results. Each result
in the cache is a pair of values returned by a call to `f` - a value and an error.

In this basic implementation of the `Get` method we can see that the first
check is if we have the result for the given `key` stored in the cache. In the 
case we don't, we simply call the function `f` with the given `key` and store
it in the cache for future calls with that same `key`. 

Caching HTTP requests can be beneficial as it allows the local storage of
previously retrieved responses, reducing the need to repeatedly fetch the
same data from external sources. This can lead to significant performance
improvements by lowering latency, reducing bandwidth usage, and offloading
the server. Consider the following snippet where we test the cache by using
a function that performs an HTTP GET request given some URL.

```go
for url := range incomingURLs() {
  start := time.Now()
  value, err := m.Get(url)
  if err != nil {
    log.Print(err)
  }
  fmt.Printf("%s, %s, %d bytes\n",
  url, time.Since(start), len(value.([]byte)))
}
```

The output of the test looks like:

```text
=== RUN   Test
https://golang.org, 702.565334ms, 59291 bytes
https://godoc.org, 353.220334ms, 31436 bytes
https://play.golang.org, 762.043042ms, 28980 bytes
http://gopl.io, 1.028100833s, 4154 bytes
https://golang.org, 1.917µs, 59291 bytes
https://godoc.org, 250ns, 31436 bytes
https://play.golang.org, 125ns, 28980 bytes
http://gopl.io, 167ns, 4154 bytes
--- PASS: Test (2.85s)
```

As we can see, the first requests take up to 1s to complete while the 
subsequents ones are much faster since we are retrieving the results
from the cache directly. Now, this works well if there is only one
goroutine that will always access this same instance of the cache but
if there are multiple goroutines concurrently calling the `Get` method 
we will start to get a *data race* as with the test shown bellow.

```go
for url := range incomingURLs() {
  n.Add(1)
  go func(url string) {
    start := time.Now()
    value, err := m.Get(url)
    if err != nil {
      log.Print(err)
    }
    fmt.Printf("%s, %s, %d bytes\n",
    url, time.Since(start), len(value.([]byte)))
    n.Done()
  }(url)
}
n.Wait()
```

Which outputs the following, when the test is ran with the `-race` flag:

```text
WARNING: DATA RACE
Write at 0x00c0002c6908 by goroutine 88:
  gopl.io/ch9/memo1.(*Memo).Get()
      /home/codespace/sandbox/gopl.io/ch9/memo1/memo.go:35 +0x13d
  gopl.io/ch9/memotest.Concurrent.func1()
      /home/codespace/sandbox/gopl.io/ch9/memotest/memotest.go:93 +0xea
  gopl.io/ch9/memotest.Concurrent.func2()
      /home/codespace/sandbox/gopl.io/ch9/memotest/memotest.go:100 +0x58
```

The provided information indicates that two goroutines have independently
updated the cache map without any synchronization. To achieve concurrency
safety in the cache, the simplest approach is to implement monitor-based
synchronization. By adding a mutex to the `Memo`, we can acquire the mutex
lock at the beginning of the `Get` method and release it before returning,
ensuring that both cache operations occur within a critical section.
This ensures that only one goroutine can access and modify the cache at a time,
preventing potential data races and maintaining consistency in the cached data.

```go
type Memo struct {
  f Func
  mu sync.Mutex // guards cache
  cache map[string]result
}

// Get is concurrency-safe.
func (memo *Memo) Get(key string) (value interface{}, err error) {
  memo.mu.Lock()
  res, ok := memo.cache[key]
  if !ok {
    res.value, res.err = memo.f(key)
    memo.cache[key] = res
  }
  memo.mu.Unlock()
  return res.value, res.err
```

Unfortunately, this change results in a reversal of our earlier performance gains,
we have simply created a blocking cache. Holding the lock for the duration of each
call to `f` in the `Get` method serializes all the I/O operations that were initially
intended for parallelization. What we require is a non-blocking cache, one that
allows concurrent calls to the memoized function without serialization.

```go
func (memo *Memo) Get(key string) (value interface{}, err error) {
  memo.mu.Lock()
  res, ok := memo.cache[key]
  memo.mu.Unlock()
  if !ok {
    res.value, res.err = memo.f(key)

    // Between the two critical sections, several goroutines
    // may race to compute f(key) and update the map.
    memo.mu.Lock()
    memo.cache[key] = res
    memo.mu.Unlock()
  }
  return res.value, res.err
}
```

With improved performance and avoiding data races, we observe some URLs are being
fetched twice when multiple goroutines call `Get` for the same URL simultaneously.
Both check the cache, find no value, and invoke the slow function `f`, updating
the map with their results. This can lead to overwriting one result with another,
creating redundant work. Ideally, we seek to avoid this and suppress duplicates,
which is sometimes referred to as *duplicate suppression*.

```go
type entry struct {
  res result
  ready chan struct{} // closed when res is ready
}

func New(f Func) *Memo {
  return &Memo{f: f, cache: make(map[string]*entry)}
}

type Memo struct {
  f Func
  mu sync.Mutex // guards cache
  cache map[string]*entry
}

func (memo *Memo) Get(key string) (value interface{}, err error) {
  memo.mu.Lock()
  e := memo.cache[key]
  if e == nil {
    // This is the first request for this key.
    // This goroutine becomes responsible for computing
    // the value and broadcasting the ready condition.
    e=&entry{ready: make(chan struct{})}
    memo.cache[key] = e
    memo.mu.Unlock()
    e.res.value, e.res.err = memo.f(key)
    close(e.ready) // broadcast ready condition
  } else {
    // This is a repeat request for this key.
    memo.mu.Unlock()
    <-e.ready // wait for ready condition
  }
  return e.res.value, e.res.err
}
```

In the following `Memo` version, each map element is a pointer to an `entry`
structure. Each entry holds the memoized result of a call to the function `f`,
as before, and also contains a channel named `ready`. After setting the entry's
result, the channel will be closed to broadcast to other goroutines that it is
now safe for them to read the result from the entry.

The updated `Get` method acquires a mutex lock to guard the cache map, checks for
an existing entry, and creates a new one if needed. If an existing entry is found,
the calling goroutine waits for the 'ready' condition to read the result. It achieves
this by reading from the ready channel, which blocks until the channel is closed.

When there is no existing entry, inserting a new 'not ready' entry into the map
makes the current goroutine responsible for invoking the slow function, updating
the entry, and broadcasting its readiness to other waiting goroutines. But notice
how it still allows other goroutines to keep calling the slow function `f` concurrenlty.

The variables `e.res.value` and `e.res.err` in the entry are shared among multiple
goroutines. While the creating goroutine sets their values, other goroutines read
them after the 'ready' condition is broadcast. Despite multiple accesses, no mutex
lock is needed. The ready channel closing occurs before any other goroutine receives
the broadcast, ensuring the write to these variables in the first goroutine happens
before they are read by subsequent goroutines, avoiding any data race. With this
implementation, our concurrent and non-blocking cache is complete!

---

This implementation has been adapted from *The Go Pogramming Language by Alan
A. A. Donovan and Brian W. Kernighan*. The code can be explored in this
[Github](https://github.com/adonovan/gopl.io/blob/master/ch9/memo1/memo.go) repository.
Additionally, I highly recommend watching this [Youtube](https://www.youtube.com/watch?v=KlDWmTcyXdA)
video by Konrad Reiche on this same topic!

---

### References
1. Donovan, A.A.A. and Kernighan, B.W. (2020) The go programming language.
New York, N.Y: Addison-Wesley. 
