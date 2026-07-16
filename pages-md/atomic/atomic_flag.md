# std::atomic_flag

```cpp
class atomic_flag;  // (since C++11)
```

`std::atomic_flag` is an atomic boolean type. Unlike all specializations of
`std::atomic`, it is guaranteed to be lock-free. Unlike `std::atomic<bool>`,
`std::atomic_flag` does not provide load or store operations.

### Member functions

- **(constructor)** — constructs an atomic_flag (public member function)
- **operator= [deleted]** — the assignment operator (deleted) (public member
  function)
- **clear** — atomically sets flag to `false` (public member function)
- **test_and_set** — atomically sets the flag to `true` and obtains its previous
  value (public member function)
- **test (C++20)** — atomically returns the value of the flag (public member
  function)
- **wait (C++20)** — blocks the thread until notified and the atomic value
  changes (public member function)
- **notify_one (C++20)** — notifies at least one thread waiting on the atomic
  object (public member function)
- **notify_all (C++20)** — notifies all threads blocked waiting on the atomic
  object (public member function)

### Example

A spinlock mutex demo can be implemented in userspace using an `atomic_flag`. Do
note that spinlock mutexes are extremely dubious in practice.

```cpp
#include <atomic>
#include <iostream>
#include <thread>
#include <vector>

std::atomic_flag lock = ATOMIC_FLAG_INIT;

void f(int n)
{
    for (int cnt = 0; cnt < 40; ++cnt)
    {
        while (lock.test_and_set(std::memory_order_acquire)) // acquire lock
        {
            // Since C++20, it is possible to update atomic_flag's
            // value only when there is a chance to acquire the lock.
            // See also: https://stackoverflow.com/questions/62318642
        #if defined(__cpp_lib_atomic_flag_test)
            while (lock.test(std::memory_order_relaxed)) // test lock
        #endif
                ; // spin
        }
        static int out{};
        std::cout << n << ((++out % 40) == 0 ? '\n' : ' ');
        lock.clear(std::memory_order_release); // release lock
    }
}

int main()
{
    std::vector<std::thread> v;
    for (int n = 0; n < 10; ++n)
        v.emplace_back(f, n);
    for (auto& t : v)
        t.join();
}
```

Possible output:

```text
0 1 1 2 0 1 3 2 3 2 0 1 2 3 2 3 0 1 3 2 0 1 2 3 2 3 0 3 2 3 2 3 2 3 1 2 3 0 1 3
2 3 2 0 1 2 3 0 1 2 3 2 0 1 2 3 0 1 2 3 2 3 2 3 2 0 1 2 3 2 3 0 1 3 2 3 0 2 1 1
1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 0 3 2 0 2 3 2 3 2 3 2 3 2 3 0 3
2 3 0 3 0 3 2 3 0 3 2 3 2 3 0 2 3 0 3 2 0 2 2 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4 4
5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5 5
6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6 6
7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7 7
8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8 8
9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9 9
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3756 | C++23 | unclear whether it's signal-safe | it's signal-safe

### See also

- **atomic_flag_test_and_setatomic_flag_test_and_set_explicit (C++11)(C++11)** —
  atomically sets the flag to `true` and returns its previous value (function)
- **atomic_flag_clearatomic_flag_clear_explicit (C++11)(C++11)** — atomically
  sets the value of the flag to `false` (function)
- **atomic_flag_waitatomic_flag_wait_explicit (C++20)(C++20)** — blocks the
  thread until notified and the flag changes (function)
- **atomic_flag_notify_one (C++20)** — notifies a thread blocked in
  atomic_flag_wait (function)
- **atomic_flag_notify_all (C++20)** — notifies all threads blocked in
  atomic_flag_wait (function)
- **ATOMIC_FLAG_INIT (C++11)** — initializes an `std::atomic_flag` to `false`
  (macro constant)

**C documentation for `atomic_flag`**

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_flag*
