# std::showbase, std::noshowbase

```cpp
std::ios_base& showbase( std::ios_base& str );  // (1)
std::ios_base& noshowbase( std::ios_base& str );  // (2)
```

1) Enables the `showbase` flag in the stream `str` as if by calling
   `str.setf(std::ios_base::showbase)`.

2) Disables the `showbase` flag in the stream `str` as if by calling
   `str.unsetf(std::ios_base::showbase)`.

This is an I/O manipulator, it may be called with an expression such as `out <<
std::showbase` for any `out` of type `std::basic_ostream` or with an expression
such as `in >> std::showbase` for any `in` of type `std::basic_istream`.

The `showbase` flag affects the behavior of integer output (see
`std::num_put::put`), monetary input (see `std::money_get::get`) and monetary
output (see `std::money_put::put`).

### Parameters

- **str** — reference to I/O stream

### Return value

`str` (reference to the stream after manipulation).

### Notes

As specifed in `std::num_put::put`, the showbase flag in integer output acts
like the # format specifier in `std::printf`, which means the numeric base
prefix is *not* added when outputting the value zero.

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <locale>
#include <sstream>

int main()
{
    // showbase affects the output of octals and hexadecimals
    std::cout << std::hex
              << "showbase: " << std::showbase << 42 << '\n'
              << "noshowbase: " << std::noshowbase << 42 << '\n';

    // and both input and output of monetary values
    std::locale::global(std::locale("en_US.utf8"));
    long double val = 0;
    std::istringstream is("3.14");
    is >> std::showbase >> std::get_money(val);
    std::cout << "With showbase, parsing 3.14 as money gives " << val << '\n';
    is.seekg(0);
    is >> std::noshowbase >> std::get_money(val);
    std::cout << "Without showbase, parsing 3.14 as money gives " << val << '\n';
}
```

Output:

```text
showbase: 0x2a
noshowbase: 2a
With showbase, parsing 3.14 as money gives 0
Without showbase, parsing 3.14 as money gives 314
```

### See also

- **resetiosflags** — clears the specified ios_base flags (function)
- **setiosflags** — sets the specified `ios_base` flags (function)

---
*Source: https://en.cppreference.com/w/cpp/io/manip/showbase*
