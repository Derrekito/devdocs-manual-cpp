# std::this_thread::sleep_for

```cpp
template< class Rep, class Period >
void sleep_for( const std::chrono::duration<Rep, Period>& sleep_duration );  // (since C++11)
```

Blocks the execution of the current thread for *at least* the specified
`sleep_duration`.

This function may block for longer than `sleep_duration` due to scheduling or
resource contention delays.

The standard recommends that a steady clock is used to measure the duration. If
an implementation uses a system clock instead, the wait time may also be
sensitive to clock adjustments.

### Parameters

- **sleep_duration** — time duration to sleep

### Return value

(none)

### Exceptions

Any exception thrown by `clock`, `time_point`, or `duration` during the
execution (clocks, time points, and durations provided by the standard library
never throw).

### Example

```cpp
#include <chrono>
#include <iostream>
#include <thread>

int main()
{
    using namespace std::chrono_literals;

    std::cout << "Hello waiter\n" << std::flush;

    const auto start = std::chrono::high_resolution_clock::now();
    std::this_thread::sleep_for(2000ms);
    const auto end = std::chrono::high_resolution_clock::now();
    const std::chrono::duration<double, std::milli> elapsed = end - start;

    std::cout << "Waited " << elapsed << '\n';
}
```

Possible output:

```text
Hello waiter
Waited 2000.13 ms
```

### See also

- **sleep_until (C++11)** — stops the execution of the current thread until a
  specified time point (function)

---
*Source: https://en.cppreference.com/w/cpp/thread/sleep_for*
