# std::atof

```cpp
double atof( const char* str );
```

Interprets a floating point value in a byte string pointed to by `str`.

Function discards any whitespace characters (as determined by `std::isspace`)
until first non-whitespace character is found. Then it takes as many characters
as possible to form a valid floating-point representation and converts them to a
floating-point value. The valid floating-point value can be one of the
following:

- decimal floating-point expression. It consists of the following parts:

- hexadecimal floating-point expression. It consists of the following parts:
- infinity expression. It consists of the following parts:
- not-a-number expression. It consists of the following parts:
*(since C++11)*

- any other expression that may be accepted by the currently installed C locale

### Parameters

- **str** — pointer to the null-terminated byte string to be interpreted

### Return value

`double` value corresponding to the contents of `str` on success. If the
converted value falls out of range of the return type, the return value is
undefined. If no conversion can be performed, `0.0` is returned.

### Example

```cpp
#include <cstdlib>
#include <iostream>

int main()
{
    std::cout << std::atof("0.0000000123") << '\n'
              << std::atof("0.012") << '\n'
              << std::atof("15e16") << '\n'
              << std::atof("-0x1afp-2") << '\n'
              << std::atof("inF") << '\n'
              << std::atof("Nan") << '\n'
              << std::atof("invalid") << '\n';
}
```

Output:

```text
1.23e-08
0.012
1.5e+17
-107.75
inf
nan
0
```

### See also

- **stofstodstold (C++11)(C++11)(C++11)** — converts a string to a floating
  point value (function)
- **strtofstrtodstrtold** — converts a byte string to a floating-point value
  (function)
- **from_chars (C++17)** — converts a character sequence to an integer or
  floating-point value (function)
- **atoiatolatoll (C++11)** — converts a byte string to an integer value
  (function)

**C documentation for `atof`**

---
*Source: https://en.cppreference.com/w/cpp/string/byte/atof*
