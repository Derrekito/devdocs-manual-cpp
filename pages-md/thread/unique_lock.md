# std::unique_lock

`std::unique_lock<Mutex>` (C++11) is the general-purpose RAII mutex
wrapper: like `lock_guard` it releases its mutex in the destructor, but
it can also unlock and re-lock mid-scope, defer locking to later, try
with a timeout, and be moved between scopes. Reach for it specifically
when you need one of those — unlocking early, deferred locking, or
using a `condition_variable` (which requires a `unique_lock<mutex>`).
Otherwise `lock_guard` is simpler and just as safe.

```cpp skip
std::unique_lock<std::mutex> lk(m);                    // locks now
std::unique_lock<std::mutex> lk(m, std::defer_lock);    // don't lock yet
std::unique_lock<std::mutex> lk(m, std::try_to_lock);   // try once, don't block
std::unique_lock<std::mutex> lk(m, std::adopt_lock);    // already locked
lk.unlock();   lk.lock();                    // release / reacquire mid-scope
```

### What you provide

- **Mutex** — the mutex type; must meet BasicLockable. If it also
  meets Lockable, so does `unique_lock<Mutex>` (usable with
  `std::lock`); if it meets TimedLockable, so does `unique_lock`
  (enabling `try_lock_for`/`try_lock_until`).
- **tag** (optional second constructor argument) — `std::defer_lock`
  (don't lock yet), `std::try_to_lock` (non-blocking attempt), or
  `std::adopt_lock` (this thread already holds the lock).

### Member functions

| Member | What it does |
| --- | --- |
| (constructor) | optionally locks/adopts the given mutex, per tag |
| (destructor) | unlocks the mutex, if owned |
| `operator=` | unlocks current mutex if owned, takes ownership of another |
| `lock()` | locks the associated mutex |
| `try_lock()` | attempts to lock without blocking |
| `try_lock_for(dur)` | tries to lock within a duration (TimedLockable) |
| `try_lock_until(tp)` | tries to lock until a time point (TimedLockable) |
| `unlock()` | releases the mutex, if owned |
| `swap(other)` | exchanges state with another `unique_lock` |
| `release()` | detaches the mutex **without** unlocking it |
| `mutex()` | pointer to the associated mutex |
| `owns_lock()`, `operator bool` | whether this lock currently owns its mutex |

### Guarantees and costs

- Movable, not copyable: MoveConstructible and MoveAssignable but not
  CopyConstructible or CopyAssignable — ownership of a lock can be
  transferred (e.g. returned from a function) but never duplicated.
- With `std::defer_lock`, the constructor takes no lock at all —
  `owns_lock()` is false until you call `lock()` yourself.
- `release()` gives up tracking the mutex without unlocking it — the
  caller becomes responsible for unlocking it through some other
  means; this is different from `unlock()`, which does unlock.

### Gotchas

- Calling `lock()`/`unlock()`/`try_lock()` when the lock already
  does/doesn't own the mutex throws `std::system_error` — a
  `unique_lock` tracks ownership precisely and enforces the state
  machine.
- Same beginner trap as `lock_guard`: forgetting to name the variable
  (`std::unique_lock<std::mutex>(m);`) silently fails to hold the
  lock.
- Locking two mutexes with two separate `unique_lock(m, ...)` calls,
  each blocking, can deadlock against another thread locking them in
  the opposite order — use `std::lock(lk1, lk2)` (as shown below) or
  `scoped_lock` to acquire several at once safely.

### Example

```cpp
#include <iostream>
#include <mutex>
#include <thread>

struct Box
{
    explicit Box(int n) : num_things{n} {}
    int num_things;
    std::mutex m;
};

void transfer(Box& from, Box& to, int n)
{
    std::unique_lock lock1{from.m, std::defer_lock};
    std::unique_lock lock2{to.m, std::defer_lock};
    std::lock(lock1, lock2);  // locks both, no deadlock

    from.num_things -= n;
    to.num_things += n;
}

int main()
{
    Box a{100}, b{50};

    std::thread t1{transfer, std::ref(a), std::ref(b), 10};
    std::thread t2{transfer, std::ref(b), std::ref(a), 5};
    t1.join();
    t2.join();

    std::cout << a.num_things << ' ' << b.num_things << '\n';
}
```

```text
95 55
```

### Reference

```cpp skip
template< class Mutex >
class unique_lock;  // (since C++11)
```

Formally: `unique_lock` meets BasicLockable unconditionally, Lockable
when `Mutex` does, and TimedLockable when `Mutex` does. LWG 2981
(applied to C++17) removed a redundant deduction guide from
`unique_lock<Mutex>`.

### See also

- **lock_guard** — simplest RAII wrapper: locks, then unlocks at scope exit
- **scoped_lock** (C++17) — RAII wrapper locking several mutexes, deadlock-free
- **lock** — locks several mutexes together, avoiding deadlock
- **condition_variable** — waits using a `unique_lock<mutex>`
- **mutex** — the basic mutual-exclusion primitive

---
*Source: https://en.cppreference.com/w/cpp/thread/unique_lock*
