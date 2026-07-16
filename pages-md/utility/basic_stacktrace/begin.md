# std::basic_stacktrace<Allocator>::begin, std::basic_stacktrace<Allocator>::cbegin

```cpp
const_iterator begin()  const noexcept;  // (1) (since C++23)
const_iterator cbegin() const noexcept;  // (2) (since C++23)
```

Returns an iterator to the first entry of the `basic_stacktrace`.

If the `basic_stacktrace` is empty, the returned iterator is equal to `end()`.

### Parameters

(none)

### Return value

Iterator to the first entry.

### Complexity

Constant.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <stacktrace>

int main()
{
    auto trace       = std::stacktrace::current();
    auto empty_trace = std::stacktrace{};

    // Print stacktrace.
    std::for_each(trace.begin(), trace.end(),
                  [](const auto& f) { std::cout << f << '\n'; });

    if (empty_trace.begin() == empty_trace.end())
        std::cout << "stacktrace 'empty_trace' is indeed empty.\n";
}
```

Possible output:

```text
0x0000000000402BA8 in ./prog.exe
__libc_start_main in /lib/x86_64-linux-gnu/libc.so.6
0x0000000000402A29 in ./prog.exe
stacktrace 'empty_trace' is indeed empty.
```

### See also

- **endcend** — returns an iterator to the end (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/basic_stacktrace/begin*
