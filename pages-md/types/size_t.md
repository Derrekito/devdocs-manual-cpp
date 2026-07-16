# std::size_t

```cpp
typedef /*implementation-defined*/ size_t;
```

`std::size_t` is the unsigned integer type of the result of the `sizeof`
operator as well as the `sizeof...` operator and the `alignof` operator(since
C++11).

The bit width of `std::size_t` is not less than 16.
*(since C++11)*

### Notes

`std::size_t` can store the maximum size of a theoretically possible object of
any type (including array). A type whose size cannot be represented by
`std::size_t` is ill-formed. On many platforms (an exception is systems with
segmented addressing) `std::size_t` can safely store the value of any non-member
pointer, in which case it is synonymous with `std::uintptr_t`.

`std::size_t` is commonly used for array indexing and loop counting. Programs
that use other types, such as `unsigned int`, for array indexing may fail on,
e.g. 64-bit systems when the index exceeds `UINT_MAX` or if it relies on 32-bit
modular arithmetic.

When indexing C++ containers, such as `std::string`, `std::vector`, etc, the
appropriate type is the member typedef `size_type` provided by such containers.
It is usually defined as a synonym for `std::size_t`.

The integer literal suffix for `std::size_t` is any combination of `z` or `Z`
with `u` or `U` (i.e. `zu`, `zU`, `Zu`, `ZU`, `uz`, `uZ`, `Uz`, or `UZ`).
*(since C++23)*

### Example

```cpp
#include <array>
#include <cstddef>
#include <iostream>

int main()
{
    std::array<std::size_t, 10> a;

    // Example with C++23 size_t literal
    for (auto i = 0uz; i != a.size(); ++i)
        std::cout << (a[i] = i) << ' ';
    std::cout << '\n';

    // Example of decrementing loop
    for (std::size_t i = a.size(); i--;)
        std::cout << a[i] << ' ';
    std::cout << '\n';

    // Note the naive decrementing loop:
    //  for (std::size_t i = a.size() - 1; i >= 0; --i) ...
    // is an infinite loop, because unsigned numbers are always non-negative
}
```

Output:

```text
0 1 2 3 4 5 6 7 8 9
9 8 7 6 5 4 3 2 1 0
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

- **ptrdiff_t** — signed integer type returned when subtracting two pointers
  (typedef)
- **offsetof** — byte offset from the beginning of a standard-layout type to
  specified member (function macro)
- **integer literals** — binary,(since C++14) decimal, octal, or hexadecimal
  numbers of integer type

**C documentation for `size_t`**

---
*Source: https://en.cppreference.com/w/cpp/types/size_t*
