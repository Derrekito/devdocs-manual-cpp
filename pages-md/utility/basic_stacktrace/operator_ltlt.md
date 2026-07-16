# std::operator<<(std::basic_stacktrace)

```cpp
template< class Allocator >
std::ostream& operator<<( std::ostream& os, const std::basic_stacktrace<Allocator>& st );  // (since C++23)
```

Inserts the description of `st` into the output stream `os`. Equivalent to
`return os << std::to_string(st);`.

### Parameters

- **os** — an output stream
- **st** — a `basic_stacktrace` whose description is to be inserted

### Return value

`os`.

### Exceptions

May throw implementation-defined exceptions.

### Example

```cpp
#include <stacktrace>
#include <iostream>

int main()
{
    std::cout << "The stacktrace obtained in the main function:\n";
    std::cout << std::stacktrace::current() << '\n';
    []{
        std::cout << "The stacktrace obtained in a nested lambda:\n";
        std::cout << std::stacktrace::current() << '\n';
    }();
}
```

Possible output:

```text
The stacktrace obtained in the main function:
 0# 0x0000000000402E7B in ./prog.exe
 1# __libc_start_main in /lib/x86_64-linux-gnu/libc.so.6
 2# 0x0000000000402CD9 in ./prog.exe

The stacktrace obtained in a nested lambda:
 0# 0x0000000000402DDA in ./prog.exe
 1# 0x0000000000402EB2 in ./prog.exe
 2# __libc_start_main in /lib/x86_64-linux-gnu/libc.so.6
 3# 0x0000000000402CD9 in ./prog.exe
```

### See also

- **operator<< (C++23)** — performs stream output of `stacktrace_entry`
  (function template)

---
*Source: https://en.cppreference.com/w/cpp/utility/basic_stacktrace/operator_ltlt*
