# std::to_string

```cpp
std::string to_string( int value );  // (1) (since C++11)
std::string to_string( long value );  // (2) (since C++11)
std::string to_string( long long value );  // (3) (since C++11)
std::string to_string( unsigned value );  // (4) (since C++11)
std::string to_string( unsigned long value );  // (5) (since C++11)
std::string to_string( unsigned long long value );  // (6) (since C++11)
std::string to_string( float value );  // (7) (since C++11)
std::string to_string( double value );  // (8) (since C++11)
std::string to_string( long double value );  // (9) (since C++11)
```

Converts a numeric value to `std::string`.

Let `buf` be an internal to the conversion functions buffer, sufficiently large
to contain the result of conversion.
1) Converts a signed integer to a string as if by `std::sprintf(buf, "%d",
value)`.
2) Converts a signed integer to a string as if by `std::sprintf(buf, "%ld",
value)`.
3) Converts a signed integer to a string as if by `std::sprintf(buf, "%lld",
value)`.
4) Converts an unsigned integer to a string as if by `std::sprintf(buf, "%u",
value)`.
5) Converts an unsigned integer to a string as if by `std::sprintf(buf, "%lu",
value)`.
6) Converts an unsigned integer to a string as if by `std::sprintf(buf, "%llu",
value)`.
7,8) Converts a floating point value to a string as if by `std::sprintf(buf,
"%f", value)`.
9) Converts a floating point value to a string as if by `std::sprintf(buf,
"%Lf", value)`.
*(until C++26)*

1-9) Converts a numeric value to a string as if by `std::format("{}", value)`.
*(since C++26)*

### Parameters

- **value** — a numeric value to convert

### Return value

A string holding the converted value.

### Exceptions

May throw `std::bad_alloc` from the `std::string` constructor.

### Notes

- With floating point types `std::to_string` may yield unexpected results as the
  number of significant digits in the returned string can be zero, see the
  example.
- The return value may differ significantly from what `std::cout` prints by
  default, see the example.

- `std::to_string` relies on the current C locale for formatting purposes, and
  therefore concurrent calls to `std::to_string` from multiple threads may
  result in partial serialization of calls. The results of overloads for integer
  types do not rely on the current C locale, and thus implementations generally
  avoid access to the current C locale in these overloads for both correctness
  and performance. However, such avoidance is not guaranteed by the standard.
*(until C++26)*

C++17 provides `std::to_chars` as a higher-performance locale-independent
alternative.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_to_string` | 202306L | (C++26) | Redefining `std::to_string` in
      terms of `std::format`

### Example

```cpp
#include <iostream>
#include <string>

int main()
{
    for (const double f : {23.43, 1e-9, 1e40, 1e-40, 123456789.0})
        std::cout << "std::cout: " << f << '\n'
                  << "to_string: " << std::to_string(f) << "\n\n";
}
```

Output:

```text
std::cout: 23.43
to_string: 23.430000

std::cout: 1e-09
to_string: 0.000000

std::cout: 1e+40
to_string: 10000000000000000303786028427003666890752.000000

std::cout: 1e-40
to_string: 0.000000

std::cout: 1.23457e+08
to_string: 123456789.000000
```

### See also

- **to_wstring (C++11)** — converts an integral or floating-point value to
  `wstring` (function)
- **stoulstoull (C++11)(C++11)** — converts a string to an unsigned integer
  (function)
- **stoistolstoll (C++11)(C++11)(C++11)** — converts a string to a signed
  integer (function)
- **stofstodstold (C++11)(C++11)(C++11)** — converts a string to a floating
  point value (function)
- **to_chars (C++17)** — converts an integer or floating-point value to a
  character sequence (function)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/to_string*
