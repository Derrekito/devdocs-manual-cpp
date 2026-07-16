# std::lock_guard

`std::lock_guard<Mutex>` (C++11) is the simplest RAII mutex wrapper:
construct it with a mutex and it locks that mutex immediately; when
the `lock_guard` goes out of scope, its destructor unlocks it — always,
including when an exception unwinds the scope. Reach for `lock_guard`
first; move to `unique_lock` only when you need to unlock early, defer
locking, or use a condition variable, or to `scoped_lock` (C++17) when
locking more than one mutex at once.

```cpp skip
std::lock_guard<std::mutex> lk(m);         // locks m now
std::lock_guard<std::mutex> lk(m, std::adopt_lock);  // already locked
```

### What you provide

- **Mutex** — the mutex type; must meet the BasicLockable requirements
  (has `lock()`/`unlock()`). Deduced from the constructor argument.

### Member functions

| Member | What it does |
| --- | --- |
| (constructor) | locks the given mutex, or adopts one already locked |
| (destructor) | unlocks the mutex |
| `operator=` | deleted — not copy-assignable |

### Guarantees and costs

- Non-copyable; there's nothing to move either — a `lock_guard`'s
  entire purpose is tying one lock to one scope.
- With the `std::adopt_lock` tag, the constructor does **not** lock —
  it assumes the calling thread already owns the mutex, and only takes
  over responsibility for unlocking it.

### Gotchas

- The classic mistake is not naming the variable:
  `std::lock_guard<std::mutex>(m);` default-constructs a variable
  named `m` shadowing the mutex, and `std::lock_guard<std::mutex>{m}`
  builds a temporary that is destroyed immediately — neither actually
  holds the lock for the rest of the scope. Always give it a name:
  `std::lock_guard<std::mutex> lk(m);`.
- `lock_guard` can't unlock early or re-lock; if you need that,
  it's a sign you want `unique_lock` instead.
- Locking the same mutex with two `lock_guard`s on one thread (nested,
  non-recursive) deadlocks — the type gives no protection against
  that.

### Example

```cpp
#include <iostream>
#include <mutex>
#include <thread>

int counter = 0;
std::mutex m;

void increment(int times)
{
    const std::lock_guard<std::mutex> lock(m);
    for (int i = 0; i < times; ++i)
        ++counter;
}  // m unlocked here

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
template< class Mutex >
class lock_guard;  // (since C++11)
```

Formally: `Mutex` must meet BasicLockable. LWG 2981 (applied to C++17)
removed a redundant deduction guide from `lock_guard<Mutex>`.

### See also

- **unique_lock** — movable wrapper, deferred locking and early unlock
- **scoped_lock** (C++17) — RAII wrapper locking several mutexes, deadlock-free
- **mutex** — the basic mutual-exclusion primitive

---
*Source: https://en.cppreference.com/w/cpp/thread/lock_guard*
