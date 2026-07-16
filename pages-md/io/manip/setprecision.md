# std::setprecision

```cpp
/*unspecified*/ setprecision( int n );
```

When used in an expression `out << setprecision(n)` or `in >> setprecision(n)`,
sets the `precision` parameter of the stream `out` or `in` to exactly `n`.

### Parameters

- **n** — new value for precision

### Return value

An object of unspecified type such that

- if `out` is an object of type `std::basic_ostream<CharT, Traits>`, the
  expression `out << setprecision(n)` has type `std::basic_ostream<CharT,
  Traits>&` has value `out` behaves as if it called `f(out, n)`
- if `in` is an object of type `std::basic_istream<CharT, Traits>`, the
  expression `in >> setprecision(n)` has type `std::basic_istream<CharT,
  Traits>&` has value `in` behaves as if it called `f(in, n)`

where the function `f` is defined as:

```cpp
void f(std::ios_base& str, int n)
{
    // set precision
    str.precision(n);
}
```

### Example

```cpp
#include <iomanip>
#include <iostream>
#include <limits>
#include <numbers>

int main()
{
    constexpr long double pi{std::numbers::pi_v<long double>};

    const auto default_precision{std::cout.precision()};
    constexpr auto max_precision{std::numeric_limits<long double>::digits10 + 1};

    std::cout << "default precision: " << default_precision << '\n'
              << "maximum precision: " << max_precision << "\n\n"
                 "precision: pi:\n";

    for (int p{0}; p <= max_precision; ++p)
        std::cout << std::setw(2) << p << "  " << std::setprecision(p) << pi << '\n';

    std::cout << std::setprecision(default_precision); // restore defaults
}
```

Output:

```text
default precision: 6
maximum precision: 19

precision: pi:
 0  3
 1  3
 2  3.1
 3  3.14
 4  3.142
 5  3.1416
 6  3.14159
 7  3.141593
 8  3.1415927
 9  3.14159265
10  3.141592654
11  3.1415926536
12  3.14159265359
13  3.14159265359
14  3.1415926535898
15  3.14159265358979
16  3.141592653589793
17  3.1415926535897932
18  3.14159265358979324
19  3.141592653589793239
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 183 | C++98 | `setprecision` could only be used with streams of type
      `std::ostream` or `std::istream` | usable with any character stream

### See also

- **fixedscientifichexfloatdefaultfloat (C++11)(C++11)** — changes formatting
  used for floating-point I/O (function)
- **precision** — manages decimal precision of floating point operations (public
  member function of `std::ios_base`)

---
*Source: https://en.cppreference.com/w/cpp/io/manip/setprecision*
