# std::future<T>::wait

```cpp
void wait() const;  // (since C++11)
```

Blocks until the result becomes available. `valid() == true` after the call.

The behavior is undefined if `valid() == false` before the call to this
function.

### Parameters

(none)

### Return value

(none)

### Exceptions

May throw implementation-defined exceptions.

### Notes

The implementations are encouraged to detect the case when `valid() == false`
before the call and throw a `std::future_error` with an error condition of
`std::future_errc::no_state`.

### Example

```cpp
#include <chrono>
#include <future>
#include <iostream>
#include <thread>

int fib(int n)
{
    if (n < 3)
        return 1;
    else
        return fib(n - 1) + fib(n - 2);
}

int main()
{
    std::future<int> f1 = std::async(std::launch::async, []() { return fib(40); });
    std::future<int> f2 = std::async(std::launch::async, []() { return fib(43); });

    std::cout << "waiting... " << std::flush;
    const auto start = std::chrono::system_clock::now();

    f1.wait();
    f2.wait();

    const auto diff = std::chrono::system_clock::now() - start;
    std::cout << std::chrono::duration<double>(diff).count() << " seconds\n";

    std::cout << "f1: " << f1.get() << '\n';
    std::cout << "f2: " << f2.get() << '\n';
}
```

Possible output:

```text
waiting... 1.61803 seconds
f1: 102334155
f2: 433494437
```

### See also

- **wait_for** — waits for the result, returns if it is not available for the
  specified timeout duration (public member function)
- **wait_until** — waits for the result, returns if it is not available until
  specified time point has been reached (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/future/wait*
