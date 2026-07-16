# std::ranges::out_value_result

```cpp
template< class O, class T >
struct out_value_result;  // (since C++23)
```

`ranges::out_value_result` is a class template that provides a way to store an
iterator and a value as a single unit.

This class template has no base classes or declared members other than those
shown below. Thus it is suitable for use with structured bindings.

All special member functions of this class template are implicitly declared,
which makes specializations be aggregate classes, and propagate triviality,
potentially-throwing-ness, and constexpr-ness of corresponding operations on
data members.

### Template parameters

- **O, T** — the types of the objects that the `ranges::out_value_result`
  stores.

### Data members

## std::ranges::out_value_result::out

```cpp
[[no_unique_address]] O out;
```

a value (that is supposed to be an iterator)

## std::ranges::out_value_result::value

```cpp
[[no_unique_address]] T value;
```

a value (that is supposed to be a stored value)

### Member functions

## std::ranges::out_value_result::operator out_value_result<O2, T2>

```cpp
template<class O2, class T2>
requires convertible_to<const O&, O2> && convertible_to<const T&, T2>
constexpr operator out_value_result<O2, T2>() const &;  // (1)
template<class O2, class T2>
requires convertible_to<O, O2> && convertible_to<T, T2>
constexpr operator out_value_result<O2, T2>() &&;  // (2)
```

Converts `*this` to the result by constructing every data member of the result
from the corresponding member of `*this`.

1) Equivalent to `return {out, value};`.

2) Equivalent to `return {std::move(out), std::move(value)};`.

### Standard library

The following standard library functions use `ranges::out_value_result` as the
return type:

**Algorithm functions**

- **ranges::iota (C++23)** — fills a range with successive increments of the
  starting value (niebloid)

### Synopsis

```cpp
namespace std::ranges
{
    template<class O, class T>
    struct out_value_result
    {
        [[no_unique_address]] O out;
        [[no_unique_address]] T value;

        template<class O2, class T2>
        requires convertible_to<const O&, O2> && convertible_to<const T&, T2>
        constexpr operator out_value_result<O2, T2>() const &
        {
            return {out, value};
        }

        template<class O2, class T2>
        requires convertible_to<O, O2> && convertible_to<T, T2>
        constexpr operator out_value_result<O2, T2>() &&
        {
            return {std::move(out), std::move(value)};
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
#include <array>
#include <cassert>
#include <numeric>
#include <ranges>

int main()
{
    std::array<int, 4> a{};
    constexpr std::array expected{2, 3, 4, 5};
    const auto result = std::ranges::iota(a, 2);
    assert(std::ranges::distance(a.cbegin(), result.out) == 4);
    assert(result.value == 6);
    assert(a == expected);
}
```

### See also

- **pair** — implements binary tuple, i.e. a pair of values (class template)
- **tuple (C++11)** — implements fixed size container, which holds elements of
  possibly different types (class template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/return_types/out_value_result*
