# std::atomic<T>::exchange

```cpp
T exchange( T desired, std::memory_order order =
                           std::memory_order_seq_cst ) noexcept;  // (1) (since C++11)
T exchange( T desired, std::memory_order order =
                           std::memory_order_seq_cst ) volatile noexcept;  // (2) (since C++11)
```

Atomically replaces the underlying value with `desired` (a read-modify-write
operation). Memory is affected according to the value of `order`.

It is deprecated if `std::atomic<T>::is_always_lock_free` is `false` and
overload (2) participates in overload resolution.
*(since C++20)*

### Parameters

- **desired** — value to assign
- **order** — memory order constraints to enforce

### Return value

The value of the atomic variable before the call.

### Example

```cpp
#include <algorithm>
#include <atomic>
#include <cstddef>
#include <iostream>
#include <syncstream>
#include <thread>
#include <vector>

int main()
{
    constexpr int thread_count{5};
    constexpr int sum{5};

    std::atomic<int> atom{0};
    std::atomic<int> counter{0};

    auto increment_to_sum = [&](const int id)
    {
        for (int next = 0; next < sum;)
        {
            // each thread is writing a value from its own knowledge
            const int current = atom.exchange(next);
            counter++;
            // sync writing to prevent from interrupting by other threads
            std::osyncstream(std::cout)
                << "Thread #" << id << " (id=" << std::this_thread::get_id()
                << ") wrote " << next << " replacing the old value "
                << current << ".\n";
            next = std::max(current, next) + 1;
        }
    };

    std::vector<std::thread> v;
    for (std::size_t i = 0; i < thread_count; ++i)
        v.emplace_back(increment_to_sum, i);

    for (auto& tr : v)
        tr.join();

    std::cout << thread_count << " threads take "
              << counter << " times in total to "
              << "increment 0 to " << sum << ".\n";
}
```

Possible output:

```text
Thread #1 (id=139722332333824) wrote 0 replacing the old value 0.
Thread #2 (id=139722323941120) wrote 0 replacing the old value 0.
Thread #1 (id=139722332333824) wrote 1 replacing the old value 0.
Thread #1 (id=139722332333824) wrote 2 replacing the old value 1.
Thread #1 (id=139722332333824) wrote 3 replacing the old value 2.
Thread #1 (id=139722332333824) wrote 4 replacing the old value 3.
Thread #0 (id=139722340726528) wrote 0 replacing the old value 0.
Thread #3 (id=139722315548416) wrote 0 replacing the old value 0.
Thread #3 (id=139722315548416) wrote 1 replacing the old value 4.
Thread #0 (id=139722340726528) wrote 1 replacing the old value 1.
Thread #4 (id=139722307155712) wrote 0 replacing the old value 1.
Thread #4 (id=139722307155712) wrote 2 replacing the old value 2.
Thread #4 (id=139722307155712) wrote 3 replacing the old value 2.
Thread #4 (id=139722307155712) wrote 4 replacing the old value 3.
Thread #2 (id=139722323941120) wrote 1 replacing the old value 0.
Thread #0 (id=139722340726528) wrote 2 replacing the old value 1.
Thread #2 (id=139722323941120) wrote 2 replacing the old value 4.
Thread #0 (id=139722340726528) wrote 3 replacing the old value 2.
Thread #0 (id=139722340726528) wrote 4 replacing the old value 3.
5 threads take 19 times in total to increment 0 to 5.
```

### See also

- **atomic_exchangeatomic_exchange_explicit (C++11)(C++11)** — atomically
  replaces the value of the atomic object with non-atomic argument and returns
  the old value of the atomic (function template)
- **exchange (C++14)** — replaces the argument with a new value and returns its
  previous value (function template)

---
*Source: https://en.cppreference.com/w/cpp/atomic/atomic/exchange*
