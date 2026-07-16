# std::timed_mutex::try_lock_for

```cpp
template< class Rep, class Period >
bool try_lock_for( const std::chrono::duration<Rep, Period>& timeout_duration );  // (since C++11)
```

Tries to lock the mutex. Blocks until the specified duration `timeout_duration`
has elapsed (timeout) or the lock is acquired (owns the mutex), whichever comes
first. On successful lock acquisition returns `true`, otherwise returns `false`.

If `timeout_duration` is less or equal `timeout_duration.zero()`, the function
behaves like `try_lock()`.

This function may block for longer than `timeout_duration` due to scheduling or
resource contention delays.

The standard recommends that a `std::steady_clock` is used to measure the
duration. If an implementation uses a `std::system_clock` instead, the wait time
may also be sensitive to clock adjustments.

As with `try_lock()`, this function is allowed to fail spuriously and return
`false` even if the mutex was not locked by any other thread at some point
during `timeout_duration`.

Prior `unlock()` operation on the same mutex *synchronizes-with* (as defined in
`std::memory_order`) this operation if it returns `true`.

If `try_lock_for` is called by a thread that already owns the mutex, the
behavior is undefined.

### Parameters

- **timeout_duration** — minimum duration to block for

### Return value

`true` if the lock was acquired successfully, otherwise `false`.

### Exceptions

Any exception thrown by `timeout_duration` (durations provided by the standard
library never throw).

### Example

```cpp
#include <chrono>
#include <iostream>
#include <mutex>
#include <sstream>
#include <thread>
#include <vector>

using namespace std::chrono_literals;

std::mutex cout_mutex; // control access to std::cout
std::timed_mutex mutex;

void job(int id)
{
    std::ostringstream stream;

    for (int i = 0; i < 3; ++i)
    {
        if (mutex.try_lock_for(100ms))
        {
            stream << "success ";
            std::this_thread::sleep_for(100ms);
            mutex.unlock();
        }
        else
            stream << "failed ";

        std::this_thread::sleep_for(100ms);
    }

    std::lock_guard<std::mutex> lock{cout_mutex};
    std::cout << '[' << id << "] " << stream.str() << '\n';
}

int main()
{
    std::vector<std::thread> threads;
    for (int i{0}; i < 4; ++i)
        threads.emplace_back(job, i);

    for (auto& th : threads)
        th.join();
}
```

Possible output:

```text
[0] failed failed failed
[3] failed failed success
[2] failed success failed
[1] success failed success
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2093 | C++11 | `try_lock_for` threw nothing | throws timeout-related
      exceptions

### See also

- **lock** — locks the mutex, blocks if the mutex is not available (public
  member function)
- **try_lock** — tries to lock the mutex, returns if the mutex is not available
  (public member function)
- **try_lock_until** — tries to lock the mutex, returns if the mutex has been
  unavailable until specified time point has been reached (public member
  function)
- **unlock** — unlocks the mutex (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/timed_mutex/try_lock_for*
