# std::cmp_equal, cmp_not_equal, cmp_less, cmp_greater, cmp_less_equal, cmp_greater_equal

```cpp
template< class T, class U >
constexpr bool cmp_equal( T t, U u ) noexcept;  // (1) (since C++20)
template< class T, class U >
constexpr bool cmp_not_equal( T t, U u ) noexcept;  // (2) (since C++20)
template< class T, class U >
constexpr bool cmp_less( T t, U u ) noexcept;  // (3) (since C++20)
template< class T, class U >
constexpr bool cmp_greater( T t, U u ) noexcept;  // (4) (since C++20)
template< class T, class U >
constexpr bool cmp_less_equal( T t, U u ) noexcept;  // (5) (since C++20)
template< class T, class U >
constexpr bool cmp_greater_equal( T t, U u ) noexcept;  // (6) (since C++20)
```

Compare the values of two integers `t` and `u`. Unlike builtin comparison
operators, negative signed integers always compare *less than* (and *not equal
to*) unsigned integers: the comparison is safe against non-value-preserving
integer conversion.

```cpp
-1 > 0u; // true
std::cmp_greater(-1, 0u); // false
```

It is a compile-time error if either `T` or `U` is a non-integer type, a
character type, or bool.

### Parameters

- **t** — left-hand argument
- **u** — right-hand argument

### Return value

1) `true` if `t` is equal to `u`.

2) `true` if `t` is not equal to `u`.

3) `true` if `t` is less than `u`.

4) `true` if `t` is greater than `u`.

5) `true` if `t` is less or equal to `u`.

6) `true` if `t` is greater or equal to `u`.

### Possible implementation

```cpp
template<class T, class U>
constexpr bool cmp_equal(T t, U u) noexcept
{
    if constexpr (std::is_signed_v<T> == std::is_signed_v<U>)
        return t == u;
    else if constexpr (std::is_signed_v<T>)
        return t >= 0 && std::make_unsigned_t<T>(t) == u;
    else
        return u >= 0 && std::make_unsigned_t<U>(u) == t;
}

template<class T, class U>
constexpr bool cmp_not_equal(T t, U u) noexcept
{
    return !cmp_equal(t, u);
}

template<class T, class U>
constexpr bool cmp_less(T t, U u) noexcept
{
    if constexpr (std::is_signed_v<T> == std::is_signed_v<U>)
        return t < u;
    else if constexpr (std::is_signed_v<T>)
        return t < 0 || std::make_unsigned_t<T>(t) < u;
    else
        return u >= 0 && t < std::make_unsigned_t<U>(u);
}

template<class T, class U>
constexpr bool cmp_greater(T t, U u) noexcept
{
    return cmp_less(u, t);
}

template<class T, class U>
constexpr bool cmp_less_equal(T t, U u) noexcept
{
    return !cmp_less(u, t);
}

template<class T, class U>
constexpr bool cmp_greater_equal(T t, U u) noexcept
{
    return !cmp_less(t, u);
}
```

### Notes

These functions cannot be used to compare enums (including `std::byte`), char,
char8_t, char16_t, char32_t, wchar_t and bool.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_integer_comparison_functions` | 202002L | (C++20) | Integer
      comparison functions

### Example

The example below might produce *different signedness comparison* warning if
compiled without an appropriate warning suppression flag, e.g.,
`-Wno-sign-compare` (gcc/clang with `-Wall -Wextra`, see also SO: disabling a
specific warning).

```cpp
#include <utility>

// Uncommenting the next line will disable "signed/unsigned comparison" warnings:
// #pragma GCC diagnostic ignored "-Wsign-compare"

int main()
{
    static_assert(sizeof(int) == 4); // precondition

    // Quite surprisingly
    static_assert(-1 > 1U); //< warning: sign-unsign comparison
    // because after implicit conversion of -1 to the RHS type (`unsigned int`)
    // the expression is equivalent to:
    static_assert(0xFFFFFFFFU > 1U);
    static_assert(0xFFFFFFFFU == static_cast<unsigned>(-1));

    // In contrast, the cmp_* family compares integers as most expected -
    // negative signed integers always compare less than unsigned integers:
    static_assert(std::cmp_less(-1, 1U));
    static_assert(std::cmp_less_equal(-1, 1U));
    static_assert(!std::cmp_greater(-1, 1U));
    static_assert(!std::cmp_greater_equal(-1, 1U));

    static_assert(-1 == 0xFFFFFFFFU); //< warning: sign-unsign comparison
    static_assert(std::cmp_not_equal(-1, 0xFFFFFFFFU));
}
```

### See also

- **equal_to** — function object implementing `x == y` (class template)
- **not_equal_to** — function object implementing `x != y` (class template)
- **less** — function object implementing `x < y` (class template)
- **greater** — function object implementing `x > y` (class template)
- **less_equal** — function object implementing `x <= y` (class template)
- **greater_equal** — function object implementing `x >= y` (class template)
- **ranges::equal_to (C++20)** — function object implementing `x == y` (class)
- **ranges::not_equal_to (C++20)** — function object implementing `x != y`
  (class)
- **ranges::less (C++20)** — function object implementing `x < y` (class)
- **ranges::greater (C++20)** — function object implementing `x > y` (class)
- **ranges::less_equal (C++20)** — function object implementing `x <= y` (class)
- **ranges::greater_equal (C++20)** — function object implementing `x >= y`
  (class)
- **compare_three_way (C++20)** — function object implementing `x <=> y` (class)
- **in_range (C++20)** — checks if an integer value is in the range of a given
  integer type (function template)
- **numeric_limits** — provides an interface to query properties of all
  fundamental numeric types (class template)

---
*Source: https://en.cppreference.com/w/cpp/utility/intcmp*
