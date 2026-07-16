# std::uppercase, std::nouppercase

```cpp
std::ios_base& uppercase( std::ios_base& str );  // (1)
std::ios_base& nouppercase( std::ios_base& str );  // (2)
```

Enables the use of uppercase characters in floating-point and hexadecimal
integer output. Has no effect on input.

1) Enables the `uppercase` flag in the stream `str` as if by calling
   `str.setf(std::ios_base::uppercase)`.

2) Disables the `uppercase` flag in the stream `str` as if by calling
   `str.unsetf(std::ios_base::uppercase)`.

This is an I/O manipulator, it may be called with an expression such as `out <<
std::uppercase` for any `out` of type `std::basic_ostream` or with an expression
such as `in >> std::uppercase` for any `in` of type `std::basic_istream`.

### Parameters

- **str** — reference to I/O stream

### Return value

`str` (reference to the stream after manipulation).

### Example

```cpp
#include <iostream>

int main()
{
    std::cout << std::hex << std::showbase
              << "0x2a with uppercase: " << std::uppercase << 0x2a << '\n'
              << "0x2a with nouppercase: " << std::nouppercase << 0x2a << '\n'
              << "1e-10 with uppercase: " << std::uppercase << 1e-10 << '\n'
              << "1e-10 with nouppercase: " << std::nouppercase << 1e-10 << '\n';
}
```

Output:

```text
0x2a with uppercase: 0X2A
0x2a with nouppercase: 0x2a
1e-10 with uppercase: 1E-10
1e-10 with nouppercase: 1e-10
```

### See also

- **resetiosflags** — clears the specified ios_base flags (function)
- **setiosflags** — sets the specified `ios_base` flags (function)

---
*Source: https://en.cppreference.com/w/cpp/io/manip/uppercase*
