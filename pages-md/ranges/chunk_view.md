# std::ranges::views::chunk, std::ranges::chunk_view

```cpp
template< ranges::view V >
    requires ranges::input_range<V>
class chunk_view
    : public ranges::view_interface<chunk_view<V>>  // (1) (since C++23)
template< ranges::view V >
    requires ranges::forward_range<V>
class chunk_view<V>
    : public ranges::view_interface<chunk_view<V>>  // (2) (since C++23)
namespace views {
    inline constexpr /* unspecified */ chunk = /* unspecified */;
}  // (3) (since C++23)
Call signature
template< ranges::viewable_range R >
constexpr ranges::view auto chunk( R&& r, ranges::range_difference_t<R> n );  // (since C++23)
template< class DifferenceType >
constexpr /*range adaptor closure*/ chunk( DifferenceType&& n );  // (since C++23)
Helper templates
template< class I >
constexpr I __div_ceil( I num, I denom );  // (4) (exposition only*)
```

`chunk_view` takes a `view` and a number `n` and produces a range of views (the
*chunks*) of the original view, such that each *chunk*, except maybe the last
one, has the size `n`. These *chunks* are non-overlapping, successive sub-ranges
of the elements of the original view, in order.

Let `s` be the size of the original view. If `s` is not the multiple of `n`, the
size of the *last* produced view is exactly `s % n` (the remainder). Otherwise,
the size of each *chunk*, including the last one, is `n`.

The size of produced view is `__div_ceil(s)`.

If the `n` is not greater than `​0​` the behavior is undefined.

1) An implementation that supports the underlying view `V` that models only
   `input_range`.

2) A partial specialization that supports the underlying view `V` that models
   `forward_range` or stronger. Models `common_range` if the underlying view `V`
   is `forward_range`, `common_range`, and either `sized_range` or non
   `bidirectional_range`.

3) The name `views::chunk` denotes a RangeAdaptorObject. Given subexpressions
   `e` and `n`, the expression `views::chunk(e, n)` is expression-equivalent to
   `chunk_view(e, n)`.

4) Computes the smallest integer value that is not less than the quotient of
   dividing `num` by `denom`. Equivalent to: I r = num / denom; if (num % denom)
   ++r; return r;

### Data members

- **`base_` (private)** — The underlying `view` of type `V`. (exposition-only
  member object*)
- **`n_` (private)** — The "chunk size" of type `ranges::range_difference_t<V>`.
  (exposition-only member object*)

**If `V` models exactly `input_range` (1)**

- **`remainder_` (private) (optional)** — The number of elements left in the
  current chunk, of type `ranges::range_difference_t<V>`. (exposition-only
  member object*)
- **`current_` (private) (optional)** — An *optional-like* object of type
  /*non-propagating-cache*/<ranges::iterator_t<V>> that caches current
  underlying iterator. (exposition-only member object*)

### Member functions

- **(constructor) (C++23)** — constructs a `chunk_view` (public member function)
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

- ***outer_iterator* (C++23)** — the output ("chunk-wise") iterator type when
  `V` models `input_range` (1) (exposition-only member class*)
- ***inner_iterator* (C++23)** — the inner ("element-wise") iterator type when
  `V` models `input_range` (1) (exposition-only member class*)
- ***iterator* (C++23)** — the iterator type when `V` models `forward_range` (2)
  (exposition-only member class template*)

### Helper templates

```cpp
template< class V >
inline constexpr bool ranges::enable_borrowed_range<chunk_view<V>> =
    ranges::forward_range<V> && ranges::enable_borrowed_range<V>;  // (since C++23)
```

This specialization of `ranges::enable_borrowed_range` makes `chunk_view`
satisfy `borrowed_range` when the underlying view `V` satisfies both, the
`forward_range` and the `borrowed_range`.

### Notes

If `V` models `input_range` (1), `chunk_view`'s iterator has a dedicated type:
`outer_iterator::value_type` that is itself an input view.

If `V` models `forward_range` or stronger (2), `chunk_view` defers to
`views::take` for its `value_type`.

If `V` models `bidirectional_range` or stronger ranges (2), the need to
calculate size the last chunk correctly (from the end iterator) requires the
underlying range type `V` to be `sized_range`.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_ranges_chunk` | 202202L | (C++23) | `std::ranges::chunk_view`

### Example

```cpp
#include <algorithm>
#include <iostream>
#include <ranges>

auto print_subrange = [](std::ranges::viewable_range auto&& r)
{
    std::cout << '[';
    for (int pos{}; auto elem : r)
        std::cout << (pos++ ? " " : "") << elem;
    std::cout << "] ";
};

int main()
{
    const auto v = {1, 2, 3, 4, 5, 6};

    for (const unsigned width : std::views::iota(1U, 2U + v.size()))
    {
        auto const chunks = v | std::views::chunk(width);
        std::cout << "chunk(" << width << "): ";
        std::ranges::for_each(chunks, print_subrange);
        std::cout << '\n';
    }
}
```

Output:

```text
chunk(1): [1] [2] [3] [4] [5] [6]
chunk(2): [1 2] [3 4] [5 6]
chunk(3): [1 2 3] [4 5 6]
chunk(4): [1 2 3 4] [5 6]
chunk(5): [1 2 3 4 5] [6]
chunk(6): [1 2 3 4 5 6]
chunk(7): [1 2 3 4 5 6]
```

### References

- C++23 standard (ISO/IEC 14882:2023):

### See also

- **ranges::chunk_by_viewviews::chunk_by (C++23)** — splits the `view` into
  subranges between each pair of adjacent elements for which the given predicate
  returns `false` (class template) (range adaptor object)
- **ranges::adjacent_viewviews::adjacent (C++23)** — a `view` consisting of
  tuples of references to adjacent elements of the adapted view (class template)
  (range adaptor object)
- **ranges::slide_viewviews::slide (C++23)** — a `view` whose Mth element is a
  `view` over the Mth through (M + N - 1)th elements of another `view` (class
  template) (range adaptor object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_view*
