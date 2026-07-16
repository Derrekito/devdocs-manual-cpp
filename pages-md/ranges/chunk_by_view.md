# std::ranges::views::chunk_by, std::ranges::chunk_by_view

```cpp
template< ranges::forward_range V, std::indirect_binary_predicate<iterator_t<V>,
          ranges::iterator_t<V>> Pred >
    requires ranges::view<V> && std::is_object_v<Pred>
class chunk_by_view
    : public ranges::view_interface<chunk_by_view<V, Pred>>  // (1) (since C++23)
namespace views {
    inline constexpr /* unspecified */ chunk_by = /* unspecified */ ;
}  // (2) (since C++23)
Call signature
template< ranges::viewable_range R, class Pred >
    requires /* see below */
constexpr ranges::view auto chunk_by( R&& r, Pred&& pred );  // (since C++23)
template< class Pred >
constexpr /*range adaptor closure*/ chunk_by( Pred&& pred );  // (since C++23)
```

1) `chunk_by_view` is a range adaptor that takes a `view` and an invocable
   object `pred` (the binary predicate), and produces a `view` of subranges
   (chunks), by splitting the underlying view between each pair of adjacent
   elements for which `pred` returns `false`. The first element of each such
   pair belongs to the previous chunk, and the second element belongs to the
   next chunk.

2) The name `views::chunk_by` denotes a RangeAdaptorObject. Given a
   subexpression `e` and `f`, the expression `views::chunk_by(e, f)` is
   expression-equivalent to `chunk_by_view(e, f)`.

`chunk_by_view` always models `forward_range`, and models `bidirectional_range`
and/or `common_range`, if adapted `view` type models the corresponding concepts.
`chunk_by_view` never models `borrowed_range` or `sized_range`.

### Data members

- **`base_` (private)** — The underlying `view` of type `V`. (exposition-only
  member object*)
- **`pred_` (private)** — An object of type `movable-box<Pred>` that wraps the
  predicate used to split the elements of `base_`. (exposition-only member
  object*)
- **`begin_` (private)** — An *optional-like* object that caches the iterator to
  the first element. (exposition-only member object*)

### Member functions

- **(constructor) (C++23)** — constructs a `chunk_by_view` (public member
  function)
- **base (C++23)** — returns a copy of the underlying (adapted) view (public
  member function)
- **pred (C++23)** — returns a reference to the stored predicate (public member
  function)
- **begin (C++23)** — returns an iterator to the beginning (public member
  function)
- **end (C++23)** — returns an iterator or a sentinel to the end (public member
  function)
- ****find_next** (C++23)** — returns an iterator to the begin of the next
  subrange (exposition-only member function*)
- ****find_prev** (C++23)** — returns an iterator to the begin of the previous
  subrange (exposition-only member function*)

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

### Deduction guides

### Nested classes

- ***iterator* (C++23)** — the iterator type (exposition-only member class
  template*)

### Notes

In order to provide the amortized constant time complexity required by the
`range` concept, the result of `begin()` is cached within the `chunk_by_view`
object. If the underlying range is modified after the first call to `begin()`,
subsequent uses of the `chunk_by_view` object might have unintuitive behavior.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_ranges_chunk_by` | 202202L | (C++23) | `std::ranges::chunk_by_view`

### Example

```cpp
#include <functional>
#include <iostream>
#include <ranges>
#include <string_view>
#include <vector>

void print_chunks(auto view, std::string_view separator = ", ")
{
    for (auto const subrange : view)
    {
        std::cout << '[';
        for (std::string_view prefix; auto const& elem : subrange)
            std::cout << prefix << elem, prefix = separator;
        std::cout << "] ";
    }
    std::cout << '\n';
}

int main()
{
    {
        auto v = std::vector{1, 2, 3, 1, 2, 3, 3, 3, 1, 2, 3};
        auto fun = std::ranges::less{};
        auto view = v | std::views::chunk_by(fun);
        print_chunks(view);
    }
    {
        auto v = std::vector{1, 2, 3, 4, 4, 0, 2, 3, 3, 3, 2, 1};
        auto fun = std::not_fn(std::ranges::equal_to{}); // or ranges::not_equal_to
        auto view = v | std::views::chunk_by(fun);
        print_chunks(view);
    }
    {
        std::string v = "__cpp_lib_ranges_chunk_by";
        auto fun = [](char x, char y) { return not(x == '_' or y == '_'); };
        auto view = v | std::views::chunk_by(fun);
        print_chunks(view, "");
    }
}
```

Output:

```text
[1, 2, 3] [1, 2, 3] [3] [3] [1, 2, 3]
[1, 2, 3, 4] [4, 0, 2, 3] [3] [3, 2, 1]
[_] [_] [cpp] [_] [lib] [_] [ranges] [_] [chunk] [_] [by]
```

### References

- C++23 standard (ISO/IEC 14882:2023):

### See also

- **ranges::chunk_viewviews::chunk (C++23)** — a range of `view`s that are
  `N`-sized non-overlapping successive chunks of the elements of another `view`
  (class template) (range adaptor object)
- **ranges::slide_viewviews::slide (C++23)** — a `view` whose Mth element is a
  `view` over the Mth through (M + N - 1)th elements of another `view` (class
  template) (range adaptor object)
- **ranges::stride_viewviews::stride (C++23)** — a `view` consisting of elements
  of another `view`, advancing over N elements at a time (class template) (range
  adaptor object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_by_view*
