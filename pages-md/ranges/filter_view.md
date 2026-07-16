# std::ranges::views::filter, std::ranges::filter_view

```cpp
template< ranges::input_range V,
          std::indirect_unary_predicate<ranges::iterator_t<V>> Pred >
    requires ranges::view<V> && std::is_object_v<Pred>
class filter_view
    : public ranges::view_interface<filter_view<V, Pred>>  // (1) (since C++20)
namespace views {
    inline constexpr /* unspecified */ filter = /* unspecified */;
}  // (2) (since C++20)
Call signature
template< ranges::viewable_range R, class Pred >
    requires /* see below */
constexpr ranges::view auto filter( R&& r, Pred&& pred );  // (since C++20)
template< class Pred >
constexpr /* range adaptor closure */ filter( Pred&& pred );  // (since C++20)
```

1) A range adaptor that represents `view` of an underlying sequence without the
   elements that fail to satisfy a predicate.

2) RangeAdaptorObject. The expression `views::filter(e, p)` is
   expression-equivalent to `filter_view(e, p)` for any suitable subexpressions
   `e` and `p`.

`filter_view` models the concepts `bidirectional_range`, `forward_range`,
`input_range`, and `common_range` when the underlying `view` `V` models
respective concepts.

### Data members

- **`base_` (private)** — The underlying `view` of type `V`. (exposition-only
  member object*)
- **`pred_` (private)** — Wraps the predicate used to filter out elements of
  `base_` of type `copyable-box``<Pred>`(until C++23)`movable-box``<Pred>`(since
  C++23) augmenting `Pred` with assignability when needed and hence always
  satisfies `copyable`or `movable`(since C++23). (exposition-only member
  object*)
- **`begin_` (private) (optional)** — An object of optional-like type that
  caches an iterator to the first element of `base_` that satisfies the `pred_`.
  Present only if `filter_view` models `forward_range`. (exposition-only member
  object*)

### Member functions

- **(constructor) (C++20)** — constructs a `filter_view` (public member
  function)
- **base (C++20)** — returns the underlying view `V` (public member function)
- **pred (C++20)** — returns a reference to the predicate stored within
  `filter_view` (public member function)
- **begin (C++20)** — returns the beginning iterator of the `filter_view`
  (public member function)
- **end (C++20)** — returns the sentinel of the `filter_view` (public member
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
- **front (C++20)** — returns the first element in the derived view. Provided if
  it satisfies `forward_range`. (public member function of
  `std::ranges::view_interface<D>`)
- **back (C++20)** — returns the last element in the derived view. Provided if
  it satisfies `bidirectional_range` and `common_range`. (public member function
  of `std::ranges::view_interface<D>`)

## std::ranges::filter_view::filter_view

```cpp
filter_view() requires std::default_initializable<V> &&
                       std::default_initializable<Pred> = default;  // (1) (since C++20)
constexpr explicit filter_view( V base, Pred pred );  // (2) (since C++20)
```

1) Value-initializes `base_` via its default member initializer (`= V()`) and
   default-initializes `pred_` (which value-initializes the contained `Pred`).

2) Initializes `base_` with `std::move(base)` and initializes `pred_` with
   `std::move(pred)`.

### Parameters

- **base** — range to filter
- **pred** — predicate to filter out elements

## std::ranges::filter_view::base

```cpp
constexpr V base() const& requires std::copy_constructible<V>;  // (1) (since C++20)
constexpr V base() &&;  // (2) (since C++20)
```

1) Equivalent to `return base_;`.

2) Equivalent to `return std::move(base_);`.

## std::ranges::filter_view::pred

```cpp
constexpr const Pred& pred() const;  // (since C++20)
```

Returns a reference to the contained `Pred` object. The behavior is undefined if
`pred_` does not contain a value.

## std::ranges::filter_view::begin

```cpp
constexpr /* iterator */ begin();  // (exposition only*)
```

In order to provide the amortized constant time complexity required by the
`range` concept, this function caches the result within the `filter_view` object
for use on subsequent calls. Equivalent to

```cpp
if constexpr (!ranges::forward_range<V>)
    return /* iterator */{*this, ranges::find_if(base_, std::ref(*pred_))};
else
{
    if (!begin_.has_value())
        begin_ = ranges::find_if(base_, std::ref(*pred_)); // caching
    return /* iterator */{*this, begin_.value())};
}
```

The behavior is undefined if `pred_` does not contain a value.

## std::ranges::filter_view::end

```cpp
constexpr auto end();  // (since C++20)
```

Returns an iterator to the end. Equivalent to

```cpp
if constexpr (ranges::common_range<V>)
    return /* iterator */{*this, ranges::end(base_)};
else
    return /* sentinel */{*this};
```

### Deduction guides

```cpp
template< class R, class Pred >
filter_view( R&&, Pred ) -> filter_view<views::all_t<R>, Pred>;  // (since C++20)
```

### Nested classes

- ***iterator* (C++20)** — the iterator type of `filter_view` (exposition-only
  member class*)
- ***sentinel* (C++20)** — the sentinel type of `filter_view` when the
  underlying view is not a `common_range` (exposition-only member class*)

### Example

```cpp
#include <iostream>
#include <ranges>

int main()
{
    auto even = [](int i) { return 0 == i % 2; };
    auto square = [](int i) { return i * i; };

    for (int i : std::views::iota(0, 6)
               | std::views::filter(even)
               | std::views::transform(square))
        std::cout << i << ' ';
    std::cout << '\n';
}
```

Output:

```text
0 4 16
```

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3714 (P2711R1) | C++20 | the multi-parameter constructor was not explicit
      | made explicit
  P2325R3 | C++20 | if `Pred` is not `default_initializable`, the default
      constructor constructs a `filter_view` which does not contain a `Pred` |
      the `filter_view` is also not `default_initializable`

### See also

- **ranges::take_while_viewviews::take_while (C++20)** — a `view` consisting of
  the initial elements of another `view`, until the first element on which a
  predicate returns `false` (class template) (range adaptor object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/filter_view*
