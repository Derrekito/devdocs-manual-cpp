# std::scoped_lock

`std::scoped_lock<MutexTypes...>` (C++17) is the RAII wrapper to reach
for when a scope needs to lock **more than one mutex at once**: it
locks all of them using the same deadlock-avoidance algorithm as
`std::lock`, and unlocks all of them in its destructor. With exactly
one mutex it behaves like `lock_guard`; with zero it's a no-op. For a
single mutex, or when you need `unique_lock`'s extra features (early
unlock, deferred locking, `condition_variable`), reach for those
instead.

```cpp skip
std::scoped_lock lk(m1, m2, m3);   // locks all three, deadlock-free
std::scoped_lock lk(m);             // one mutex: same as lock_guard
std::scoped_lock<> lk;              // zero mutexes: does nothing
```

### What you provide

- **MutexTypes...** — the mutexes to lock, one argument each. Each
  type must meet the Lockable requirements, unless exactly one mutex
  is given, in which case it need only meet BasicLockable.

### Member functions

| Member | What it does |
| --- | --- |
| (constructor) | locks the given mutexes (deadlock-free if more than one) |
| (destructor) | unlocks the mutexes |
| `operator=` | deleted — not copy-assignable |

### Guarantees and costs

- Non-copyable, and — like `lock_guard` — has no move either; its
  entire purpose is tying a fixed set of locks to one scope.
- With two or more mutexes, they are locked as if by `std::lock`:
  either all of them lock, or (on contention) the implementation backs
  off and retries, so two threads locking the same set in different
  orders cannot deadlock each other.

### Gotchas

- Same beginner trap as `lock_guard`: forgetting to name the variable
  (`std::scoped_lock(m);`) fails to hold the lock for the scope.
- `scoped_lock` can't unlock early, defer locking, or be used with a
  `condition_variable` — use `unique_lock` (locked via `std::lock` on
  several `unique_lock`s, deferred) if you need those.
- Locking the *same* mutex twice in one `scoped_lock` (e.g.
  `std::scoped_lock(m, m)`) is undefined behavior, same as any
  non-recursive double-lock.

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
    std::scoped_lock lock(from.m, to.m);  // locks both, no deadlock
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
template< class... MutexTypes >
class scoped_lock;  // (since C++17)
```

Formally: when `sizeof...(MutexTypes) == 1`, the member type
`mutex_type` names that sole mutex type. LWG 2981 (applied to C++17)
removed a redundant deduction guide from `scoped_lock<MutexTypes...>`.

Feature-test macro: `__cpp_lib_scoped_lock` — `201703L` (C++17).

### See also

- **lock_guard** — RAII wrapper for a single mutex
- **unique_lock** — movable wrapper supporting deferred/timed locking
- **lock** — the deadlock-avoidance algorithm `scoped_lock` uses internally
- **mutex** — the basic mutual-exclusion primitive

---
*Source: https://en.cppreference.com/w/cpp/thread/scoped_lock*
