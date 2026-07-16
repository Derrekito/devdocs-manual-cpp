# std::ranges::in_value_result

```cpp
template< class I, class T >
struct in_value_result;  // (since C++23)
```

`ranges::in_value_result` is a class template that provides a way to store an
iterator and a value as a single unit.

This class template has no base classes or declared members other than those
shown below. Thus it is suitable for use with structured bindings.

All special member functions of this class template are implicitly declared,
which makes specializations be aggregate classes, and propagate triviality,
potentially-throwing-ness, and constexpr-ness of corresponding operations on
data members.

### Template parameters

- **I, T** — the types of the objects that the `ranges::in_value_result` stores.

### Data members

## std::ranges::in_value_result::in

```cpp
[[no_unique_address]] I in;
```

a value (that is supposed to be an iterator)

## std::ranges::out_value_result::value

```cpp
[[no_unique_address]] T value;
```

a value (that is supposed to be a stored value)

### Member functions

## std::ranges::in_value_result::operator in_value_result<I2, T2>

```cpp
template<class I2, class T2>
requires convertible_to<const I&, I2> && convertible_to<const T&, T2>
constexpr operator in_value_result<I2, T2>() const &;  // (1)
template<class I2, class T2>
requires convertible_to<I, I2> && convertible_to<T, T2>
constexpr operator in_value_result<I2, T2>() &&;  // (2)
```

Converts `*this` to the result by constructing every data member of the result
from the corresponding member of `*this`.

1) Equivalent to `return {in, value};`.

2) Equivalent to `return {std::move(in), std::move(value)};`.

### Standard library

The following standard library functions use `ranges::in_value_result` as the
return type:

**Algorithm functions**

- **ranges::fold_left_with_iter (C++23)** — left-folds a range of elements, and
  returns a **pair** (iterator, value) (niebloid)
- **ranges::fold_left_first_with_iter (C++23)** — left-folds a range of elements
  using the first element as an initial value, and returns a **pair** (iterator,
  optional) (niebloid)

### Synopsis

```cpp
namespace std::ranges
{
    template<class I, class T>
    struct in_value_result
    {
        [[no_unique_address]] I in;
        [[no_unique_address]] T value;

        template<class I2, class T2>
        requires convertible_to<const I&, I2> && convertible_to<const T&, T2>
        constexpr operator in_value_result<I2, T2>() const &
        {
            return {in, value};
        }

        template<class I2, class T2>
        requires convertible_to<I, I2> && convertible_to<T, T2>
        constexpr operator in_value_result<I2, T2>() &&
        {
            return {std::move(in), std::move(value)};
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
#include <cassert>
#include <functional>

int main()
{
    const auto v = {1, 2, 3};
    const auto ret = std::ranges::fold_left_with_iter(v, 4, std::plus<>());
    assert(ret.in == v.end());
    assert(ret.value == 1 + 2 + 3 + 4);
}
```

### See also

- **pair** — implements binary tuple, i.e. a pair of values (class template)
- **tuple (C++11)** — implements fixed size container, which holds elements of
  possibly different types (class template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/return_types/in_value_result*
