# std::atomic_flag_test_and_set, std::atomic_flag_test_and_set_explicit

```cpp
bool atomic_flag_test_and_set( volatile std::atomic_flag* obj ) noexcept;  // (1) (since C++11)
bool atomic_flag_test_and_set( std::atomic_flag* obj ) noexcept;  // (2) (since C++11)
bool atomic_flag_test_and_set_explicit( volatile std::atomic_flag* obj,
                                        std::memory_order order ) noexcept;  // (3) (since C++11)
bool atomic_flag_test_and_set_explicit( std::atomic_flag* obj,
                                        std::memory_order order ) noexcept;  // (4) (since C++11)
```

Atomically changes the state of a `std::atomic_flag` pointed to by `obj` to set
(`true`) and returns the value it held before.

1,2) The memory synchronization order is `std::memory_order_seq_cst`.

3,4) The memory synchronization order is `order`.

### Parameters

- **obj** — pointer to `std::atomic_flag` to access
- **order** — the memory synchronization order

### Return value

The value previously held by the flag pointed to by `obj`.

### Notes

`std::atomic_flag_test_and_set` and `std::atomic_flag_test_and_set_explicit` can
be implemented as `obj->test_and_set()` and `obj->test_and_set(order)`
respectively.

### Example

A spinlock mutex can be implemented in userspace using an `std::atomic_flag`.

```cpp
#include <atomic>
#include <iostream>
#include <thread>
#include <vector>

std::atomic_flag lock = ATOMIC_FLAG_INIT;

void f(int n)
{
    for (int cnt = 0; cnt < 100; ++cnt)
    {
        while (std::atomic_flag_test_and_set_explicit(&lock, std::memory_order_acquire))
            ; // spin until the lock is acquired
        std::cout << "Output from thread " << n << '\n';
        std::atomic_flag_clear_explicit(&lock, std::memory_order_release);
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

Output:

```text
Output from thread 2
Output from thread 6
Output from thread 7
...<exactly 1000 lines>...
```

### See also

- **atomic_flag (C++11)** — the lock-free boolean atomic type (class)
- **atomic_flag_clearatomic_flag_clear_explicit (C++11)(C++11)** — atomically
  sets the value of the flag to `false` (function)
- **memory_order (C++11)** — defines memory ordering constraints for the given
  atomic operation (enum)

**C documentation for `atomic_flag_test_and_set, atomic_flag_test_and_set_explicit`**

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_flag_test_and_set*
