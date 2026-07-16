# std::thread::hardware_concurrency

```cpp
static unsigned int hardware_concurrency() noexcept;  // (since C++11)
```

Returns the number of concurrent threads supported by the implementation. The
value should be considered only a hint.

### Parameters

(none)

### Return value

Number of concurrent threads supported. If the value is not well defined or not
computable, returns `​0​`.

### Example

```cpp
#include <iostream>
#include <thread>

int main()
{
    unsigned int n = std::thread::hardware_concurrency();
    std::cout << n << " concurrent threads are supported.\n";
}
```

Possible output:

```text
4 concurrent threads are supported.
```

### See also

-
  **hardware_destructive_interference_sizehardware_constructive_interference_size
  (C++17)** — min offset to avoid false sharing max offset to promote true
  sharing (constant)

---
*Source: https://en.cppreference.com/w/cpp/thread/thread/hardware_concurrency*
