# std::basic_stacktrace<Allocator>::rend, std::basic_stacktrace<Allocator>::crend

```cpp
const_reverse_iterator rend()  const noexcept;  // (1) (since C++23)
const_reverse_iterator crend() const noexcept;  // (2) (since C++23)
```

Returns a reverse iterator pointing past the last entry of the reversed
`basic_stacktrace`. It corresponds to the iterator preceding the first entry of
the original `basic_stacktrace`. This iterator acts as a placeholder, attempting
to dereference it results in undefined behavior.

### Parameters

(none)

### Return value

The end iterator of the reversed `basic_stacktrace`.

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

- **rbegincrbegin** — returns a reverse iterator to the beginning (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/utility/basic_stacktrace/rend*
