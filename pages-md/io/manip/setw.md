# std::setw

```cpp
/* unspecified */ setw( int n );
```

When used in an expression `out << std::setw(n)` or `in >> std::setw(n)`, sets
the `width` parameter of the stream `out` or `in` to exactly `n`.

Some operations reset the width to zero (see below), so `std::setw` may need to
be repeatedly called to set the width for multiple operations.

### Parameters

- **n** — new value for width

### Return value

An object of unspecified type such that

- if `out` is an object of type `std::basic_ostream<CharT, Traits>`, the
  expression `out << setw(n)` has type `std::basic_ostream<CharT, Traits>&` has
  value `out` behaves as if it called `f(out, n)`
- if `in` is an object of type `std::basic_istream<CharT, Traits>`, the
  expression `in >> setw(n)` has type `std::basic_istream<CharT, Traits>&` has
  value `in` behaves as if it called `f(in, n)`

where the function `f` is defined as:

```cpp
void f(std::ios_base& str, int n)
{
    // set width
    str.width(n);
}
```

### Notes

The width property of the stream will be reset to zero (meaning "unspecified")
if any of the following functions are called:

- Input
- Output

The exact effects this modifier has on the input and output vary between the
individual I/O functions and are described at each operator<< and operator>>
overload page individually.

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <sstream>

int main()
{
    std::cout << "no setw: [" << 42 << "]\n"
              << "setw(6): [" << std::setw(6) << 42 << "]\n"
              << "no setw, several elements: [" << 89 << 12 << 34 << "]\n"
              << "setw(6), several elements: [" << 89 << std::setw(6) << 12 << 34 << "]\n";

    std::istringstream is("hello, world");
    char arr[10];

    is >> std::setw(6) >> arr;
    std::cout << "Input from \"" << is.str() << "\" with setw(6) gave \""
              << arr << "\"\n";
}
```

Output:

```text
no setw: [42]
setw(6): [    42]
no setw, several elements: [891234]
setw(6), several elements: [89    1234]
Input from "hello, world" with setw(6) gave "hello"
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 183 | C++98 | `setw` could only be used with streams of type
      `std::ostream` or `std::istream` | usable with any character stream

### See also

- **width** — manages field width (public member function of `std::ios_base`)
- **setfill** — changes the fill character (function template)
- **internalleftright** — sets the placement of fill characters (function)
- **showbasenoshowbase** — controls whether prefix is used to indicate numeric
  base (function)

---
*Source: https://en.cppreference.com/w/cpp/io/manip/setw*
