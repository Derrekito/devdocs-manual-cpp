# std::setiosflags

```cpp
/*unspecified*/ setiosflags( std::ios_base::fmtflags mask );
```

When used in an expression `out << setiosflags(mask)` or `in >>
setiosflags(mask)`, sets all format flags of the stream `out` or `in` as
specified by the `mask`.

### Parameters

- **mask** — bitmask of the flags to set

### Return value

An object of unspecified type such that

- if `out` is an object of type `std::basic_ostream<CharT, Traits>`, the
  expression `out << setiosflags(mask)` has type `std::basic_ostream<CharT,
  Traits>&` has value `out` behaves as if it called `f(out, mask)`
- if `in` is an object of type `std::basic_istream<CharT, Traits>`, the
  expression `in >> setiosflags(mask)` has type `std::basic_istream<CharT,
  Traits>&` has value `in` behaves as if it called `f(in, mask)`

where the function `f` is defined as:

```cpp
void f(std::ios_base& str, std::ios_base::fmtflags mask)
{
    // set specified flags
    str.setf(mask);
}
```

### Example

```cpp
#include <iomanip>
#include <iostream>

int main()
{
    std::cout << std::resetiosflags(std::ios_base::dec)
              << std::setiosflags(  std::ios_base::hex
                                  | std::ios_base::uppercase
                                  | std::ios_base::showbase) << 42 << '\n';
}
```

Output:

```text
0X2A
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 183 | C++98 | `setiosflags` could only be used with streams of type
      `std::ostream` or `std::istream` | usable with any character stream

### See also

- **setf** — sets specific format flag (public member function of
  `std::ios_base`)
- **resetiosflags** — clears the specified ios_base flags (function)

---
*Source: https://en.cppreference.com/w/cpp/io/manip/setiosflags*
