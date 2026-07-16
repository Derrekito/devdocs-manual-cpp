# std::to_wstring

```cpp
std::wstring to_wstring( int value );  // (1) (since C++11)
std::wstring to_wstring( long value );  // (2) (since C++11)
std::wstring to_wstring( long long value );  // (3) (since C++11)
std::wstring to_wstring( unsigned value );  // (4) (since C++11)
std::wstring to_wstring( unsigned long value );  // (5) (since C++11)
std::wstring to_wstring( unsigned long long value );  // (6) (since C++11)
std::wstring to_wstring( float value );  // (7) (since C++11)
std::wstring to_wstring( double value );  // (8) (since C++11)
std::wstring to_wstring( long double value );  // (9) (since C++11)
```

Converts a numeric value to `std::wstring`.

Let `buf` be an internal to the conversion functions buffer, sufficiently large
to contain the result of conversion.
1) Converts a signed decimal integer to a wide string as if by
`std::swprintf(buf, sz, L"%d", value)`.
2) Converts a signed decimal integer to a wide string as if by
`std::swprintf(buf, sz, L"%ld", value)`.
3) Converts a signed decimal integer to a wide string as if by
`std::swprintf(buf, sz, L"%lld", value)`.
4) Converts an unsigned decimal integer to a wide string as if by
`std::swprintf(buf, sz, L"%u", value)`.
5) Converts an unsigned decimal integer to a wide string as if by
`std::swprintf(buf, sz, L"%lu", value)`.
6) Converts an unsigned decimal integer to a wide string as if by
`std::swprintf(buf, sz, L"%llu", value)`.
7,8) Converts a floating point value to a wide string as if by
`std::swprintf(buf, sz, L"%f", value)`.
9) Converts a floating point value to a wide string as if by `std::swprintf(buf,
sz, L"%Lf", value)`.
*(until C++26)*

1-9) Converts a numeric value to a wide string as if by `std::format(L"{}",
value)`.
*(since C++26)*

### Parameters

- **value** — a numeric value to convert

### Return value

A wide string holding the converted value.

### Exceptions

May throw `std::bad_alloc` from the `std::wstring` constructor.

### Example

```cpp
#include <iostream>
#include <string>

int main()
{
    for (const double f : {23.43, 1e-9, 1e40, 1e-40, 123456789.0})
        std::wcout << "std::wcout: " << f << '\n'
                   << "to_wstring: " << std::to_wstring(f) << "\n\n";
}
```

Output:

```text
std::wcout: 23.43
to_wstring: 23.430000

std::wcout: 1e-09
to_wstring: 0.000000

std::wcout: 1e+40
to_wstring: 10000000000000000303786028427003666890752.000000

std::wcout: 1e-40
to_wstring: 0.000000

std::wcout: 1.23457e+08
to_wstring: 123456789.000000
```

### See also

- **to_string (C++11)** — converts an integral or floating-point value to
  `string` (function)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/to_wstring*
