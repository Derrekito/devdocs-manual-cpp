# std::ratio_greater_equal

```cpp
template< class R1, class R2 >
struct ratio_greater_equal : std::integral_constant<bool, /* see below */> { };  // (since C++11)
```

If the ratio `R1` is greater than or equal to the ratio `R2`, provides the
member constant `value` equal `true`. Otherwise, `value` is `false`.

### Helper variable template

```cpp
template< class R1, class R2 >
inline constexpr bool ratio_greater_equal_v = ratio_greater_equal<R1, R2>::value;  // (since C++17)
```

## Inherited from std::integral_constant

### Member constants

- **value [static]** — `true` if `R1::num * R2::den >= R2::num * R1::den`, or
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
    static_assert(std::ratio_greater_equal<
        std::ratio<2, 3>,
        std::ratio<2, 3>>::value, "2/3 >= 2/3");

    if (std::ratio_greater_equal<
        std::ratio<2,1>, std::ratio<1, 2>>::value)
        std::cout << "2/1 >= 1/2" "\n";

    if (std::ratio_greater_equal<
        std::ratio<1,2>, std::ratio<1, 2>>::value)
        std::cout << "1/2 >= 1/2" "\n";

    // Since C++17
    static_assert(std::ratio_greater_equal_v<
        std::ratio<999'999, 1'000'000>,
        std::ratio<999'998, 999'999>> );

    if constexpr (std::ratio_greater_equal_v<
        std::ratio<999'999, 1'000'000>,
        std::ratio<999'998, 999'999>>)
        std::cout << "999'999/1'000'000 >= 999'998/999'999" "\n";

    if constexpr (std::ratio_greater_equal_v<
        std::ratio<999'999, 1'000'000>,
        std::ratio<999'999, 1'000'000>>)
        std::cout << "999'999/1'000'000 >= 999'999/1'000'000" "\n";
}
```

Output:

```text
2/1 >= 1/2
1/2 >= 1/2
999'999/1'000'000 >= 999'998/999'999
999'999/1'000'000 >= 999'999/1'000'000
```

### See also

- **ratio_equal (C++11)** — compares two `ratio` objects for equality at
  compile-time (class template)
- **ratio_less (C++11)** — compares two `ratio` objects for *less than* at
  compile-time (class template)

---
*Source: https://en.cppreference.com/w/cpp/numeric/ratio/ratio_greater_equal*
