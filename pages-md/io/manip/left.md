# std::left, std::right, std::internal

```cpp
std::ios_base& left( std::ios_base& str );  // (1)
std::ios_base& right( std::ios_base& str );  // (2)
std::ios_base& internal( std::ios_base& str );  // (3)
```

Modifies the positioning of the fill characters in an output stream. `left` and
`right` apply to any type being output, `internal` applies to integer,
floating-point, and monetary output. Has no effect on input.

1) Sets the `adjustfield` of the stream `str` to `left` as if by calling
   `str.setf(std::ios_base::left, std::ios_base::adjustfield)`.

2) Sets the `adjustfield` of the stream `str` to `right` as if by calling
   `str.setf(std::ios_base::right, std::ios_base::adjustfield)`.

3) Sets the `adjustfield` of the stream `str` to `internal` as if by calling
   `str.setf(std::ios_base::internal, std::ios_base::adjustfield)`.

The initial default for standard streams is equivalent to `right`.

This is an I/O manipulator. It may be called with an expression such as `out <<
std::left` for any `out` of type `std::basic_ostream` or with an expression such
as `in >> std::left` for any `in` of type `std::basic_istream`.

### Parameters

- **str** — reference to I/O stream

### Return value

`str` (reference to the stream after manipulation).

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <locale>

int main()
{
    std::cout.imbue(std::locale("en_US.utf8"));

    std::cout << "Default positioning:\n" << std::setfill('*')
              << std::setw(12) << -1.23  << '\n'
              << std::setw(12) << std::hex << std::showbase << 42 << '\n'
              << std::setw(12) << std::put_money(123, true) << "\n\n";

    std::cout << "Left positioning:\n" << std::left
              << std::setw(12) << -1.23  << '\n'
              << std::setw(12) << 42 << '\n'
              << std::setw(12) << std::put_money(123, true) << "\n\n";

    std::cout << "Internal positioning:\n" << std::internal
              << std::setw(12) << -1.23  << '\n'
              << std::setw(12) << 42 << '\n'
              << std::setw(12) << std::put_money(123, true) << "\n\n";

    std::cout << "Right positioning:\n" << std::right
              << std::setw(12) << -1.23  << '\n'
              << std::setw(12) << 42 << '\n'
              << std::setw(12) << std::put_money(123, true) << '\n';
}
```

Output:

```text
Default positioning:
*******-1.23
********0x2a
***USD *1.23

Left positioning:
-1.23*******
0x2a********
USD *1.23***

Internal positioning:
-*******1.23
0x********2a
USD ****1.23

Right positioning:
*******-1.23
********0x2a
***USD *1.23
```

### See also

- **setw** — changes the width of the next input/output field (function)
- **setfill** — changes the fill character (function template)
- **showbasenoshowbase** — controls whether prefix is used to indicate numeric
  base (function)

---
*Source: https://en.cppreference.com/w/cpp/io/manip/left*
