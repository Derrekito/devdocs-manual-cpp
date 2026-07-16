# std::ranges::borrowed_range, std::ranges::enable_borrowed_range

```cpp
template<class R>
concept borrowed_range =
    ranges::range<R> &&
    (std::is_lvalue_reference_v<R> ||
     ranges::enable_borrowed_range<std::remove_cvref_t<R>>);  // (1) (since C++20)
template<class R>
inline constexpr bool enable_borrowed_range = false;  // (2) (since C++20)
```

1) The concept `borrowed_range` defines the requirements of a range such that a
   function can take it by value and return iterators obtained from it without
   danger of dangling.

2) The `enable_borrowed_range` variable template is used to indicate whether a
   `range` is a `borrowed_range`. The primary template is defined as `false`.

### Semantic requirements

Let `U` be `std::remove_reference_t<T>` if `T` is an rvalue reference type, and
`T` otherwise. Given a variable `u` of type `U`, `T` models `borrowed_range`
only if the validity of iterators obtained from `u` is not tied to the lifetime
of that variable.

### Specializations

Specializations of `enable_borrowed_range` for all specializations of the
following standard templates are defined as `true`:

- `std::basic_string_view`
- `std::span`
- `std::ranges::subrange`
- `std::ranges::ref_view`
- `std::ranges::empty_view`
- `std::ranges::iota_view`

Specialization of `enable_borrowed_range` for the following standard range
adaptors are defined as `true` if and only if
`std::ranges::enable_borrowed_range<V>` is `true`, where `V` is the underlying
view type:

- `std::ranges::owning_view`
- `std::ranges::take_view`
- `std::ranges::drop_view`
- `std::ranges::drop_while_view`
- `std::ranges::common_view`
- `std::ranges::reverse_view`
- `std::ranges::elements_view`

- `std::ranges::adjacent_view`
*(since C++23)*

Specialization for `std::ranges::zip_view` is defined as `true` if and only if
`(std::ranges::enable_borrowed_range<Vs> && ...)` is `true`, where `Vs...` are
all view types it adapts.
*(since C++23)*

Users may specialize `enable_borrowed_range` to `true` for cv-unqualified
program-defined types which model `borrowed_range`, and `false` for types which
do not. Such specializations shall be usable in constant expressions and have
type `const bool`.

### Example

Demonstrates the specializations of `enable_borrowed_range` for program defined
types. Such specializations protect against potentially dangling results.

```cpp
#include <algorithm>
#include <array>
#include <cstddef>
#include <iostream>
#include <ranges>
#include <span>
#include <type_traits>

template <typename T, std::size_t N>
struct MyRange : std::array<T, N> { };

template <typename T, std::size_t N>
inline constexpr bool std::ranges::enable_borrowed_range<MyRange<T, N>> = false;

template <typename T, std::size_t N>
struct MyBorrowedRange : std::span<T, N> { };

template <typename T, std::size_t N>
inline constexpr bool std::ranges::enable_borrowed_range<MyBorrowedRange<T, N>> = true;

int main()
{
    static_assert(std::ranges::range<MyRange<int, 8>>);
    static_assert(std::ranges::borrowed_range<MyRange<int, 8>> == false);
    static_assert(std::ranges::range<MyBorrowedRange<int, 8>>);
    static_assert(std::ranges::borrowed_range<MyBorrowedRange<int, 8>> == true);

    auto getMyRangeByValue = [] { return MyRange<int, 4>{ {1, 2, 42, 3} }; };
    auto dangling_iter = std::ranges::max_element(getMyRangeByValue());
    static_assert(std::is_same_v<std::ranges::dangling, decltype(dangling_iter)>);
//  *dangling_iter; // compilation error (i.e. dangling protection works.)

    auto my = MyRange<int, 4>{ {1, 2, 42, 3} };
    auto valid_iter = std::ranges::max_element(my);
    std::cout << *valid_iter << ' '; // OK: 42

    auto getMyBorrowedRangeByValue = [] {
        static int sa[4] {1, 2, 42, 3};
        return MyBorrowedRange<int, std::size(sa)>{sa};
    };
    auto valid_iter2 = std::ranges::max_element(getMyBorrowedRangeByValue());
    std::cout << *valid_iter2 << '\n'; // OK: 42
}
```

Output:

```text
42 42
```

### See also

- **ranges::dangling (C++20)** — a placeholder type indicating that an iterator
  or a `subrange` should not be returned since it would be dangling (class)

---
*Source: https://en.cppreference.com/w/cpp/ranges/borrowed_range*
