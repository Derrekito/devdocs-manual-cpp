# std::setfill

```cpp
template< class CharT >
/*unspecified*/ setfill( CharT c );
```

When used in an expression `out << setfill(c)` sets the fill character of the
stream `out` to `c`.

### Parameters

- **c** — new value for the fill character

### Return value

An object of unspecified type such that

- if `out` is an object of type `std::basic_ostream<CharT, Traits>`, the
  expression `out << setfill(c)` has type `std::basic_ostream<CharT, Traits>&`
  has value `out` behaves as if it called `f(out, c)`

where the function `f` is defined as:

```cpp
template<class CharT, class Traits>
void f(std::basic_ios<CharT, Traits>& str, CharT c)
{
    // set fill character
    str.fill(c);
}
```

### Notes

The current fill character may be obtained with `std::ostream::fill`.

### Example

```cpp
#include <iomanip>
#include <iostream>

int main()
{
    std::cout << "default fill: [" << std::setw(10) << 42 << "]\n"
              << "setfill('*'): [" << std::setfill('*')
                                   << std::setw(10) << 42 << "]\n";
}
```

Output:

```text
default fill: [        42]
setfill('*'): [********42]
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 183 | C++98 | `setbase` could only be used with streams of type
      `std::ostream` | usable with any output character stream

### See also

- **fill** — manages the fill character (public member function of
  `std::basic_ios<CharT,Traits>`)
- **internalleftright** — sets the placement of fill characters (function)
- **setw** — changes the width of the next input/output field (function)

---
*Source: https://en.cppreference.com/w/cpp/io/manip/setfill*
