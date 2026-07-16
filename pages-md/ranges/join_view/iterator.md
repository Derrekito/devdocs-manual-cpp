# std::ranges::join_view<V>::*iterator*

```cpp
template< bool Const >
class /*iterator*/  // (since C++20) (exposition only*)
```

The return type of `join_view::begin`, and of `join_view::end` when both the
outer range `V` and the inner range `ranges::range_reference_t<V>` satisfy
`common_range` and the parent `join_view` is a `forward_range`.

If `V` is not a simple view (e.g. if `ranges::iterator_t<const V>` is invalid or
different from `ranges::iterator_t<V>`), `Const` is true for iterators returned
from the const overloads, and false otherwise. If `V` is a simple view, `Const`
is true if and only if `ranges::range_reference_t<V>` is a reference.

### Member types

- **`Parent`** — `const join_view<V>` if `Const` is `true`, otherwise
  `join_view<V>` (exposition-only member type*)
- **`Base`** — `const V` if `Const` is `true`, otherwise `V` (exposition-only
  member type*)
- **`OuterIter`** — `ranges::iterator_t<Base>` (exposition-only member type*)
- **`InnerIter`** — `ranges::iterator_t<ranges::range_reference_t<Base>>`
  (exposition-only member type*)
- **`iterator_concept`** — `std::bidirectional_iterator_tag`, if
  `ranges::range_reference_t<Base>` is a reference type, and `Base` and
  `ranges::range_reference_t<Base>` each model `bidirectional_range`;
  `std::forward_iterator_tag`, if `ranges::range_reference_t<Base>` is a
  reference type, and `Base` and `ranges::range_reference_t<Base>` each model
  `forward_range`; `std::input_iterator_tag` otherwise.
- **`iterator_category`** — Defined only if `iterator::iterator_concept` (see
  above) denotes `std::forward_iterator_tag`. Let `OUTERC` be
  `std::iterator_traits<ranges::iterator_t<Base>>::iterator_category`, and let
  `INNERC` be
  `std::iterator_traits<ranges::iterator_t<ranges::range_reference_t<Base>>>::iterator_category`.
  `std::bidirectional_iterator_tag`, if `OUTERC` and `INNERC` each model
  `std::derived_from<std::bidirectional_iterator_tag>`;
  `std::forward_iterator_tag`, if `OUTERC` and `INNERC` each model
  `std::derived_from<std::forward_iterator_tag>`; `std::input_iterator_tag`
  otherwise.
- **`value_type`** — `ranges::range_value_t<ranges::range_reference_t<Base>>`
- **`difference_type`** — `std::common_type_t<ranges::range_difference_t<Base>,
  ranges::range_difference_t<ranges::range_reference_t<Base>>>`

### Member objects

- **`outer_` (private)** — An object of type `OuterIter` (exposition-only member
  object*)
- **`inner_` (private)** — An object of type `InnerIter` (exposition-only member
  object*)
- **`parent_` (private)** — An object of type `Parent` (exposition-only member
  object*)

### Member functions

- **(constructor) (C++20)** — constructs an iterator (public member function)
- **operator*operator-> (C++20)** — accesses the element (public member
  function)
- **operator++operator++(int)operator--operator--(int) (C++20)** — advances or
  decrements the underlying iterators (public member function)
- ***satisfy* (C++20)** — skips over empty inner ranges (exposition-only member
  function*)

### Non-member functions

- **operator== (C++20)** — compares the underlying iterators (function)
- **iter_move (C++20)** — casts the result of dereferencing the underlying
  iterator to its associated rvalue reference type (function)
- **iter_swap (C++20)** — swaps the objects pointed to by two underlying
  iterators (function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/join_view/iterator*
