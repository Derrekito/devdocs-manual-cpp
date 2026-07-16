# std::ranges::in_out_result

```cpp
template< class I, class O >
struct in_out_result;  // (since C++20)
```

`ranges::in_out_result` is a class template that provides a way to store two
iterators as a single unit.

This class template has no base classes or declared members other than those
shown below. Thus it is suitable for use with structured bindings.

All special member functions of this class template are implicitly declared,
which makes specializations be aggregate classes, and propagate triviality,
potentially-throwing-ness, and constexpr-ness of corresponding operations on
data members.

### Template parameters

- **I, O** — the types of the objects that the `ranges::in_out_result` stores.

### Data members

## std::ranges::in_out_result::in

```cpp
[[no_unique_address]] I in;
```

a value (that is supposed to be an iterator)

## std::ranges::in_out_result::out

```cpp
[[no_unique_address]] O out;
```

a value (that is supposed to be an iterator)

### Member functions

## std::ranges::in_out_result::operator in_out_result<I2, O2>

```cpp
template<class I2, class O2>
requires std::convertible_to<const I&, I2> && std::convertible_to<const O&, O2>
constexpr operator in_out_result<I2, O2>() const &;  // (1)
template<class I2, class O2>
requires std::convertible_to<I, I2> && std::convertible_to<O, O2>
constexpr operator in_out_result<I2, O2>() &&;  // (2)
```

Converts `*this` to the result by constructing every data member of the result
from the corresponding member of `*this`.

1) Equivalent to `return {in, out};`.

2) Equivalent to `return {std::move(in), std::move(out)};`.

### Standard library

The following standard library functions use `ranges::in_out_result` as the
return type:

**Algorithm functions**

- **ranges::copyranges::copy_if (C++20)(C++20)** — copies a range of elements to
  a new location (niebloid)
- **ranges::copy_n (C++20)** — copies a number of elements to a new location
  (niebloid)
- **ranges::copy_backward (C++20)** — copies a range of elements in backwards
  order (niebloid)
- **ranges::move (C++20)** — moves a range of elements to a new location
  (niebloid)
- **ranges::move_backward (C++20)** — moves a range of elements to a new
  location in backwards order (niebloid)
- **ranges::transform (C++20)** — applies a function to a range of elements
  (niebloid)
- **ranges::replace_copyranges::replace_copy_if (C++20)(C++20)** — copies a
  range, replacing elements satisfying specific criteria with another value
  (niebloid)
- **ranges::remove_copyranges::remove_copy_if (C++20)(C++20)** — copies a range
  of elements omitting those that satisfy specific criteria (niebloid)
- **ranges::unique_copy (C++20)** — creates a copy of some range of elements
  that contains no consecutive duplicates (niebloid)
- **ranges::reverse_copy (C++20)** — creates a copy of a range that is reversed
  (niebloid)
- **ranges::rotate_copy (C++20)** — copies and rotate a range of elements
  (niebloid)
- **ranges::partial_sort_copy (C++20)** — copies and partially sorts a range of
  elements (niebloid)
- **ranges::set_difference (C++20)** — computes the difference between two sets
  (niebloid)

**Memory functions**

- **ranges::uninitialized_copy (C++20)** — copies a range of objects to an
  uninitialized area of memory (niebloid)
- **ranges::uninitialized_copy_n (C++20)** — copies a number of objects to an
  uninitialized area of memory (niebloid)
- **ranges::uninitialized_move (C++20)** — moves a range of objects to an
  uninitialized area of memory (niebloid)
- **ranges::uninitialized_move_n (C++20)** — moves a number of objects to an
  uninitialized area of memory (niebloid)

### Synopsis

```cpp
namespace std::ranges
{
    template<class I, class O>
    struct in_out_result
    {
        [[no_unique_address]] I in;
        [[no_unique_address]] O out;

        template<class I2, class O2>
        requires std::convertible_to<const I&, I2> && std::convertible_to<const O&, O2>
        constexpr operator in_out_result<I2, O2>() const &
        {
            return {in, out};
        }

        template<class I2, class O2>
        requires std::convertible_to<I, I2> && std::convertible_to<O, O2>
        constexpr operator in_out_result<I2, O2>() &&
        {
            return {std::move(in), std::move(out)};
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
#include <cctype>
#include <iostream>
#include <ranges>

int main()
{
    constexpr char in[] = "transform" "\n";
    std::array<char, sizeof(in)> out;

    const auto result = std::ranges::transform(in, out.begin(),
        [](char c) { return std::toupper(c); });

    auto print = [](char c) { std::cout << c; };
    std::ranges::for_each(std::cbegin(in), result.in, print);
    std::ranges::for_each(out.cbegin(), result.out, print);
}
```

Output:

```text
transform
TRANSFORM
```

### See also

- **pair** — implements binary tuple, i.e. a pair of values (class template)
- **tuple (C++11)** — implements fixed size container, which holds elements of
  possibly different types (class template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/return_types/in_out_result*
