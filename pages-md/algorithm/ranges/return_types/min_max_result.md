# std::ranges::min_max_result

```cpp
template< class T >
struct min_max_result;  // (since C++20)
```

`ranges::min_max_result` is a class template that provides a way to store two
objects or references of the same type as a single unit.

This class template has no base classes or declared members other than those
shown below. Thus it is suitable for use with structured bindings.

All special member functions of this class template are implicitly declared,
which makes specializations be aggregate classes, and propagate triviality,
potentially-throwing-ness, and constexpr-ness of corresponding operations on
data members.

### Template parameters

- **T** — the type of the objects or references that the
  `ranges::min_max_result` stores.

### Data members

## std::ranges::min_max_result::min

```cpp
[[no_unique_address]] T min;
```

May be a reference to, a copy of, or an iterator to a minimum element in a
range.

## std::ranges::min_max_result::max

```cpp
[[no_unique_address]] T max;
```

May be a reference to, a copy of, or an iterator to a maximum element in a
range.

### Member functions

## std::ranges::min_max_result::operator min_max_result<T2>

```cpp
template<class T2>
requires std::convertible_to<const T&, T2>
constexpr operator min_max_result<T2>() const &;  // (1)
template<class T2>
requires std::convertible_to<T, T2>
constexpr operator min_max_result<T2>() &&;  // (2)
```

Converts `*this` to the result by constructing every data member of the result
from the corresponding member of `*this`.

1) Equivalent to `return {min, max};`.

2) Equivalent to `return {std::move(min), std::move(max)};`.

### Standard library

The following standard library functions use `ranges::min_max_result` as the
return type:

**Algorithm functions**

- **ranges::minmax (C++20)** — returns the smaller and larger of two elements
  (niebloid)
- **ranges::minmax_element (C++20)** — returns the smallest and the largest
  elements in a range (niebloid)

### Synopsis

```cpp
namespace std::ranges
{
    template<class T>
    struct min_max_result
    {
        [[no_unique_address]] T min;
        [[no_unique_address]] T max;

        template<class T2>
        requires std::convertible_to<const T&, T2>
        constexpr operator min_max_result<T2>() const &
        {
            return {min, max};
        }

        template<class T2>
        requires std::convertible_to<T, T2>
        constexpr operator min_max_result<T2>() &&
        {
            return {std::move(min), std::move(max)};
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
#include <ranges>

int main()
{
    constexpr static auto v = {3, 1, 4, 1, 5, 9, 2};
    {
        constexpr auto result = std::ranges::minmax(v);
        static_assert(1 == result.min && 9 == result.max);
    }
    {
        constexpr auto result = std::ranges::minmax_element(v);
        static_assert(1 == *result.min && 9 == *result.max);
    }
}
```

### See also

- **pair** — implements binary tuple, i.e. a pair of values (class template)
- **tuple (C++11)** — implements fixed size container, which holds elements of
  possibly different types (class template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/return_types/min_max_result*
