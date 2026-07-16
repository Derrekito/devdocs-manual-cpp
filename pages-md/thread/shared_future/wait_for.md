# std::shared_future<T>::wait_for

```cpp
template< class Rep, class Period >
std::future_status wait_for( const std::chrono::duration<Rep,Period>& timeout_duration ) const;  // (since C++11)
```

Waits for the result to become available. Blocks until specified
`timeout_duration` has elapsed or the result becomes available, whichever comes
first. The return value identifies the state of the result.

If the future is the result of a call to `std::async` that used lazy evaluation,
this function returns immediately without waiting.

This function may block for longer than `timeout_duration` due to scheduling or
resource contention delays.

The standard recommends that a steady clock is used to measure the duration. If
an implementation uses a system clock instead, the wait time may also be
sensitive to clock adjustments.

The behavior is undefined if `valid()` is `false` before the call to this
function.

### Parameters

- **timeout_duration** — maximum duration to block for

### Return value

- **`future_status::deferred`** — The shared state contains a deferred function
  using lazy evaluation, so the result will be computed only when explicitly
  requested
- **`future_status::ready`** — The result is ready
- **`future_status::timeout`** — The timeout has expired

### Exceptions

Any exception thrown by clock, time_point, or duration during the execution
(clocks, time points, and durations provided by the standard library never
throw).

### Notes

The implementations are encouraged to detect the case when `valid == false`
before the call and throw a `std::future_error` with an error condition of
`std::future_errc::no_state`.

### Example

```cpp
#include <chrono>
#include <future>
#include <iostream>
#include <thread>
using namespace std::chrono_literals;

int main()
{
    std::shared_future<int> future = std::async(std::launch::async, []()
    {
        std::this_thread::sleep_for(3s);
        return 8;
    });

    std::cout << "waiting...\n";
    std::future_status status;

    do
    {
        switch (status = future.wait_for(1s); status)
        {
            case std::future_status::deferred:
                std::cout << "deferred\n";
                break;
            case std::future_status::timeout:
                std::cout << "timeout\n";
                break;
            case std::future_status::ready:
                std::cout << "ready!\n";
                break;
        }
    }
    while (status != std::future_status::ready);

    std::cout << "result is " << future.get() << '\n';
}
```

Possible output:

```text
waiting...
timeout
timeout
timeout
ready!
result is 8
```

### See also

- **wait** — waits for the result to become available (public member function)
- **wait_until** — waits for the result, returns if it is not available until
  specified time point has been reached (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/shared_future/wait_for*
