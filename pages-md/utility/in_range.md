# std::in_range

```cpp
template< class R, class T >
constexpr bool in_range( T t ) noexcept;  // (since C++20)
```

Returns `true` if the value of `t` is in the range of values that can be
represented in `R`, that is, if `t` can be converted to `R` in a
value-preserving manner.

It is a compile-time error if either `T` or `U` is a non-integer type, a
character type, or bool.

### Parameters

- **t** — value to test

### Return value

`true` if the value of `t` is representable in `R`, `false` otherwise.

### Possible implementation

```cpp
template<class R, class T>
constexpr bool in_range(T t) noexcept
{
    return std::cmp_greater_equal(t, std::numeric_limits<R>::min()) &&
        std::cmp_less_equal(t, std::numeric_limits<R>::max());
}
```

### Notes

This function cannot be used with enums (including `std::byte`), char, char8_t,
char16_t, char32_t, wchar_t and bool.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_integer_comparison_functions` | 202002L | (C++20) | Integer
      comparison functions

### Example

```cpp
#include <iostream>
#include <utility>

int main()
{
    std::cout << std::boolalpha;

    std::cout << std::in_range<std::size_t>(-1) << '\n';
    std::cout << std::in_range<std::size_t>(42) << '\n';
}
```

Output:

```text
false
true
```

### See also

- **ranges::min (C++20)** — returns the smaller of the given values (niebloid)
- **ranges::max (C++20)** — returns the greater of the given values (niebloid)
- **ranges::clamp (C++20)** — clamps a value between a pair of boundary values
  (niebloid)
- **lerp (C++20)** — linear interpolation function (function)

---
*Source: https://en.cppreference.com/w/cpp/utility/in_range*
