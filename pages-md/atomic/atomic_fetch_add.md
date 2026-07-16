# std::atomic_fetch_add, std::atomic_fetch_add_explicit

```cpp
template< class T >
T atomic_fetch_add( std::atomic<T>* obj,
                    typename std::atomic<T>::difference_type arg ) noexcept;  // (1) (since C++11)
template< class T >
T atomic_fetch_add( volatile std::atomic<T>* obj,
                    typename std::atomic<T>::difference_type arg ) noexcept;  // (2) (since C++11)
template< class T >
T atomic_fetch_add_explicit( std::atomic<T>* obj,
                             typename std::atomic<T>::difference_type arg,
                             std::memory_order order ) noexcept;  // (3) (since C++11)
template< class T >
T atomic_fetch_add_explicit( volatile std::atomic<T>* obj,
                             typename std::atomic<T>::difference_type arg,
                             std::memory_order order ) noexcept;  // (4) (since C++11)
```

Performs atomic addition. Atomically adds `arg` to the value pointed to by `obj`
and returns the value `obj` held previously. The operation is performed as if
the following was executed:

1,2) `obj->fetch_add(arg)`

3,4) `obj->fetch_add(arg, order)`

If `std::atomic<T>` has no `fetch_add` member (this member is only provided for
integral, floating-point(since C++20) and pointer types except bool), the
program is ill-formed.

### Parameters

- **obj** — pointer to the atomic object to modify
- **arg** — the value to add to the value stored in the atomic object
- **order** — the memory synchronization ordering

### Return value

The value immediately preceding the effects of this function in the modification
order of `*obj`.

### Example

Single-writer/multiple-reader lock can be made with `std::atomic_fetch_add`.
Note that this simplistic implementation is not lockout-free.

```cpp
#include <atomic>
#include <chrono>
#include <iostream>
#include <string>
#include <thread>
#include <vector>

using namespace std::chrono_literals;

// meaning of cnt:
//  5: readers and writer are in race. There are no active readers or writers.
//  4...0: there are 1...5 active readers, The writer is blocked.
// -1: writer won the race and readers are blocked.

const int N = 5; // four concurrent readers are allowed
std::atomic<int> cnt(N);

std::vector<int> data;

void reader(int id)
{
    for (;;)
    {
        // lock
        while (std::atomic_fetch_sub(&cnt, 1) <= 0)
            std::atomic_fetch_add(&cnt, 1);

        // read
        if (!data.empty())
            std::cout << ("reader " + std::to_string(id) +
                          " sees " + std::to_string(*data.rbegin()) + '\n');
        if (data.size() == 25)
            break;

        // unlock
        std::atomic_fetch_add(&cnt, 1);

        // pause
        std::this_thread::sleep_for(1ms);
    }
}

void writer()
{
    for (int n = 0; n < 25; ++n)
    {
        // lock
        while (std::atomic_fetch_sub(&cnt, N + 1) != N)
            std::atomic_fetch_add(&cnt, N + 1);

        // write
        data.push_back(n);
        std::cout << "writer pushed back " << n << '\n';

        // unlock
        std::atomic_fetch_add(&cnt, N + 1);

        // pause
        std::this_thread::sleep_for(1ms);
    }
}

int main()
{
    std::vector<std::thread> v;
    for (int n = 0; n < N; ++n)
        v.emplace_back(reader, n);
    v.emplace_back(writer);

    for (auto& t : v)
        t.join();
}
```

Output:

```text
writer pushed back 0
reader 2 sees 0
reader 3 sees 0
reader 1 sees 0
<...>
reader 2 sees 24
reader 4 sees 24
reader 1 sees 24
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P0558R1 | C++11 | exact type match was required because `T` was deduced from
      multiple arguments | `T` is only deduced from `obj`

### See also

- **fetch_add** — atomically adds the argument to the value stored in the atomic
  object and obtains the value held previously (public member function of
  `std::atomic<T>`)
- **atomic_fetch_subatomic_fetch_sub_explicit (C++11)(C++11)** — subtracts a
  non-atomic value from an atomic object and obtains the previous value of the
  atomic (function template)

**C documentation for `atomic_fetch_add, atomic_fetch_add_explicit`**

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic_fetch_add*
