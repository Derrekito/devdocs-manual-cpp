# std::ranges::views::reverse, std::ranges::reverse_view

```cpp
template< ranges::view V >
    requires ranges::bidirectional_range<V>
class reverse_view
    : public ranges::view_interface<reverse_view<V>>  // (1) (since C++20)
namespace views {
    inline constexpr /* unspecified */ reverse = /* unspecified */;
}  // (2) (since C++20)
Call signature
template< ranges::viewable_range R >
    requires /* see below */
constexpr ranges::view auto reverse( R&& r );  // (since C++20)
```

1) A range adaptor that represents a view of underlying `view` with reversed
   order.
2) RangeAdaptorObject. The expression `views::reverse(e)` is
expression-equivalent to one of the following expressions, except that `e` is
evaluated only once:

- `e.base()`, if the type of `e` is a (possibly cv-qualified) specialization of
  `reverse_view`;
- otherwise, if the type of `e` is (possibly cv-qualified)
  `ranges::subrange<std::reverse_iterator<I>, std::reverse_iterator<I>, K>` for
  some iterator type `I` and value `K` of type `ranges::subrange_kind`:
- otherwise `ranges::reverse_view{e}`.

In other words, `views::reverse` unwraps reversed views if possible.

A `reverse_view` always models `bidirectional_range` and `common_range`, and it
models `borrowed_range`, `sized_range`, or `random_access_range` if the
underlying view type `V` models the corresponding concept.

### Data members

- **`base_` (private)** — The underlying view of type `V`. (exposition-only
  member object*)
- **`cached_end_` (private)** — An optional-like cache object that holds either
  no value or the end iterator/position of the underlying view, which exists
  only if the underlying view type `V` does not model `common_range`.
  (exposition-only member object*)

### Member functions

- **(constructor) (C++20)** — constructs a `reverse_view` (public member
  function)
- **base (C++20)** — returns the underlying view `V` (public member function)
- **begin (C++20)** — returns the beginning iterator of the `reverse_view`
  (public member function)
- **end (C++20)** — returns the end iterator of the `reverse_view` (public
  member function)
- **size (C++20)** — returns the size of the view if it is bounded (public
  member function)

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

## std::ranges::reverse_view::reverse_view

```cpp
reverse_view() requires std::default_initializable<V> = default;  // (1) (since C++20)
constexpr reverse_view( V r );  // (2) (since C++20)
```

1) Value-initializes `base_` via its default member initializer (`= V()`).

2) Initializes `base_` with `std::move(r)`.

### Parameters

- **r** — range to reverse

## std::ranges::reverse_view::base

```cpp
constexpr V base() const& requires std::copy_constructible<V>;  // (1) (since C++20)
constexpr V base() &&;  // (2) (since C++20)
```

Returns the underlying view.

1) Copy-constructs the result from the underlying view. Equivalent to `return
   base_;`.

2) Move-constructs the result from the underlying view. Equivalent to `return
   std::move(base_);`.

## std::ranges::reverse_view::begin

```cpp
constexpr std::reverse_iterator<ranges::iterator_t<V>> begin();  // (1) (since C++20)
constexpr std::reverse_iterator<ranges::iterator_t<V>> begin()
    requires ranges::common_range<V>;  // (2) (since C++20)
constexpr auto begin() const requires ranges::common_range<const V>;  // (3) (since C++20)
```

1) Returns `std::make_reverse_iterator(ranges::next(ranges::begin(base_),
   ranges::end(base_)))`. In order to provide the amortized constant time
   complexity required by the `range` concept, this function caches the result
   within the cache object for use on subsequent calls.

2,3) Equivalent to `return std::make_reverse_iterator(ranges::end(base_));`.

## std::ranges::reverse_view::end

```cpp
constexpr std::reverse_iterator<ranges::iterator_t<V>> end();  // (1) (since C++20)
constexpr auto end() const requires ranges::common_range<const V>;  // (2) (since C++20)
```

Equivalent to `return std::make_reverse_iterator(ranges::begin(base_));`.

## std::ranges::reverse_view::size

```cpp
constexpr auto size() requires ranges::sized_range<V>;  // (1) (since C++20)
constexpr auto size() const requires ranges::sized_range<const V>;  // (2) (since C++20)
```

Returns the size of the view if the view is bounded.

1,2) Equivalent to `return ranges::size(base_);`.

### Deduction guides

```cpp
template< class R >
reverse_view( R&& ) -> reverse_view<views::all_t<R>>;  // (since C++20)
```

### Helper templates

```cpp
template< class T >
inline constexpr bool enable_borrowed_range<std::ranges::reverse_view<T>> =
    std::ranges::enable_borrowed_range<T>;  // (since C++20)
```

This specialization of `std::ranges::enable_borrowed_range` makes `reverse_view`
satisfy `borrowed_range` when the underlying view satisfies it.

### Example

```cpp
#include <iostream>
#include <ranges>

int main()
{
    static constexpr auto il = {3, 1, 4, 1, 5, 9};

    std::ranges::reverse_view rv{il};
    for (int i : rv)
        std::cout << i << ' ';
    std::cout << '\n';

    for (int i : il | std::views::reverse)
        std::cout << i << ' ';
    std::cout << '\n';

    // operator[] is inherited from std::view_interface
    for (auto i{0U}; i != rv.size(); ++i)
        std::cout << rv[i] << ' ';
    std::cout << '\n';
}
```

Output:

```text
9 5 1 4 1 3
9 5 1 4 1 3
9 5 1 4 1 3
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3494 | C++20 | `reverse_view` was never a `borrowed_range` | it is a
      `borrowed_range` if its underlying view is

### See also

- **reverse_iterator** — iterator adaptor for reverse-order traversal (class
  template)
- **ranges::reverse (C++20)** — reverses the order of elements in a range
  (niebloid)
- **ranges::reverse_copy (C++20)** — creates a copy of a range that is reversed
  (niebloid)

---
*Source: https://en.cppreference.com/w/cpp/ranges/reverse_view*
