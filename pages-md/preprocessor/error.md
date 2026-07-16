# Diagnostic directives

Shows the given error message and renders the program ill-formed, or shows the
given warning message without affecting the validity of the program(since
C++23).

### Syntax

```text
#error diagnostic-message    (1)
#warning diagnostic-message    (2)    (since C++23)
```

### Explanation

1) After encountering the `#error` directive, an implementation displays the
   message diagnostic-message and renders the program ill-formed (the
   compilation stops).

2) Same as (1), except the validity of the program is not affected and the
   compilation continues.

diagnostic-message can consist of several words not necessarily in quotes.

### Notes

Before its standardization in C++23, `#warning` has been provided by many
compilers in all modes as a conforming extension.

### Example

```cpp
#if __STDC_HOSTED__ != 1
#   error "Not a hosted implementation"
#endif

#if __cplusplus >= 202302L
#   warning "Using #warning as a standard feature"
#endif

#include <iostream>

int main()
{
    std::cout << "The implementation used is hosted\n";
}
```

Possible output:

```text
The implementation used is hosted
```

### References

- C++23 standard (ISO/IEC 14882:2023):
- C++20 standard (ISO/IEC 14882:2020):
- C++17 standard (ISO/IEC 14882:2017):
- C++14 standard (ISO/IEC 14882:2014):
- C++11 standard (ISO/IEC 14882:2011):
- C++03 standard (ISO/IEC 14882:2003):
- C++98 standard (ISO/IEC 14882:1998):

### See also

**C documentation for Diagnostic directives**

---
*Source: https://en.cppreference.com/w/cpp/preprocessor/error*
