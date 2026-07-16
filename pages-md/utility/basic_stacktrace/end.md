# std::basic_stacktrace<Allocator>::end, std::basic_stacktrace<Allocator>::cend

```cpp
const_iterator end()  const noexcept;  // (1) (since C++23)
const_iterator cend() const noexcept;  // (2) (since C++23)
```

Returns the iterator pointing past the last entry of the `basic_stacktrace`.

This iterator acts as a placeholder; attempting to dereference it results in
undefined behavior.

### Parameters

(none)

### Return value

The end iterator.

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

- **begincbegin** — returns an iterator to the beginning (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/utility/basic_stacktrace/end*
