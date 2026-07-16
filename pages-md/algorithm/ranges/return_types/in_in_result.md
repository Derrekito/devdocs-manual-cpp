# std::ranges::in_in_result

```cpp
template< class I1, class I2 >
struct in_in_result;  // (since C++20)
```

`ranges::in_in_result` is a class template that provides a way to store two
iterators as a single unit.

This class template has no base classes or declared members other than those
shown below. Thus it is suitable for use with structured bindings.

All special member functions of this class template are implicitly declared,
which makes specializations be aggregate classes, and propagate triviality,
potentially-throwing-ness, and constexpr-ness of corresponding operations on
data members.

### Template parameters

- **I1, I2** — the types of the iterators that the `ranges::in_in_result`
  stores.

### Data members

## std::ranges::in_in_result::in1

```cpp
[[no_unique_address]] I1 in1;
```

a value (that is supposed to be an iterator)

## std::ranges::in_in_result::in2

```cpp
[[no_unique_address]] I2 in2;
```

a value (that is supposed to be an iterator)

### Member functions

## std::ranges::in_in_result::operator in_in_result<II1, II2>

```cpp
template<class II1, class II2>
requires std::convertible_to<const I1&, II1> && std::convertible_to<const I2&, II2>
constexpr operator in_in_result<II1, II2>() const &;  // (1)
template<class II1, class II2>
requires std::convertible_to<I1, II1> && std::convertible_to<I2, II2>
constexpr operator in_in_result<II1, II2>() &&;  // (2)
```

Converts `*this` to the result by constructing every data member of the result
from the corresponding member of `*this`.

1) Equivalent to `return {in1, in2};`.

2) Equivalent to `return {std::move(in1), std::move(in2)};`.

### Standard library

The following standard library functions use `ranges::in_in_result` as the
return type:

**Algorithm functions**

- **ranges::mismatch (C++20)** — finds the first position where two ranges
  differ (niebloid)
- **ranges::swap_ranges (C++20)** — swaps two ranges of elements (niebloid)

### Synopsis

```cpp
namespace std::ranges
{
    template<class I1, class I2>
    struct in_in_result
    {
        [[no_unique_address]] I1 in1;
        [[no_unique_address]] I2 in2;

        template<class II1, class II2>
        requires std::convertible_to<const I1&, II1> && std::convertible_to<const I2&, II2>
        constexpr operator in_in_result<II1, II2>() const &
        {
            return {in1, in2};
        }

        template<class II1, class II2>
        requires std::convertible_to<I1, II1> && std::convertible_to<I2, II2>
        constexpr operator in_in_result<II1, II2>() &&
        {
            return {std::move(in1), std::move(in2)};
        }
    };
}
```

### Notes

Each standard library algorithm that uses this family of return types declares a
new alias type, e.g. `using merge_result = in_in_out_result<I1, I2, O>;`.

The names for such aliases are formed by adding the suffix "*`_result`*" to the
algorithm's name. So, the return type of `std::ranges::merge` can be named as
`std::ranges::merge_result`.

Unlike `std::pair` and `std::tuple`, this class template has data members of
meaningful names.

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <ranges>

int main()
{
    constexpr static auto in1 = {1, 2, 3, 4};
    constexpr static auto in2 = {1, 2, 4, 5};

    constexpr auto result {std::ranges::mismatch(in1, in2)};

    static_assert(2 == std::ranges::distance(in1.begin(), result.in1));
    static_assert(2 == std::ranges::distance(in2.begin(), result.in2));
}
```

### See also

- **pair** — implements binary tuple, i.e. a pair of values (class template)
- **tuple (C++11)** — implements fixed size container, which holds elements of
  possibly different types (class template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/return_types/in_in_result*
