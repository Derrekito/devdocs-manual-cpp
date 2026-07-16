# std::ratio

```cpp
template<
    std::intmax_t Num,
    std::intmax_t Denom = 1
> class ratio;  // (since C++11)
```

The class template `std::ratio` provides compile-time rational arithmetic
support. Each instantiation of this template exactly represents any finite
rational number as long as its numerator `Num` and denominator `Denom` are
representable as compile-time constants of type `std::intmax_t`. In addition,
`Denom` may not be zero and both `Num` and `Denom` may not be equal to the most
negative value.

The static data members `num` and `den` representing the numerator and
denominator are calculated by dividing `Num` and `Denom` by their greatest
common divisor. However, two `std::ratio` with different `Num` or `Denom` are
distinct types even if they represent the same rational number (after
reduction). A `std::ratio` type can be reduced to the lowest terms via its
`type` member: std::ratio<3, 6>::type is std::ratio<1, 2>.

The following convenience typedefs that correspond to the SI ratios are provided
by the standard library:

- **`quecto` (since C++26)** — std::ratio<1,
  1000000000000000000000000000000>(10-30)[1]
- **`ronto` (since C++26)** — std::ratio<1,
  1000000000000000000000000000>(10-27)[1]
- **`yocto`** — std::ratio<1, 1000000000000000000000000>(10-24)[1]
- **`zepto`** — std::ratio<1, 1000000000000000000000>(10-21)[1]
- **`atto`** — std::ratio<1, 1000000000000000000>(10-18)
- **`femto`** — std::ratio<1, 1000000000000000>(10-15)
- **`pico`** — std::ratio<1, 1000000000000>(10-12)
- **`nano`** — std::ratio<1, 1000000000>(10-9)
- **`micro`** — std::ratio<1, 1000000>(10-6)
- **`milli`** — std::ratio<1, 1000>(10-3)
- **`centi`** — std::ratio<1, 100>(10-2)
- **`deci`** — std::ratio<1, 10>(10-1)
- **`deca`** — std::ratio<10, 1>(101)
- **`hecto`** — std::ratio<100, 1>(102)
- **`kilo`** — std::ratio<1000, 1>(103)
- **`mega`** — std::ratio<1000000, 1>(106)
- **`giga`** — std::ratio<1000000000, 1>(109)
- **`tera`** — std::ratio<1000000000000, 1>(1012)
- **`peta`** — std::ratio<1000000000000000, 1>(1015)
- **`exa`** — std::ratio<1000000000000000000, 1>(1018)
- **`zetta`** — std::ratio<1000000000000000000000, 1>(1021)[2]
- **`yotta`** — std::ratio<1000000000000000000000000, 1>(1024)[2]
- **`ronna` (since C++26)** — std::ratio<1000000000000000000000000000,
  1>(1027)[2]
- **`quetta` (since C++26)** — std::ratio<1000000000000000000000000000000,
  1>(1030)[2]

1. These typedefs are only defined if `std::intmax_t` can represent the
   denominator.
1. These typedefs are only defined if `std::intmax_t` can represent the
   numerator.

### Nested types

- **`type`** — std::ratio<num, den> (the rational type after reduction)

### Data members

In the definitions given below,

- `sign(Denom)` is `-1` if `Denom` is negative, or `1` otherwise; and
- `gcd(Num, Denom)` is the greatest common divisor of `std::abs(Num)` and
  `std::abs(Denom)`.

- **`constexpr std::intmax_t` num [static]** — `sign(Denom) * Num / gcd(Num,
  Denom)` (public static member constant)
- **`constexpr std::intmax_t` den [static]** — `std::abs(Denom) / gcd(Num,
  Denom)` (public static member constant)

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_ratio` | 202306L | (C++26) | Adding the new 2022 SI prefixes:
      quecto, quetta, ronto, ronna

### Example

```cpp
#include <ratio>

static_assert
(
    std::ratio_equal_v<std::ratio_multiply<std::femto, std::exa>, std::kilo>
);

int main() {}
```

### See also

- **Mathematical constants (C++20)** — provides several mathematical constants,
  such as `std::numbers::e` for e

---
*Source: https://en.cppreference.com/w/cpp/numeric/ratio/ratio*
