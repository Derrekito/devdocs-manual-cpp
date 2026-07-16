# std::this_thread::get_id

```cpp
std::thread::id get_id() noexcept;  // (since C++11)
```

Returns the *id* of the current thread.

### Parameters

(none)

### Return value

*id* of the current thread.

### Example

```cpp
#include <chrono>
#include <iostream>
#include <syncstream>
#include <thread>
using namespace std::chrono_literals;

void foo()
{
    std::thread::id this_id = std::this_thread::get_id();

    std::osyncstream(std::cout) << "thread " << this_id << " sleeping...\n";

    std::this_thread::sleep_for(500ms);
}

int main()
{
    std::jthread t1{foo};
    std::jthread t2{foo};
}
```

Possible output:

```text
thread 140113018054400 sleeping...
thread 140113009661696 sleeping...
```

### See also

- **get_id** — returns the *id* of the thread (public member function of
  `std::thread`)

**C documentation for `thrd_current`**

---
*Source: https://en.cppreference.com/w/cpp/thread/get_id*
