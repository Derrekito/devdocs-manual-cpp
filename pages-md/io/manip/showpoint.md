# std::showpoint, std::noshowpoint

```cpp
std::ios_base& showpoint( std::ios_base& str );  // (1)
std::ios_base& noshowpoint( std::ios_base& str );  // (2)
```

Enables or disables the unconditional inclusion of the decimal point character
in floating-point output. Has no effect on input.

1) Enables the `showpoint` flag in the stream `str` as if by calling
   `str.setf(std::ios_base::showpoint)`.

2) Disables the `showpoint` flag in the stream `str` as if by calling
   `str.unsetf(std::ios_base::showpoint)`.

This is an I/O manipulator, it may be called with an expression such as `out <<
std::showpoint` for any `out` of type `std::basic_ostream` or with an expression
such as `in >> std::showpoint` for any `in` of type `std::basic_istream`.

The character to use as decimal point character is determined by the numpunct
facet of the locale imbued in the stream at the time of output, as described in
`std::num_put::put`.

### Parameters

- **str** — reference to I/O stream

### Return value

`str` (reference to the stream after manipulation).

### Example

```cpp
#include <iostream>

int main()
{
    std::cout << "1.0 with showpoint: " << std::showpoint << 1.0 << '\n'
              << "1.0 with noshowpoint: " << std::noshowpoint << 1.0 << '\n';
}
```

Output:

```text
1.0 with showpoint: 1.00000
1.0 with noshowpoint: 1
```

### See also

- **resetiosflags** — clears the specified ios_base flags (function)
- **setiosflags** — sets the specified `ios_base` flags (function)

---
*Source: https://en.cppreference.com/w/cpp/io/manip/showpoint*
