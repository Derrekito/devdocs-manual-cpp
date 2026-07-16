# std::operator<<(std::stacktrace_entry)

```cpp
std::ostream& operator<<( std::ostream& os, const std::stacktrace_entry& f );  // (since C++23)
```

Inserts the description of `f` into the output stream `os`. Equivalent to
`return os << std::to_string(f);`.

### Parameters

- **os** — an output stream
- **f** — a `stacktrace_entry` whose description is to be inserted

### Return value

`os`

### Exceptions

May throw implementation-defined exceptions.

### Example

```cpp
#include <iostream>
#include <stacktrace>

int main()
{
    for (const auto& f : std::stacktrace::current())
        std::cout << f << '\n';
}
```

Possible output:

```text
0x0000000000402AA7 in ./prog.exe
__libc_start_main in /lib/x86_64-linux-gnu/libc.so.6
0x00000000004029B9 in ./prog.exe
```

### See also

- **operator<< (C++23)** — performs stream output of `basic_stracktrace`
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/stacktrace_entry/operator_ltlt*
