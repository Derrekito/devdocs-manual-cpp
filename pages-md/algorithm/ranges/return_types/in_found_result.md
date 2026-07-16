# std::ranges::in_found_result

```cpp
template< class I >
struct in_found_result;  // (since C++20)
```

`ranges::in_found_result` is a class template that provides a way to store an
iterator and a boolean flag as a single unit.

This class template has no base classes or declared members other than those
shown below. Thus it is suitable for use with structured bindings.

All special member functions of this class template are implicitly declared,
which makes specializations be aggregate classes, and propagate triviality,
potentially-throwing-ness, and constexpr-ness of corresponding operations on
data members.

### Template parameters

- **I** — the type of the iterator that the `ranges::in_found_result` stores.

### Data members

## std::ranges::in_found_result::in

```cpp
[[no_unique_address]] I in;
```

a value (that is supposed to be an iterator)

## std::ranges::in_found_result::found

```cpp
bool found;
```

a boolean flag (that may show whether an appropriate range can be found)

### Member functions

## std::ranges::in_found_result::operator in_found_result<I2>

```cpp
template<class I2>
requires std::convertible_to<const I&, I2>
constexpr operator in_found_result<I2>() const &;  // (1)
template<class I2>
requires std::convertible_to<I, I2>
constexpr operator in_found_result<I2>() &&;  // (2)
```

Converts `*this` to the result by constructing every data member of the result
from the corresponding member of `*this`.

1) Equivalent to `return {in, found};`.

2) Equivalent to `return {std::move(in), found};`.

### Standard library

The following standard library functions use `ranges::in_found_result` as the
return type:

**Algorithm functions**

- **ranges::next_permutation (C++20)** — generates the next greater
  lexicographic permutation of a range of elements (niebloid)
- **ranges::prev_permutation (C++20)** — generates the next smaller
  lexicographic permutation of a range of elements (niebloid)

### Synopsis

```cpp
namespace std::ranges
{
    template<class I>
    struct in_found_result
    {
        [[no_unique_address]] I in;
        bool found;

        template<class I2>
        requires std::convertible_to<const I&, I2>
        constexpr operator in_found_result<I2>() const &
        {
            return {in, found};
        }

        template<class I2>
        requires std::convertible_to<I, I2>
        constexpr operator in_found_result<I2>() &&
        {
            return {std::move(in), found};
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
#include <iterator>
#include <ranges>

int main()
{
    int v[] {1, 2, 3};

    const auto result = std::ranges::next_permutation(v);

    std::ranges::for_each(std::cbegin(v), result.in, [](int e) {std::cout << e << ' ';});

    std::cout << std::boolalpha << "\n" "result.found: " << result.found << '\n';
}
```

Output:

```text
1 3 2
result.found = true
```

### See also

- **pair** — implements binary tuple, i.e. a pair of values (class template)
- **tuple (C++11)** — implements fixed size container, which holds elements of
  possibly different types (class template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/return_types/in_found_result*
