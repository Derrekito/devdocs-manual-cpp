# std::this_thread::sleep_until

```cpp
template< class Clock, class Duration >
void sleep_until( const std::chrono::time_point<Clock, Duration>& sleep_time );  // (since C++11)
```

Blocks the execution of the current thread until specified `sleep_time` has been
reached.

`Clock` must meet the Clock requirements. The program is ill-formed if
`std::chrono::is_clock_v<Clock>` is `false`.(since C++20)

The standard recommends that the clock tied to `sleep_time` be used, in which
case adjustments of the clock may be taken into account. Thus, the duration of
the block might be more or less than `sleep_time - Clock::now()` at the time of
the call, depending on the direction of the adjustment and whether it is honored
by the implementation. The function also may block until after `sleep_time` has
been reached due to process scheduling or resource contention delays.

### Parameters

- **sleep_time** — time to block until

### Return value

(none)

### Exceptions

Any exception thrown by `Clock` or `Duration` (clocks and durations provided by
the standard library never throw).

### Example

```cpp
#include <chrono>
#include <iostream>
#include <thread>

auto now() { return std::chrono::steady_clock::now(); }

auto awake_time()
{
    using std::chrono::operator""ms;
    return now() + 2000ms;
}

int main()
{
    std::cout << "Hello, waiter...\n" << std::flush;
    const auto start{now()};
    std::this_thread::sleep_until(awake_time());
    std::chrono::duration<double, std::milli> elapsed{now() - start};
    std::cout << "Waited " << elapsed.count() << " ms\n";
}
```

Possible output:

```text
Hello, waiter...
Waited 2000.17 ms
```

### See also

- **sleep_for (C++11)** — stops the execution of the current thread for a
  specified time duration (function)

**C documentation for `thrd_sleep`**

---
*Source: https://en.cppreference.com/w/cpp/thread/sleep_until*
