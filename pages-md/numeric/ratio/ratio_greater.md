# std::ratio_greater

```cpp
template< class R1, class R2 >
struct ratio_greater : std::integral_constant<bool, /* see below */> { };  // (since C++11)
```

If the ratio `R1` is greater than the ratio `R2`, provides the member constant
`value` equal `true`. Otherwise, `value` is `false`.

### Helper variable template

```cpp
template< class R1, class R2 >
inline constexpr bool ratio_greater_v = ratio_greater<R1, R2>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `R1::num * R2::den > R2::num * R1::den`, or
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
    static_assert(std::ratio_greater<std::ratio<3, 4>, std::ratio<1, 2>>::value,
                  "3/4 > 1/2");

    if (std::ratio_greater<std::ratio<11, 12>, std::ratio<10, 11>>::value)
        std::cout << "11/12 > 10/11" "\n";

    // Since C++17
    static_assert(std::ratio_greater_v<std::ratio<12, 13>, std::ratio<11, 12>>);

    if constexpr (std::ratio_greater_v<std::ratio<12, 13>, std::ratio<11, 12>>)
        std::cout << "12/13 > 11/12" "\n";
}
```

Output:

```text
11/12 > 10/11
12/13 > 11/12
```

### See also

- **ratio_greater_equal (C++11)** — compares two `ratio` objects for *greater
  than or equal to* at compile-time (class template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/ratio/ratio_greater*
