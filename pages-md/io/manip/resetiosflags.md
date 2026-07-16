# std::resetiosflags

```cpp
/*unspecified*/ resetiosflags( std::ios_base::fmtflags mask );
```

When used in an expression `out << resetiosflags(mask)` or `in >>
resetiosflags(mask)`, clears all format flags of the stream `out` or `in` as
specified by the `mask`.

### Parameters

- **mask** — bitmask of the flags to clear

### Return value

An object of unspecified type such that

- if `out` is an object of type `std::basic_ostream<CharT, Traits>`, the
  expression `out << resetiosflags(mask)` has type `std::basic_ostream<CharT,
  Traits>&` has value `out` behaves as if it called `f(out, mask)`
- if `in` is an object of type `std::basic_istream<CharT, Traits>`, the
  expression `in >> resetiosflags(mask)` has type `std::basic_istream<CharT,
  Traits>&` has value `in` behaves as if it called `f(in, mask)`

where the function `f` is defined as:

```cpp
void f(std::ios_base& str, std::ios_base::fmtflags mask)
{
    // reset specified flags
    str.setf(ios_base::fmtflags(0), mask);
}
```

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <sstream>

int main()
{
    std::istringstream in("10 010 10 010 10 010");
    int n1, n2;

    in >> std::oct >> n1 >> n2;
    std::cout << "Parsing \"10 010\" with std::oct gives: " << n1 << ' ' << n2 << '\n';

    in >> std::dec >> n1 >> n2;
    std::cout << "Parsing \"10 010\" with std::dec gives: " << n1 << ' ' << n2 << '\n';

    in >> std::resetiosflags(std::ios_base::basefield) >> n1 >> n2;
    std::cout << "Parsing \"10 010\" with autodetect gives: " << n1 << ' ' << n2 << '\n';
}
```

Output:

```text
Parsing "10 010" with std::oct gives: 8 8
Parsing "10 010" with std::dec gives: 10 10
Parsing "10 010" with autodetect gives: 10 8
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 183 | C++98 | `resetiosflags` could only be used with streams of type
      `std::ostream` or `std::istream` | usable with any character stream

### See also

- **setf** — sets specific format flag (public member function of
  `std::ios_base`)
- **setiosflags** — sets the specified `ios_base` flags (function)

---
*Source: https://en.cppreference.com/w/cpp/io/manip/resetiosflags*
