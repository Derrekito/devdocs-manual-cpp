# std::boolalpha, std::noboolalpha

```cpp
std::ios_base& boolalpha( std::ios_base& str );  // (1)
std::ios_base& noboolalpha( std::ios_base& str );  // (2)
```

1) Enables the `boolalpha` flag in the stream `str` as if by calling
   `str.setf(std::ios_base::boolalpha)`.

2) Disables the `boolalpha` flag in the stream `str` as if by calling
   `str.unsetf(std::ios_base::boolalpha)`.

`std::boolalpha` is an I/O manipulator, so it may be called with an expression
such as `out << std::boolalpha` for any `out` of type `std::basic_ostream` or
with an expression such as `in >> std::boolalpha` for any `in` of type
`std::basic_istream`.

### Parameters

- **str** — reference to I/O stream

### Return value

`str` (reference to the stream after manipulation).

### Example

```cpp
#include <iostream>
#include <sstream>

int main()
{
    // boolalpha output
    std::cout << "default true: " << true << '\n'
              << "default false: " << false << '\n'
              << std::boolalpha
              << "boolalpha true: " << true << '\n'
              << "boolalpha false: " << false << '\n'
              << std::noboolalpha
              << "noboolalpha true: " << true << '\n'
              << "noboolalpha false: " << false << '\n';

    // boolalpha parse
    bool b1, b2;
    std::istringstream is("true false");
    is >> std::boolalpha >> b1 >> b2;

    std::cout << '"' << is.str() << "\" parsed as: "
              << std::boolalpha << b1 << ' ' << b2 << '\n';
}
```

Output:

```text
default true: 1
default false: 0
boolalpha true: true
boolalpha false: false
noboolalpha true: 1
noboolalpha false: 0
"true false" parsed as: true false
```

### See also

- **resetiosflags** — clears the specified ios_base flags (function)
- **setiosflags** — sets the specified `ios_base` flags (function)
- **do_truenamedo_falsename [virtual]** — provides the string to use as the name
  of the boolean `true` and `false` (virtual protected member function of
  `std::numpunct<CharT>`)

---
*Source: https://en.cppreference.com/w/cpp/io/manip/boolalpha*
