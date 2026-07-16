# std::ranges::views::stride, std::ranges::stride_view

```cpp
template< ranges::input_range V >
    requires ranges::view<V>
class stride_view
    : public ranges::view_interface<stride_view<V>>  // (1) (since C++23)
namespace views {
    inline constexpr /* unspecified */ stride = /* unspecified */;
}  // (2) (since C++23)
Call signature
template< ranges::viewable_range R >
constexpr ranges::view auto stride( R&& r, ranges::range_difference_t<R> n );  // (since C++23)
template< class DifferenceType >
constexpr /*range adaptor closure*/ stride( DifferenceType&& n );  // (since C++23)
Helper templates
```

1) `stride_view` is a range adaptor that takes a `view` and a number `n` and
   produces a view, that consists of elements of the original view by advancing
   over *n* elements at a time. This means that each `m`*th* element of the
   produced view is `(n * i)`th element of the original view, for some
   non-negative index `i`. The elements of the original view, whose "index" is
   not a multiple of `n`, are not present in the produced view.
Let `S` be the size of the original view. Then the size of produced view is:

- `(S / n) + (S % n ? 1 : 0)`, if `S >= n`; otherwise,
- `1`, if `S > 0`; otherwise,
- `​0​`, and the resulting view is empty.

2) The name `views::stride` denotes a RangeAdaptorObject. Given subexpressions
   `e` and `n`, the expression `views::stride(e, n)` is expression-equivalent to
   `stride_view(e, n)`.

The `n` must be greater than `​0​`, otherwise the behavior is undefined.

`stride_view` always models `input_range`, and models `forward_range`,
`bidirectional_range`, `random_access_range`, and/or `sized_range`, if adapted
`view` type `V` models the corresponding concept. `stride_view<V>` models
`common_range` whenever the underlying view `V` does.

### Data members

- **`base_` (private)** — The underlying `view` of type `V`. (exposition-only
  member object*)
- **`stride_` (private)** — The size object (the "stride") of type
  `ranges::range_difference_t<V>`. (exposition-only member object*)

### Member functions

- **(constructor) (C++23)** — constructs a `stride_view` (public member
  function)
- **stride (C++23)** — returns the stored stride value (public member function)
- **base (C++23)** — returns a copy of the underlying (adapted) view (public
  member function)
- **begin (C++23)** — returns an iterator to the beginning (public member
  function)
- **end (C++23)** — returns an iterator or a sentinel to the end (public member
  function)
- **size (C++23)** — returns the number of elements. Provided only if the
  underlying (adapted) range satisfies `sized_range`. (public member function)

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

### Nested classes

- ***iterator* (C++23)** — the iterator type (exposition-only member class
  template*)

### Helper templates

```cpp
template< class V >
inline constexpr bool ranges::enable_borrowed_range<stride_view<V>> =
    ranges::enable_borrowed_range<V>;  // (since C++23)
```

This specialization of `ranges::enable_borrowed_range` makes `stride_view`
satisfy `borrowed_range` when the underlying view satisfies it.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_ranges_stride` | 202207L | (C++23) | `std::ranges::stride_view`

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <ranges>
#include <string_view>
using namespace std::literals;

void print(std::ranges::viewable_range auto&& v, std::string_view separator = " ")
{
    for (auto const& x : v)
        std::cout << x << separator;
    std::cout << '\n';
}

int main()
{
    print(std::views::iota(1, 13) | std::views::stride(3));
    print(std::views::iota(1, 13) | std::views::stride(3) | std::views::reverse);
    print(std::views::iota(1, 13) | std::views::reverse | std::views::stride(3));

    print("0x0!133713337*x//42/A$@"sv | std::views::stride(0B11) |
          std::views::transform([](char O) -> char { return 0100 | O; }),
          "");
}
```

Output:

```text
1 4 7 10
10 7 4 1
12 9 6 3
password
```

### References

- C++23 standard (ISO/IEC 14882:2023):

### See also

- **ranges::slide_viewviews::slide (C++23)** — a `view` whose Mth element is a
  `view` over the Mth through (M + N - 1)th elements of another `view` (class
  template) (range adaptor object)
- **ranges::chunk_viewviews::chunk (C++23)** — a range of `view`s that are
  `N`-sized non-overlapping successive chunks of the elements of another `view`
  (class template) (range adaptor object)
- **ranges::adjacent_viewviews::adjacent (C++23)** — a `view` consisting of
  tuples of references to adjacent elements of the adapted view (class template)
  (range adaptor object)
- **ranges::filter_viewviews::filter (C++20)** — a `view` that consists of the
  elements of a `range` that satisfies a predicate (class template) (range
  adaptor object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/stride_view*
