# std::ranges::views::drop_while, std::ranges::drop_while_view

```cpp
template< ranges::view V, class Pred >
    requires ranges::input_range<V> &&
             std::is_object_v<Pred> &&
             std::indirect_unary_predicate<const Pred, ranges::iterator_t<V>>
class drop_while_view
    : public ranges::view_interface<drop_while_view<V, Pred>>  // (1) (since C++20)
namespace views {
    inline constexpr /* unspecified */ drop_while = /* unspecified */;
}  // (2) (since C++20)
Call signature
template< ranges::viewable_range R, class Pred >
    requires /* see below */
constexpr ranges::view auto drop_while( R&& r, Pred&& pred );  // (since C++20)
template< class Pred >
constexpr /*range adaptor closure*/ drop_while( Pred&& pred );  // (since C++20)
```

1) A range adaptor that represents `view` of elements from an underlying
   sequence, beginning at the first element for which the predicate returns
   `false`.

2) RangeAdaptorObject. The expression `views::drop_while(e, f)` is
   expression-equivalent to `drop_while_view(e, f)` for any suitable
   subexpressions `e` and `f`.

`drop_while_view` models the concepts `contiguous_range`, `random_access_range`,
`bidirectional_range`, `forward_range`, `input_range`, and `common_range` when
the underlying view `V` models respective concepts. It also models `sized_range`
if `ranges::forward_range<V>` and
`std::sized_sentinel_for<ranges::sentinel_t<V>, ranges::iterator_t<V>>` are
modeled.

### Member functions

- **(constructor) (C++20)** — constructs a `drop_while_view` (public member
  function)
- **base (C++20)** — returns a copy of the underlying (adapted) view (public
  member function)
- **pred (C++20)** — returns a reference to the stored predicate (public member
  function)
- **begin (C++20)** — returns an iterator to the beginning (public member
  function)
- **end (C++20)** — returns an iterator or a sentinel to the end (public member
  function)

**Inherited from `std::ranges::view_interface`**

- **empty (C++20)** — returns whether the derived view is empty. Provided if it
  satisfies `sized_range` or `forward_range`. (public member function of
  `std::ranges::view_interface<D>`)
- **cbegin (C++23)** — returns a constant iterator to the beginning of the
  range. (public member function of `std::ranges::view_interface<D>`)
- **cend (C++23)** — returns a sentinel for the constant iterator of the range.
  (public member function of `std::ranges::view_interface<D>`)
- **operator bool (C++20)** — returns whether the derived view is not empty.
  Provided if `ranges::empty` is applicable to it. (public member function of
  `std::ranges::view_interface<D>`)
- **data (C++20)** — gets the address of derived view's data. Provided if its
  iterator type satisfies `contiguous_iterator`. (public member function of
  `std::ranges::view_interface<D>`)
- **size (C++20)** — returns the number of elements in the derived view.
  Provided if it satisfies `forward_range` and its sentinel and iterator type
  satisfy `sized_sentinel_for`. (public member function of
  `std::ranges::view_interface<D>`)
- **front (C++20)** — returns the first element in the derived view. Provided if
  it satisfies `forward_range`. (public member function of
  `std::ranges::view_interface<D>`)
- **back (C++20)** — returns the last element in the derived view. Provided if
  it satisfies `bidirectional_range` and `common_range`. (public member function
  of `std::ranges::view_interface<D>`)
- **operator[] (C++20)** — returns the nth element in the derived view. Provided
  if it satisfies `random_access_range`. (public member function of
  `std::ranges::view_interface<D>`)

### Deduction guides

### Helper templates

```cpp
template< class T, class Pred >
inline constexpr bool enable_borrowed_range<std::ranges::drop_while_view<T, Pred>> =
    std::ranges::enable_borrowed_range<T>;  // (since C++20)
```

This specialization of `std::ranges::enable_borrowed_range` makes
`drop_while_view` satisfy `borrowed_range` when the underlying view satisfies
it.

### Notes

In order to provide the amortized constant time complexity required by the
`range` concept, the result of `begin` is cached within the `drop_while_view`
object. If the underlying range is modified after the first call to `begin()`,
subsequent uses of the `drop_while_view` object might have unintuitive behavior.

### Example

```cpp
#include <algorithm>
#include <iomanip>
#include <iostream>
#include <ranges>
#include <string>
#include <string_view>

using std::operator""sv;

[[nodiscard]]
constexpr bool is_space(char q) noexcept
{
    static constexpr auto ws = {' ', '\t', '\n', '\v', '\r', '\f'};
    return std::ranges::any_of(ws, [q](auto p) { return p == q; });
};

[[nodiscard]]
constexpr std::string trim_left(std::string_view const in) noexcept
{
    auto view = in | std::views::drop_while(is_space);
    return {view.begin(), view.end()};
}

[[nodiscard]]
constexpr std::string trim(std::string_view const in)
{
    auto view = in
              | std::views::drop_while(is_space) | std::views::reverse
              | std::views::drop_while(is_space) | std::views::reverse
              ;
    return {view.begin(), view.end()};
}

int main()
{
    static_assert(trim_left(" \n C++23") == "C++23"sv);
    constexpr std::string_view src = " \f\n\t\r\vHello, C++20!\f\n\t\r\v ";

    const auto s = trim(src);
    std::cout << "s = " << std::quoted(s) << '\n';

    static constexpr auto v = {0, 1, 2, 3, 4, 5};
    for (int n : v | std::views::drop_while([](int i) { return i < 3; }))
        std::cout << n << ' ';
    std::cout << '\n';
}
```

Output:

```text
s = "Hello, C++20!"
3 4 5
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3494 | C++20 | `drop_while_view` was never a `borrowed_range` | it is a
      `borrowed_range` if its underlying view is

### See also

- **ranges::drop_viewviews::drop (C++20)** — a `view` consisting of elements of
  another `view`, skipping the first N elements (class template) (range adaptor
  object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/drop_while_view*
