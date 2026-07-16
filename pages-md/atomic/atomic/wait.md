# std::atomic<T>::wait

```cpp
void wait( T old, std::memory_order order =
                      std::memory_order::seq_cst ) const noexcept;  // (1) (since C++20)
void wait( T old, std::memory_order order =
                      std::memory_order::seq_cst ) const volatile noexcept;  // (2) (since C++20)
```

Performs atomic waiting operations. Behaves as if it repeatedly performs the
following steps:

- Compare the value representation of `this->load(order)` with that of `old`. If
  those are equal, then blocks until `*this` is notified by `notify_one()` or
  `notify_all()`, or the thread is unblocked spuriously. Otherwise, returns.

These functions are guaranteed to return only if value has changed, even if
underlying implementation unblocks spuriously.

If `order` is one of `std::memory_order::release` and
`std::memory_order::acq_rel`, the behavior is undefined.

### Parameters

- **old** — the value to check the `atomic`'s object no longer contains
- **order** — memory order constraints to enforce

### Return value

(none)

### Notes

This form of change-detection is often more efficient than simple polling or
pure spinlocks.

Due to the ABA problem, transient changes from `old` to another value and back
to `old` might be missed, and not unblock.

The comparison is bitwise (similar to `std::memcmp`); no comparison operator is
used. Padding bits that never participate in an object's value representation
are ignored.

### Example

```cpp
#include <atomic>
#include <chrono>
#include <future>
#include <iostream>
#include <thread>

using namespace std::literals;

int main()
{
    std::atomic<bool> all_tasks_completed{false};
    std::atomic<unsigned> completion_count{};
    std::future<void> task_futures[16];
    std::atomic<unsigned> outstanding_task_count{16};

    // Spawn several tasks which take different amounts of
    // time, then decrement the outstanding task count.
    for (std::future<void>& task_future : task_futures)
        task_future = std::async([&]
        {
            // This sleep represents doing real work...
            std::this_thread::sleep_for(50ms);

            ++completion_count;
            --outstanding_task_count;

            // When the task count falls to zero, notify
            // the waiter (main thread in this case).
            if (outstanding_task_count.load() == 0)
            {
                all_tasks_completed = true;
                all_tasks_completed.notify_one();
            }
        });

    all_tasks_completed.wait(false);

    std::cout << "Tasks completed = " << completion_count.load() << '\n';
}
```

Output:

```text
Tasks completed = 16
```

### See also

- **notify_one (C++20)** — notifies at least one thread waiting on the atomic
  object (public member function)
- **notify_all (C++20)** — notifies all threads blocked waiting on the atomic
  object (public member function)
- **atomic_notify_one (C++20)** — notifies a thread blocked in atomic_wait
  (function template)
- **atomic_notify_all (C++20)** — notifies all threads blocked in atomic_wait
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic/wait*
