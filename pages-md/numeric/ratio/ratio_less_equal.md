# std::ratio_less_equal

```cpp
template< class R1, class R2 >
struct ratio_less_equal : std::integral_constant<bool, /* see below */> { };  // (since C++11)
```

If the ratio `R1` is less than or equal to the ratio `R2`, provides the member
constant `value` equal `true`. Otherwise, `value` is `false`.

### Helper variable template

```cpp
template< class R1, class R2 >
inline constexpr bool ratio_less_equal_v = ratio_less_equal<R1, R2>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `R1::num * R2::den <= R2::num * R1::den`, or
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
    static_assert(std::ratio_less_equal<std::ratio<1, 2>, std::ratio<3, 4>>::value,
                  "1/2 <= 3/4");

    if (std::ratio_less_equal<std::ratio<10,11>, std::ratio<11,12>>::value)
        std::cout << "10/11 <= 11/12" "\n";

    static_assert(std::ratio_less_equal_v<std::ratio<10, 11>, std::ratio<11, 12>>);

    if constexpr (std::ratio_less_equal_v<std::ratio<10, 11>, std::ratio<11, 12>>)
        std::cout << "11/12 <= 12/13" "\n";
}
```

Output:

```text
10/11 <= 11/12
11/12 <= 12/13
```

### See also

- **ratio_equal (C++11)** — compares two `ratio` objects for equality at
  compile-time (class template)
- **ratio_not_equal (C++11)** — compares two `ratio` objects for inequality at
  compile-time (class template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/ratio/ratio_less_equal*
