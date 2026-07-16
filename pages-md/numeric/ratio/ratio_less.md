# std::ratio_less

```cpp
template< class R1, class R2 >
struct ratio_less : std::integral_constant<bool, /* see below */> { };  // (since C++11)
```

If the ratio R1 is less than the ratio R2, provides the member constant `value`
equal `true`. Otherwise, `value` is `false`.

### Helper variable template

```cpp
template< class R1, class R2 >
inline constexpr bool ratio_less_v = ratio_less<R1, R2>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `R1::num * R2::den < R2::num * R1::den`, or
  equivalent expression that avoids overflow, `false` otherwise (public static
  member constant)

### Member functions

- **operator bool** — converts the object to bool, returns `value` (public
  member function)
- **operator() (C++14)** — returns `value` (public member function)

### Member types

- **`value_type`** — bool
- **`type`** — std::integral_constant<bool, value>

### Example

```cpp
#include <iostream>
#include <ratio>

int main()
{
    using x = std::ratio<69, 90>;
    using y = std::ratio<70, 90>;

    if constexpr (std::ratio_less_v<x, y>)
        std::cout << x::num << '/' << x::den << " < "
                  << y::num << '/' << y::den << '\n';
}
```

Output:

```text
23/30 < 7/9
```

### See also

- **ratio_greater (C++11)** — compares two `ratio` objects for *greater than* at
  compile-time (class template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/ratio/ratio_less*
