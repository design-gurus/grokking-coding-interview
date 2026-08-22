# Pattern: Multi-threaded

> Several threads run at once and the answer has to be correct no matter how they interleave. The goal is correctness under concurrency, not a faster big-O.

## What it is

In a concurrency question, the code itself is usually short. What is being tested is whether you can state which parts must not overlap, and pick the right tool to enforce that.

Three tools cover almost every interview problem. A lock gives one thread exclusive access to shared state. A semaphore counts permits, so it can control how many threads pass or force an order. A condition variable lets a thread sleep until some fact becomes true, instead of spinning in a loop.

## Recognize it when

- The problem statement mentions threads, concurrency, or says several methods may be called at the same time.
- The order of printed output must be fixed even though the calls arrive in any order.
- It is a producer and consumer, a bounded buffer, or a shared counter.
- The role is backend, systems, or infrastructure, where these questions appear more often.

**Words that give it away:** "thread safe", "concurrently", "in order", "producer consumer", "deadlock", "synchronize", "at the same time".

## How it works

Ordering across threads is the most common shape, and semaphores handle it cleanly. Give each step its own permit, start with only the first one available, and have each step release the permit for the next.

```
first()   waits on nothing        releases permit for second
second()  waits on permit A       releases permit for third
third()   waits on permit B

whatever order the threads are started in, the output is fixed
```

The rule for condition variables is that the predicate is always checked in a loop, never in a single `if`. A waiting thread can wake up without the condition being true, both because of spurious wake-ups and because another thread may have taken the resource first.

## The code template

```python
import threading

class Ordered:
    """Prints first, second, third in that order, whatever order the threads run."""

    def __init__(self):
        self.second_ready = threading.Semaphore(0)   # starts with no permits
        self.third_ready = threading.Semaphore(0)

    def first(self, output):
        output("first")
        self.second_ready.release()                  # unblock the next step

    def second(self, output):
        self.second_ready.acquire()                  # wait for the previous step
        output("second")
        self.third_ready.release()

    def third(self, output):
        self.third_ready.acquire()
        output("third")


class BoundedBuffer:
    def __init__(self, capacity):
        self.items = []
        self.lock = threading.Lock()
        self.not_full = threading.Condition(self.lock)
        self.not_empty = threading.Condition(self.lock)
        self.capacity = capacity

    def put(self, item):
        with self.lock:
            while len(self.items) == self.capacity:   # while, never if
                self.not_full.wait()
            self.items.append(item)
            self.not_empty.notify()

    def take(self):
        with self.lock:
            while not self.items:
                self.not_empty.wait()
            item = self.items.pop(0)
            self.not_full.notify()
            return item
```

## Complexity

The asymptotic cost is normally the same as the single-threaded version. What is being judged is correctness: no race, no deadlock, and no thread left waiting forever.

## Variations

- Strict ordering of output across threads.
- Alternating output between two or more threads.
- Producer and consumer with a bounded buffer.
- Reader and writer locks, where many readers may share access but a writer needs it alone.
- Building a structure with worker threads, as in a multi-threaded web crawler.
- Dining philosophers, which is a deadlock-avoidance puzzle rather than a throughput problem.

## Problems that use it

Print in Order, Print FooBar Alternately, Print Zero Even Odd, Building H2O, Fizz Buzz Multithreaded, Web Crawler Multithreaded, Dining Philosophers, Design Bounded Blocking Queue.

## Common mistakes

- Deadlock from inconsistent lock ordering. If one thread takes A then B while another takes B then A, they can each hold what the other needs. Always acquire locks in the same order.
- Checking a condition with `if` instead of `while`, so a spurious wake-up lets a thread proceed when the condition is false again.
- Reading or writing shared state outside the critical section, including the check that decides whether to enter it.
- Busy waiting in a loop instead of blocking, which uses a whole core and can starve the thread you are waiting for.
- Forgetting to release a lock on an error path. Use a `with` block in Python, or `try`/`finally` in Java.
- Assuming a starting order. Threads may be scheduled in any order, and your solution must not depend on which one runs first.

## Go deeper

- The full pattern, with worked solutions in six languages and runnable tests: [Grokking the Coding Interview](https://www.designgurus.io/course/grokking-the-coding-interview)
- Concurrency in depth: [Grokking Multithreading and Concurrency](https://www.designgurus.io/course/grokking-multithreading-and-concurrency-for-coding-interviews)
