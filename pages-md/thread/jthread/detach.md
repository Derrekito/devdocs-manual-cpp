# std::jthread::detach

```cpp
void detach();  // (since C++20)
```

Separates the thread of execution from the jthread object, allowing execution to
continue independently. Any allocated resources will be freed once the thread
exits.

After calling `detach` `*this` no longer owns any thread.

### Parameters

(none)

### Return value

(none)

### Postconditions

`joinable` is `false`.

### Exceptions

`std::system_error` if `joinable() == false` or an error occurs.

### Example

```cpp
#include <chrono>
#include <iostream>
#include <thread>

void independentThread()
{
    std::cout << "Starting concurrent thread.\n";
    std::this_thread::sleep_for(std::chrono::seconds(2));
    std::cout << "Exiting concurrent thread.\n";
}

void threadCaller()
{
    std::cout << "Starting thread caller.\n";
    std::jthread t(independentThread);
    t.detach();
    std::this_thread::sleep_for(std::chrono::seconds(1));
    std::cout << "Exiting thread caller.\n";
}

int main()
{
    threadCaller();
    std::this_thread::sleep_for(std::chrono::seconds(5));
}
```

Possible output:

```text
Starting thread caller.
Starting concurrent thread.
Exiting thread caller.
Exiting concurrent thread.
```

### References

- C++23 standard (ISO/IEC 14882:2023):
- C++20 standard (ISO/IEC 14882:2020):

### See also

- **join** — waits for the thread to finish its execution (public member
  function)
- **joinable** — checks whether the thread is joinable, i.e. potentially running
  in parallel context (public member function)

**C documentation for `thrd_detach`**

---
*Source: https://en.cppreference.com/w/cpp/thread/jthread/detach*
