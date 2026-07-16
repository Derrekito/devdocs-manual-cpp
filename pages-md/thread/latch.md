# std::latch

```cpp
class latch;  // (since C++20)
```

The `latch` class is a downward counter of type `std::ptrdiff_t` which can be
used to synchronize threads. The value of the counter is initialized on
creation. Threads may block on the latch until the counter is decremented to
zero. There is no possibility to increase or reset the counter, which makes the
latch a single-use barrier.

Concurrent invocations of the member functions of `std::latch`, except for the
destructor, do not introduce data races.

Unlike `std::barrier`, `std::latch` can be decremented by a participating thread
more than once.

### Member functions

- **(constructor)** — constructs a `latch` (public member function)
- **(destructor)** — destroys the `latch` (public member function)
- **operator= [deleted]** — `latch` is not assignable (public member function)
- **count_down** — decrements the counter in a non-blocking manner (public
  member function)
- **try_wait** — tests if the internal counter equals zero (public member
  function)
- **wait** — blocks until the counter reaches zero (public member function)
- **arrive_and_wait** — decrements the counter and blocks until it reaches zero
  (public member function)

**Constants**

- **max [static]** — the maximum value of counter supported by the
  implementation (public static member function)

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_latch` | 201907L | (C++20) | `std::latch`

### Example

```cpp
#include <functional>
#include <iostream>
#include <latch>
#include <string>
#include <thread>

struct Job
{
    const std::string name;
    std::string product{"not worked"};
    std::thread action{};
};

int main()
{
    Job jobs[]{{"Annika"}, {"Buru"}, {"Chuck"}};

    std::latch work_done{std::size(jobs)};
    std::latch start_clean_up{1};

    auto work = [&](Job& my_job)
    {
        my_job.product = my_job.name + " worked";
        work_done.count_down();
        start_clean_up.wait();
        my_job.product = my_job.name + " cleaned";
    };

    std::cout << "Work is starting... ";
    for (auto& job : jobs)
        job.action = std::thread{work, std::ref(job)};

    work_done.wait();
    std::cout << "done:\n";
    for (auto const& job : jobs)
        std::cout << "  " << job.product << '\n';

    std::cout << "Workers are cleaning up... ";
    start_clean_up.count_down();
    for (auto& job : jobs)
        job.action.join();

    std::cout << "done:\n";
    for (auto const& job : jobs)
        std::cout << "  " << job.product << '\n';
}
```

Output:

```text
Work is starting... done:
  Annika worked
  Buru worked
  Chuck worked
Workers are cleaning up... done:
  Annika cleaned
  Buru cleaned
  Chuck cleaned
```

### See also

- **barrier (C++20)** — reusable thread barrier (class template)

---
*Source: https://en.cppreference.com/w/cpp/thread/latch*
