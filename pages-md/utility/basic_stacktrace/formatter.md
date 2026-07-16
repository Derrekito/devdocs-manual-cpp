# std::formatter<std::basic_stacktrace>

```cpp
template< class Allocator >
struct formatter<std::basic_stacktrace<Allocator>>;  // (since C++23)
```

The template specialization of `std::formatter` for
`std::basic_stacktrace<Allocator>` allows users to convert a stacktrace object
to string using formatting functions such as `std::format`.

No format specifier is allowed.

A stacktrace object `s` is formatted as if by copying `std::to_string(s)` to the
output.

### Example

```cpp
#include <format>
#include <iostream>
#include <stacktrace>

int main()
{
    auto trace = std::stacktrace::current();
    std::cout << std::format("{}\n", trace);
}
```

Possible output:

```text
0# 0x0000000000402D97 in ./prog.exe
1# __libc_start_main in /lib/x86_64-linux-gnu/libc.so.6
2# 0x0000000000402CA9 in ./prog.exe
```

### See also

- **formatter (C++20)** — defines formatting rules for a given type (class
  template)
- **print (C++23)** — prints to `stdout` or a file stream using formatted
  representation of the arguments (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/basic_stacktrace/formatter*
