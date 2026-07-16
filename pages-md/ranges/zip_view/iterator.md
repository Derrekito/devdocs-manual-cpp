# std::ranges::zip_view<Views...>::*iterator*

```cpp
template< bool Const >
class /*iterator*/;  // (1) (exposition only*)
Helper concepts
template< bool C, class... Views >
concept /*all-forward*/ =
    (ranges::forward_range<std::conditional_t<C, const Views, Views>> && ...);  // (2) (exposition only*)
template< bool C, class... Views >
concept /*all-bidirectional*/ =
    (ranges::bidirectional_range<std::conditional_t<C, const Views, Views>> && ...);  // (3) (exposition only*)
template< bool C, class... Views >
concept /*all-random-access*/ =
    (ranges::random_access_range<std::conditional_t<C, const Views, Views>> && ...);  // (4) (exposition only*)
```

The iterator type of a possibly const-qualified `zip_view`, returned by
`zip_view::begin` and in certain cases by `zip_view::end`.

The type `/*iterator*/<true>` or `/*iterator*/<false>` treats the underlying
views as const-qualified or non-const-qualified respectively.

### Member types

- **`iterator_concept`** — `std::random_access_iterator_tag` if
  `/*all-random-access*/<Const, Views...>` is `true`, otherwise
  `std::bidirectional_iterator_tag` if `/*all-bidirectional*/<Const, Views...>`
  is `true`, otherwise `std::forward_iterator_tag` if `/*all-forward*/<Const,
  Views...>` is `true`, otherwise `std::input_iterator_tag`
- **`iterator_category`** — `std::input_iterator_tag` if `/*all-forward*/<Const,
  Views...>` is `true`, not defined otherwise
- **`value_type`** — `std::tuple<ranges::range_value_t<Views>...>` if `Const` is
  `false`, `std::tuple<ranges::range_value_t<const Views>...>` otherwise.
- **`difference_type`** —
  `std::common_type_t<ranges::range_difference_t<Views>...>` if `Const` is
  `false`, `std::common_type_t<ranges::range_difference_t<const Views>...>`
  otherwise.

### Data members

- **`current_` (private)** — A tuple of underlying iterators of type
  `std::tuple<ranges::iterator_t<Views>...>` or
  `std::tuple<ranges::iterator_t<const Views>...>` when `Const` is `false` or
  `true` respectively. (exposition-only member object*)

### Member functions

- **(constructor) (C++23)** — constructs an iterator (public member function)
- **operator* (C++23)** — obtains a tuple-like value that consists of underlying
  pointed-to elements (public member function)
- **operator[] (C++23)** — obtains a tuple-like value that consists of
  underlying elements at given offset (public member function)
- **operator++operator++(int)operator--operator--(int)operator+=operator-=
  (C++23)** — advances or decrements the underlying iterators (public member
  function)

### Non-member functions

- **operator==operator<operator>operator<=operator>=operator<=> (C++23)** —
  compares the underlying iterators (function)
- **operator+operator- (C++23)** — performs iterator arithmetic on underlying
  iterators (function)
- **iter_move (C++23)** — obtains a tuple-like value that denotes underlying
  pointed-to elements to be moved (function)
- **iter_swap (C++23)** — swaps underlying pointed-to elements (function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/zip_view/iterator*
