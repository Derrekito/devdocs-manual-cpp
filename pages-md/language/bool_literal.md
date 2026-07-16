# Boolean literals

### Syntax

```text
true    (1)
false    (2)
```

### Explanation

The Boolean literals are the keywords `true` and `false`. They are prvalues of
type `bool`.

### Notes

See Integral conversions for implicit conversions from `bool` to other types and
boolean conversions for the implicit conversions from other types to `bool`.

### Example

```cpp
#include <iostream>

int main()
{
    std::cout << std::boolalpha
              << true << '\n'
              << false << '\n'
              << std::noboolalpha
              << true << '\n'
              << false << '\n';
}
```

Output:

```text
true
false
1
0
```

### References

- C++23 standard (ISO/IEC 14882:2023):
- C++20 standard (ISO/IEC 14882:2020):
- C++17 standard (ISO/IEC 14882:2017):
- C++14 standard (ISO/IEC 14882:2014):
- C++11 standard (ISO/IEC 14882:2011):
- C++98 standard (ISO/IEC 14882:1998):

### See also

**C documentation for Predefined Boolean constants**

---
*Source: https://en.cppreference.com/w/cpp/language/bool_literal*
