# std::ranges::views::take_while, std::ranges::take_while_view

```cpp
template< ranges::view V, class Pred >
    requires ranges::input_range<V> &&
             std::is_object_v<Pred> &&
             std::indirect_unary_predicate<const Pred, ranges::iterator_t<V>>
class take_while_view
    : public ranges::view_interface<take_while_view<V, Pred>>  // (1) (since C++20)
namespace views {
    inline constexpr /*unspecified*/ take_while = /*unspecified*/;
}  // (2) (since C++20)
Call signature
template< ranges::viewable_range R, class Pred >
    requires /* see below */
constexpr ranges::view auto take_while( R&& r, Pred&& pred );  // (since C++20)
template< class Pred >
constexpr /*range adaptor closure*/ take_while( Pred&& pred );  // (since C++20)
```

1) A range adaptor that represents `view` of the elements from an underlying
   sequence, starting at the beginning and ending at the first element for which
   the predicate returns false.

2) RangeAdaptorObject. The expression `views::take_while(e, f)` is
   expression-equivalent to `take_while_view(e, f)` for any suitable
   subexpressions `e` and `f`.

`take_while_view` models the concepts `contiguous_range`, `random_access_range`,
`bidirectional_range`, `forward_range`, and `input_range` when the underlying
view `V` models respective concepts.

### Member functions

- **(constructor) (C++20)** — constructs a `take_while_view` (public member
  function)
- **base (C++20)** — returns a copy of the underlying (adapted) view (public
  member function)
- **pred (C++20)** — returns a reference to the stored predicate (public member
  function)
- **begin (C++20)** — returns an iterator to the beginning (public member
  function)
- **end (C++20)** — returns a sentinel representing the end (public member
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
- **front (C++20)** — returns the first element in the derived view. Provided if
  it satisfies `forward_range`. (public member function of
  `std::ranges::view_interface<D>`)
- **operator[] (C++20)** — returns the nth element in the derived view. Provided
  if it satisfies `random_access_range`. (public member function of
  `std::ranges::view_interface<D>`)

### Deduction guides

### Nested classes

- ***sentinel*** — the sentinel type (exposition-only member class template*)

### Example

```cpp
#include <iostream>
#include <ranges>

int main()
{
    for (int year : std::views::iota(2020)
                  | std::views::take_while([](int y) { return y <= 2023; }))
        std::cout << year << ' ';
    std::cout << '\n';

    const char idea[] {"Today is yesterday's tomorrow!..."};
    for (char x : std::ranges::take_while_view(idea, [](char c) { return c != '.'; }))
        std::cout << x;
    std::cout << '\n';
}
```

Output:

```text
2020 2021 2022 2023
Today is yesterday's tomorrow!
```

### See also

- **ranges::take_viewviews::take (C++20)** — a `view` consisting of the first N
  elements of another `view` (class template) (range adaptor object)
- **ranges::drop_while_viewviews::drop_while (C++20)** — a `view` consisting of
  the elements of another `view`, skipping the initial subsequence of elements
  until the first element where the predicate returns `false` (class template)
  (range adaptor object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/take_while_view*
