# std::max_align_t

```cpp
typedef /*implementation-defined*/ max_align_t;  // (since C++11)
```

`std::max_align_t` is a trivial standard-layout type whose alignment requirement
is at least as strict (as large) as that of every scalar type.

### Notes

Pointers returned by allocation functions such as `std::malloc` are suitably
aligned for any object, which means they are aligned at least as strictly as
`std::max_align_t`.

`std::max_align_t` is usually synonymous with the largest scalar type, which is
`long double` on most platforms, and its alignment requirement is either 8 or
16.

### Example

```cpp
#include <iostream>
#include <cstddef>
int main()
{
    std::cout << alignof(std::max_align_t) << '\n';
}
```

Possible output:

```text
16
```

### References

- C++23 standard (ISO/IEC 14882:2023):
- C++20 standard (ISO/IEC 14882:2020):
- C++17 standard (ISO/IEC 14882:2017):
- C++14 standard (ISO/IEC 14882:2014):
- C++11 standard (ISO/IEC 14882:2011):

### See also

- **`alignof` operator(C++11)** — queries alignment requirements of a type
- **alignment_of (C++11)** — obtains the type's alignment requirements (class
  template)
- **is_scalar (C++11)** — checks if a type is a scalar type (class template)

**C documentation for `max_align_t`**

---
*Source: https://en.cppreference.com/w/cpp/types/max_align_t*
