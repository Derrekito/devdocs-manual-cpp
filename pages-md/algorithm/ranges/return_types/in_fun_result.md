# std::ranges::in_fun_result

```cpp
template< class I, class F >
struct in_fun_result;  // (since C++20)
```

`ranges::in_fun_result` is a class template that provides a way to store an
iterator and a function object as a single unit.

This class template has no base classes or declared members other than those
shown below. Thus it is suitable for use with structured bindings.

All special member functions of this class template are implicitly declared,
which makes specializations be aggregate classes, and propagate triviality,
potentially-throwing-ness, and constexpr-ness of corresponding operations on
data members.

### Template parameters

- **I** — the type of the iterator that the `ranges::in_fun_result` stores.
- **F** — the type of the function object that the `ranges::in_fun_result`
  stores.

### Data members

## std::ranges::in_fun_result::in

```cpp
[[no_unique_address]] I in;
```

a value (that is supposed to be an iterator)

## std::ranges::in_fun_result::fun

```cpp
[[no_unique_address]] F fun;
```

a value (that is supposed to be a function object)

### Member functions

## std::ranges::in_fun_result::operator in_fun_result<I2, F2>

```cpp
template<class I2, class F2>
requires std::convertible_to<const I&, I2> && std::convertible_to<const F&, F2>
constexpr operator in_fun_result<I2, F2>() const &;  // (1)
template<class I2, class F2>
requires std::convertible_to<I, I2> && std::convertible_to<F, F2>
constexpr operator in_fun_result<I2, F2>() &&;  // (2)
```

Converts `*this` to the result by constructing every data member of the result
from the corresponding member of `*this`.

1) Equivalent to `return {in, fun};`.

2) Equivalent to `return {std::move(in), std::move(fun)};`.

### Standard library

The following standard library functions use `ranges::in_fun_result` as the
return type:

**Algorithm functions**

- **ranges::for_each (C++20)** — applies a function to a range of elements
  (niebloid)
- **ranges::for_each_n (C++20)** — applies a function object to the first N
  elements of a sequence (niebloid)

### Synopsis

```cpp
namespace std::ranges
{
    template<class I, class F>
    struct in_fun_result
    {
        [[no_unique_address]] I in;
        [[no_unique_address]] F fun;

        template<class I2, class F2>
        requires std::convertible_to<const I&, I2> && std::convertible_to<const F&, F2>
        constexpr operator in_fun_result<I2, F2>() const &
        {
            return {in, fun};
        }

        template<class I2, class F2>
        requires std::convertible_to<I, I2> && std::convertible_to<F, F2>
        constexpr operator in_fun_result<I2, F2>() &&
        {
            return {std::move(in), std::move(fun)};
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

    const auto [last, negate] = std::ranges::for_each_n(v, std::size(v),
        [](int& x) { return x = -x; });

    const auto result = std::ranges::for_each(std::cbegin(v), last,
        [](int x) { std::cout << x << ' '; });
    std::cout << "│ ";

    std::ranges::for_each(v, negate);
    std::ranges::for_each(v, result.fun);
    std::cout << '\n';
}
```

Output:

```text
-1 -2 -3 │ 1 2 3
```

### See also

- **pair** — implements binary tuple, i.e. a pair of values (class template)
- **tuple (C++11)** — implements fixed size container, which holds elements of
  possibly different types (class template)

---
*Source: https://en.cppreference.com/w/cpp/algorithm/ranges/return_types/in_fun_result*
