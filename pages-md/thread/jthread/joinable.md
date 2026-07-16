# std::jthread::joinable

```cpp
[[nodiscard]] bool joinable() const noexcept;  // (since C++20)
```

Checks if the `std::jthread` object identifies an active thread of execution.
Specifically, returns `true` if `get_id() != std::jthread::id()`. So a default
constructed jthread is not joinable.

A thread that has finished executing code, but has not yet been joined is still
considered an active thread of execution and is therefore joinable.

### Parameters

(none)

### Return value

`true` if the jthread object identifies an active thread of execution, `false`
otherwise.

### Example

```cpp
#include <chrono>
#include <iostream>
#include <thread>
using namespace std::chrono_literals;

void foo()
{
    std::this_thread::sleep_for(500ms);
}

int main()
{
    std::cout << std::boolalpha;

    std::jthread t;
    std::cout << "before starting, joinable: " << t.joinable() << '\n';

    t = std::jthread{foo};
    std::cout << "after starting, joinable: " << t.joinable() << '\n';

    t.join();
    std::cout << "after joining, joinable: " << t.joinable() << '\n';

    t = std::jthread{foo};
    t.detach();
    std::cout << "after detaching, joinable: " << t.joinable() << '\n';

}
```

Output:

```text
before starting, joinable: false
after starting, joinable: true
after joining, joinable: false
after detaching, joinable: false
```

### References

- C++23 standard (ISO/IEC 14882:2023):
- C++20 standard (ISO/IEC 14882:2020):

### See also

- **get_id** — returns the *id* of the thread (public member function)
- **join** — waits for the thread to finish its execution (public member
  function)
- **detach** — permits the thread to execute independently from the thread
  handle (public member function)

---
*Source: https://en.cppreference.com/w/cpp/thread/jthread/joinable*
