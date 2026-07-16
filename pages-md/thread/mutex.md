# std::mutex

`std::mutex` (C++11) is the basic mutual-exclusion primitive: whichever
thread successfully calls `lock()` *owns* the mutex until it calls
`unlock()`, and every other thread trying to lock it blocks (or gets
`false` from `try_lock()`) until then. Don't call `lock()`/`unlock()`
by hand — a single missed `unlock()` on an exception path or an early
`return` leaves the mutex locked forever. Use `std::lock_guard`,
`std::unique_lock`, or `std::scoped_lock` (C++17) instead; they release
the mutex in their destructor no matter how the scope is exited.

```cpp skip
std::mutex m;
{
    std::lock_guard<std::mutex> lk(m);  // preferred: exception-safe
    // ... access data shared across threads ...
}                                         // released here, always

m.try_lock();  // non-blocking attempt; returns bool, still manual unlock
```

### Member functions

| Member | What it does |
| --- | --- |
| (constructor) | constructs an unlocked mutex |
| (destructor) | destroys the mutex; UB if any thread still owns it |
| `lock()` | blocks until this thread owns the mutex |
| `try_lock()` | attempts to lock without blocking; returns success |
| `unlock()` | releases ownership |
| `native_handle()` | implementation-defined OS handle |

### Guarantees and costs

- Exclusive and **non-recursive**: a thread must not already own the
  mutex when it calls `lock()` or `try_lock()` — doing so is undefined
  behavior (see `recursive_mutex` if you need to re-lock from the same
  thread).
- `try_lock()` is allowed to fail spuriously even when the mutex is
  free; treat a `false` return as "didn't get it," not as proof it was
  contended.
- `mutex` is neither copyable nor movable.

### Gotchas

- Destroying a `mutex` while a thread still owns it, or exiting a
  thread while it owns a `mutex`, is undefined behavior — every lock
  must be released before either happens.
- Locking a `mutex` you already hold on the same thread (e.g. two
  nested `lock_guard`s on the same mutex) deadlocks immediately; it
  will not detect or throw.
- Manual `lock()`/`unlock()` pairs don't survive exceptions — any
  throw between them leaves the mutex locked. This is the whole reason
  the RAII wrappers exist.

### Example

```cpp
#include <iostream>
#include <mutex>
#include <thread>

int counter = 0;
std::mutex counter_mutex;

void increment(int times)
{
    std::lock_guard<std::mutex> lock(counter_mutex);
    for (int i = 0; i < times; ++i)
        ++counter;
}

int main()
{
    std::thread t1(increment, 1000);
    std::thread t2(increment, 1000);
    t1.join();
    t2.join();

    std::cout << counter << '\n';
}
```

```text
2000
```

### Reference

```cpp skip
class mutex;  // (since C++11)
```

Formally: `mutex` satisfies the Mutex and StandardLayoutType
requirements.

### See also

- **lock_guard** — simplest RAII wrapper: locks, then unlocks at scope exit
- **unique_lock** — movable wrapper supporting deferred/timed locking
- **scoped_lock** (C++17) — RAII wrapper locking several mutexes, deadlock-free
- **recursive_mutex** — mutex a thread may lock more than once
- **condition_variable** — blocks until notified, waits on a `unique_lock`

---
*Source: https://en.cppreference.com/w/cpp/thread/mutex*
