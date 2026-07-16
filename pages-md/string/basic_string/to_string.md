# std::to_string

Converts a number to a `std::string` — the quick, low-ceremony way to
get a numeric value into a string, at the cost of `printf`-style
formatting rather than anything configurable.

```cpp skip
std::to_string(42);        // "42"
std::to_string(3.14);      // "3.140000" — %f-style, six decimals, no trimming
```

All overloads since C++11, covering every built-in signed/unsigned
integer type and `float`/`double`/`long double`.

### What you provide

- **value** — the number to convert: any of `int`, `long`, `long
  long`, `unsigned`, `unsigned long`, `unsigned long long`, `float`,
  `double`, or `long double`.

### Guarantees and costs

- Integer overloads convert as if by `std::sprintf` with `%d`/`%ld`/
  `%lld` (or the unsigned equivalents) — exact, no locale dependence
  in practice, though the standard doesn't formally guarantee that.
- Floating-point overloads convert as if by `std::sprintf` with `%f`
  (`%Lf` for `long double`) — always fixed notation, always six
  digits after the decimal point, regardless of magnitude or how many
  of those digits are significant.
- May throw `std::bad_alloc` from the `std::string` constructor.
- Relies on the current C locale for formatting; concurrent calls from
  multiple threads may partially serialize as a result (integer
  overloads generally avoid this in practice, though it isn't
  guaranteed). *(until C++26)*
- Since C++26, every overload is redefined in terms of
  `std::format("{}", value)`, which sidesteps the locale dependency.

### Gotchas

- Floating-point results can be misleading: `1e-9` becomes
  `"0.000000"` (six decimals swallows the whole value), and `1e40`
  becomes a 40-plus-digit string — nothing like what `std::cout`
  prints by default. See the example.
- Always fixed notation with a hardcoded six-digit precision; there's
  no way to request scientific notation, a different precision, or
  trimming of trailing zeroes. For control over any of that, use
  `std::to_chars` (fast, locale-free) or `std::format` (flexible).

### Example

```cpp
#include <iostream>
#include <string>

int main()
{
    for (double f : {23.43, 1e-9, 1e-40})
        std::cout << "std::cout: " << f << '\n'
                  << "to_string: " << std::to_string(f) << "\n\n";
}
```

```text
std::cout: 23.43
to_string: 23.430000

std::cout: 1e-09
to_string: 0.000000

std::cout: 1e-40
to_string: 0.000000
```

### Reference

```cpp skip
std::string to_string( int value );               // (since C++11)
std::string to_string( long value );               // (since C++11)
std::string to_string( long long value );           // (since C++11)
std::string to_string( unsigned value );            // (since C++11)
std::string to_string( unsigned long value );        // (since C++11)
std::string to_string( unsigned long long value );   // (since C++11)
std::string to_string( float value );               // (since C++11)
std::string to_string( double value );              // (since C++11)
std::string to_string( long double value );          // (since C++11)
```

Feature-test macro: `__cpp_lib_to_string` — `202306L` (C++26,
redefining `to_string` in terms of `std::format`).

### See also

- **to_wstring (C++11)** — same, producing a `std::wstring`
- **to_chars (C++17)** — locale-free, non-allocating, non-throwing conversion
- **stoi, stol, stoll (C++11)** — string to signed integer
- **stof, stod, stold (C++11)** — string to floating point

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/to_string*
