# std::ranges::views::adjacent_transform, std::ranges::adjacent_transform_view, std::ranges::views::pairwise_transform

```cpp
template< ranges::forward_range V, std::move_constructible F, std::size_t N >
  requires ranges::view<V> && (N > 0) && std::is_object_v<F> &&
           std::regular_invocable<F&,
               /*REPEAT*/(ranges::range_reference_t<V>, N)...> &&
           /*can-reference*/<std::invoke_result_t<F&,
               /*REPEAT*/(ranges::range_reference_t<V>, N)...>>
class adjacent_transform_view
    : public ranges::view_interface<adjacent_transform_view<V, F, N>>  // (1) (since C++23)
namespace views {
template< std::size_t N >
    inline constexpr /* unspecified */ adjacent_transform = /* unspecified */;
}  // (2) (since C++23)
namespace views {
    inline constexpr auto pairwise_transform = adjacent_transform<2>;
}  // (3) (since C++23)
Call signature
template< ranges::viewable_range R, class F >
    requires /* see below */
constexpr ranges::view auto adjacent_transform<N>( R&& r, F&& fun );  // (since C++23)
template< class F >
constexpr /*range adaptor closure*/ adjacent_transform<N>( F&& fun );  // (since C++23)
```

1) `adjacent_transform_view` is a range adaptor that takes a `view` and an
   invocable object `fun`, and produces a `view` whose `i`th element is a value
   that is the result of applying `fun` to each element in `[``i``,``i + N``)`
   of the original view. `F` always has arity `N`.
Let `S` be the size of the original view. Then the size of produced view is:

- `S - N + 1`, if `S >= N`,
- `​0​` otherwise, and the resulting view is empty.

2) The name `views::adjacent_transform<N>` denotes a RangeAdaptorObject. Given
subexpressions `e` and `f`, and a constant expression `N`, the expression
`views::adjacent_transform<N>(e, f)` is expression-equivalent to:

- `((void)e, views::zip_transform(f))`, if `N` is equal to `​0​` (except that
  the evaluations of `e` and `f` are indeterminately sequenced),
- `adjacent_transform_view<views::all_t<decltype((e))>,
  std::decay_t<decltype((f))>, N>(e, f)` otherwise.

3) The name `views::pairwise_transform` denotes a RangeAdaptorObject that
   behaves exactly as `views::adjacent_transform<2>`. In particular, the arity
   of `F` is also `2` and `fun` is a binary invocable object.

`adjacent_transform_view` always models `forward_range`, and models
`bidirectional_range`, `random_access_range`, or `sized_range`, if adapted
`view` type models the corresponding concept.

### Member functions

- **(constructor) (C++23)** — constructs a `adjacent_transform_view` (public
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

(none)

### Member types

- **`InnerView` (private)** — `ranges::adjacent_view<V, N>`. (exposition-only
  member type*)
- **`inner_iterator` (private)** — `ranges::iterator_t<const InnerView>`, if
  `Const` is `true`. Otherwise, `ranges::iterator_t<InnerView>`.
  (exposition-only member type*)
- **`inner_sentinel` (private)** — `ranges::sentinel_t<const InnerView>`, if
  `Const` is `true`. Otherwise, `ranges::sentinel_t<InnerView>`.
  (exposition-only member type*)

### Data members

- **`fun_` (private)** — `/*movable-box*/<F>` (exposition-only member object*)
- **`inner_` (private)** — `ranges::adjacent_view<V,N>` (exposition-only member
  object*)

### Nested classes

- ***iterator* (C++23)** — the iterator type (exposition-only member class
  template*)
- ***sentinel* (C++23)** — the sentinel type used when `adjacent_transform_view`
  is not a `common_range` (exposition-only member class template*)

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_ranges_zip` | 202110L | (C++23) | `std::ranges::zip_view`,
      `std::ranges::zip_transform_view`, `std::ranges::adjacent_view`,
      `std::ranges::adjacent_transform_view`

### Example

```cpp
#include <array>
#include <iostream>
#include <ranges>

int main()
{
    constexpr static std::array data{1, 2, 3, 4, 5, 6};
    constexpr int window{3};

    auto Fun = [](auto... ints) { return (... + ints); };
    // Alternatively, the Fun could be any ternary (if window == 3) callable, e.g.:
    // auto Fun = [](int x, int y, int z) { return x + y + z; };

    constexpr auto view = data | std::views::adjacent_transform<window>(Fun);

    static_assert(
        view.size() == (data.size() - window + 1)
        && std::array{6, 9, 12, 15}
        == std::array{view[0], view[1], view[2], view[3]}
        && view[0] == Fun(data[0], data[1], data[2])
        && view[1] == Fun(data[1], data[2], data[3])
        && view[2] == Fun(data[2], data[3], data[4])
        && view[3] == Fun(data[3], data[4], data[5])
    );

    for (int x : view)
        std::cout << x << ' ';
    std::cout << '\n';
}
```

Output:

```text
6 9 12 15
```

### References

- C++23 standard (ISO/IEC 14882:2023):

### See also

- **ranges::adjacent_viewviews::adjacent (C++23)** — a `view` consisting of
  tuples of references to adjacent elements of the adapted view (class template)
  (range adaptor object)
- **ranges::transform_viewviews::transform (C++20)** — a `view` of a sequence
  that applies a transformation function to each element (class template) (range
  adaptor object)
- **ranges::zip_transform_viewviews::zip_transform (C++23)** — a `view`
  consisting of tuples of results of application of a transformation function to
  corresponding elements of the adapted views (class template) (customization
  point object)
- **ranges::transform (C++20)** — applies a function to a range of elements
  (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/ranges/adjacent_transform_view*
