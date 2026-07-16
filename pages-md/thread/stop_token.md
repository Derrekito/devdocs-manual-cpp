# std::stop_token

```cpp
class stop_token;  // (since C++20)
```

The `stop_token` class provides the means to check if a stop request has been
made or can be made, for its associated `std::stop_source` object. It is
essentially a thread-safe "view" of the associated stop-state.

The `stop_token` can also be passed to the constructor of `std::stop_callback`,
such that the callback will be invoked if the `stop_token`'s associated
`std::stop_source` is requested to stop. And `stop_token` can be passed to the
interruptible waiting functions of `std::condition_variable_any`, to interrupt
the condition variable's wait if stop is requested.

### Member functions

- **(constructor)** — constructs new `stop_token` object (public member
  function)
- **(destructor)** — destructs the `stop_token` object (public member function)
- **operator=** — assigns the `stop_token` object (public member function)

**Modifiers**

- **swap** — swaps two `stop_token` objects (public member function)

**Observers**

- **stop_requested** — checks whether the associated stop-state has been
  requested to stop (public member function)
- **stop_possible** — checks whether associated stop-state can be requested to
  stop (public member function)

### Non-member functions

- **operator==** — compares two `std::stop_token` objects (function)
- **swap(std::stop_token) (C++20)** — specializes the `std::swap` algorithm
  (function)

### Notes

A `stop_token` object is not generally constructed independently, but rather
retrieved from a `std::jthread` or `std::stop_source`. This makes it share the
same associated stop-state as the `std::jthread` or `std::stop_source`.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_jthread` | 201911L | (C++20) | Stop token and joining thread

### Example

```cpp
#include <iostream>
#include <thread>

using namespace std::literals::chrono_literals;

void f(std::stop_token stop_token, int value)
{
    while (!stop_token.stop_requested())
    {
        std::cout << value++ << ' ' << std::flush;
        std::this_thread::sleep_for(200ms);
    }
    std::cout << std::endl;
}

int main()
{
    std::jthread thread(f, 5); // prints 5 6 7 8... for approximately 3 seconds
    std::this_thread::sleep_for(3s);
    // The destructor of jthread calls request_stop() and join().
}
```

Possible output:

```text
5 6 7 8 9 10 11 12 13 14 15 16 17 18 19
```

---
*Source: https://en.cppreference.com/w/cpp/thread/stop_token*
