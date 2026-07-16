# std::basic_stacktrace<Allocator>::rbegin, std::basic_stacktrace<Allocator>::crbegin

```cpp
const_reverse_iterator rbegin()  const noexcept;  // (1) (since C++23)
const_reverse_iterator crbegin() const noexcept;  // (2) (since C++23)
```

Returns a reverse iterator to the first entry of the reversed
`basic_stacktrace`. It corresponds to the last entry of the original
`basic_stacktrace`. If the `basic_stacktrace` is empty, the returned iterator is
equal to `rend()`.

### Parameters

(none)

### Return value

Reverse iterator to the first entry.

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
    std::for_each(trace.rbegin(), trace.rend(),
                  [](const auto& f) { std::cout << f << '\n'; });

    if (empty_trace.rbegin() == empty_trace.rend())
        std::cout << "stacktrace 'empty_trace' is indeed empty.\n";
}
```

Possible output:

```text
0x0000000000402A29 in ./prog.exe
__libc_start_main in /lib/x86_64-linux-gnu/libc.so.6
0x0000000000402BA5 in ./prog.exe
stacktrace 'empty_trace' is indeed empty.
```

### See also

- **rendcrend** — returns a reverse iterator to the end (public member function)

---
*Source: https://en.cppreference.com/w/cpp/utility/basic_stacktrace/rbegin*
