# std::ranges::in_out_out_result

```cpp
template< class I, class O1, class O2 >
struct in_out_out_result;  // (since C++20)
```

`ranges::in_out_out_result` is a class template that provides a way to store
three iterators as a single unit.

This class template has no base classes or declared members other than those
shown below. Thus it is suitable for use with structured bindings.

All special member functions of this class template are implicitly declared,
which makes specializations be aggregate classes, and propagate triviality,
potentially-throwing-ness, and constexpr-ness of corresponding operations on
data members.

### Template parameters

- **I, O1, O2** — the types of the objects that the `ranges::in_out_out_result`
  stores.

### Data members

## std::ranges::in_out_out_result::in

```cpp
[[no_unique_address]] I in;
```

a value (that is supposed to be an iterator)

## std::ranges::in_out_out_result::out1

```cpp
[[no_unique_address]] O1 out1;
```

a value (that is supposed to be an iterator)

## std::ranges::in_out_out_result::out2

```cpp
[[no_unique_address]] O2 out2;
```

a value (that is supposed to be an iterator)

### Member functions

## std::ranges::in_out_out_result::operator in_out_out_result<II, OO1, OO2>

```cpp
template<class II, class OO1, class OO2>
requires std::convertible_to<const I&, II> &&
         std::convertible_to<const O1&, OO1> &&
         std::convertible_to<const O2&, OO2>
constexpr operator in_out_out_result<II, OO1, OO2>() const &;  // (1)
template<class II, class OO1, class OO2>
requires std::convertible_to<I, II> &&
         std::convertible_to<O1, OO1> &&
         std::convertible_to<O2, OO2>
constexpr operator in_out_out_result<II, OO1, OO2>() &&;  // (2)
```

Converts `*this` to the result by constructing every data member of the result
from the corresponding member of `*this`.

1) Equivalent to `return {in, out1, out2};`.

2) Equivalent to `return {std::move(in), std::move(out1), std::move(out2)};`.

### Standard library

The following standard library functions use `ranges::in_out_out_result` as the
return type:

**Algorithm functions**

- **ranges::partition_copy (C++20)** — copies a range dividing the elements into
  two groups (niebloid)

### Synopsis

```cpp
namespace std::ranges
{
    template<class I, class O1, class O2>
    struct in_out_out_result
    {
        [[no_unique_address]] I  in;
        [[no_unique_address]] O1 out1;
        [[no_unique_address]] O2 out2;

        template<class II, class OO1, class OO2>
        requires std::convertible_to<const I&, II> &&
                 std::convertible_to<const O1&, OO1> &&
                 std::convertible_to<const O2&, OO2>
        constexpr operator in_out_out_result<II, OO1, OO2>() const &
        {
            return {in, out1, out2};
        }

        template<class II, class OO1, class OO2>
        requires std::convertible_to<I, II> &&
                 std::convertible_to<O1, OO1> &&
                 std::convertible_to<O2, OO2>
        constexpr operator in_out_out_result<II, OO1, OO2>() &&
        {
            return {std::move(in), std::move(out1), std::move(out2)};
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
#include <iterator>
#include <ranges>
#include <string_view>

void print(std::string_view rem, auto first, auto last)
{
    for (std::cout << rem << ": { "; first != last; ++first)
        std::cout << *first << ' ';
    std::cout << "}\n";
}

int main()
{
    constexpr std::string_view in {"TvEeNcStOoRr"};
    std::array<char, in.size()> o1, o2;

    const auto result = std::ranges::partition_copy(in, o1.begin(), o2.begin(),
        [](char c) { return std::isupper(c); });

    print("in", in.begin(), result.in);
    print("o1", o1.begin(), result.out1);
    print("o2", o2.begin(), result.out2);
}
```

Output:

```text
in: { T v E e N c S t O o R r }
o1: { T E N S O R }
o2: { v e c t o r }
```

### See also

- **pair** — implements binary tuple, i.e. a pair of values (class template)
- **tuple (C++11)** — implements fixed size container, which holds elements of
  possibly different types (class template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/return_types/in_out_out_result*
