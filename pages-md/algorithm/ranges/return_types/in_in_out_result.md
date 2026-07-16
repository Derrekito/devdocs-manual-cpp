# std::ranges::in_in_out_result

```cpp
template< class I1, class I2, class O >
struct in_in_out_result;  // (since C++20)
```

`ranges::in_in_out_result` is a class template that provides a way to store
three iterators as a single unit.

This class template has no base classes or declared members other than those
shown below. Thus it is suitable for use with structured bindings.

All special member functions of this class template are implicitly declared,
which makes specializations be aggregate classes, and propagate triviality,
potentially-throwing-ness, and constexpr-ness of corresponding operations on
data members.

### Template parameters

- **I1, I2, O** — the types of the iterators that the `ranges::in_in_out_result`
  stores.

### Data members

## std::ranges::in_in_out_result::in1

```cpp
[[no_unique_address]] I1 in1;
```

a value (that is supposed to be an iterator)

## std::ranges::in_in_out_result::in2

```cpp
[[no_unique_address]] I2 in2;
```

a value (that is supposed to be an iterator)

## std::ranges::in_in_out_result::out

```cpp
[[no_unique_address]] O out;
```

a value (that is supposed to be an iterator)

### Member functions

## std::ranges::in_in_out_result::operator in_in_out_result<II1, II2, OO>

```cpp
template<class II1, class II2, class OO>
requires std::convertible_to<const I1&, II1> &&
         std::convertible_to<const I2&, II2> &&
         std::convertible_to<const O&, OO>
constexpr operator in_in_out_result<II1, II2, OO>() const &;  // (1)
template<class II1, class II2, class OO>
requires std::convertible_to<I1, II1> &&
         std::convertible_to<I2, II2> &&
         std::convertible_to<O, OO>
constexpr operator in_in_out_result<II1, II2, OO>() &&;  // (2)
```

Converts `*this` to the result by constructing every data member of the result
from the corresponding member of `*this`.

1) Equivalent to `return {in1, in2, out};`.

2) Equivalent to `return {std::move(in1), std::move(in2), std::move(out)};`.

### Standard library

The following standard library functions use `ranges::in_in_out_result` as the
return type:

**Algorithm functions**

- **ranges::transform (C++20)** — applies a function to a range of elements
  (niebloid)
- **ranges::merge (C++20)** — merges two sorted ranges (niebloid)
- **ranges::set_union (C++20)** — computes the union of two sets (niebloid)
- **ranges::set_intersection (C++20)** — computes the intersection of two sets
  (niebloid)
- **ranges::set_symmetric_difference (C++20)** — computes the symmetric
  difference between two sets (niebloid)

### Synopsis

```cpp
namespace std::ranges
{
    template<class I1, class I2, class O>
    struct in_in_out_result
    {
        [[no_unique_address]] I1 in1;
        [[no_unique_address]] I2 in2;
        [[no_unique_address]] O  out;

        template<class II1, class II2, class OO>
        requires std::convertible_to<const I1&, II1> &&
                 std::convertible_to<const I2&, II2> &&
                 std::convertible_to<const O&, OO>
        constexpr operator in_in_out_result<II1, II2, OO>() const &
        {
            return {in1, in2, out};
        }

        template<class II1, class II2, class OO>
        requires std::convertible_to<I1, II1> &&
                 std::convertible_to<I2, II2> &&
                 std::convertible_to<O, OO>
        constexpr operator in_in_out_result<II1, II2, OO>() &&
        {
            return {std::move(in1), std::move(in2), std::move(out)};
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
#include <iostream>
#include <iterator>
#include <ranges>

void print(auto rem, auto first, auto last)
{
    for (std::cout << rem << ": "; first != last; ++first)
        std::cout << *first << ' ';
    std::cout << '\n';
}

int main()
{
    constexpr static auto in1 = {1, 2, 3, 4, 5, 5};
    constexpr static auto in2 = {3, 4, 5, 6, 7};
    std::array<int, std::size(in1) + std::size(in2)> out;

    const auto result = std::ranges::merge(in1, in2, out.begin());
    print("in1", in1.begin(), result.in1);
    print("in2", in2.begin(), result.in2);
    print("out", out.begin(), result.out);
}
```

Output:

```text
in1: 1 2 3 4 5 5
in2: 3 4 5 6 7
out: 1 2 3 3 4 4 5 5 5 6 7
```

### See also

- **pair** — implements binary tuple, i.e. a pair of values (class template)
- **tuple (C++11)** — implements fixed size container, which holds elements of
  possibly different types (class template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/return_types/in_in_out_result*
