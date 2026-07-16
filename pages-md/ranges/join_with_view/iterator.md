# std::ranges::join_with_view<V,Pattern>::*iterator*

```cpp
template< bool Const >
class /*iterator*/  // (since C++23) (exposition only*)
```

The return type of `join_with_view::begin`, and of `join_with_view::end` when
both the outer range `V` and the inner range `ranges::range_reference_t<V>`
satisfy `common_range` and the parent `join_with_view` is a `forward_range`.

If either `V` or `Pattern` is not a simple view, `Const` is true for iterators
returned from the const overloads, and false otherwise. If `V` and `Pattern` are
simple views, `Const` is true if and only if `ranges::range_reference_t<V>` is a
reference.

### Member types

- **`Parent`** — `const join_view<V>` if `Const` is true, otherwise
  `join_view<V>` (exposition-only member type*)
- **`Base`** — `const V` if `Const` is true, otherwise `V` (exposition-only
  member type*)
- **`InnerBase`** — `ranges::range_reference_t<Base>` (exposition-only member
  type*)
- **`PatternBase`** — `const Pattern` if `Const` is true, otherwise `Pattern`
  (exposition-only member type*)
- **`iterator_concept`** — `std::bidirectional_iterator_tag`, if
  `ranges::range_reference_t<Base>` is a reference type, `Base`, `InnerBase` and
  `PatternBase` each model `bidirectional_range`, and `InnerBase` and
  `PatternBase` each model `common_range`; `std::forward_iterator_tag`, if
  `ranges::range_reference_t<Base>` is a reference type, and `Base` and
  `InnerBase` each model `forward_range`; `std::input_iterator_tag` otherwise.
- **`iterator_category`** — Defined only if `iterator::iterator_concept` (see
  above) denotes `std::forward_iterator_tag`. Let `OUTERC` be
  `std::iterator_traits<ranges::iterator_t<Base>>::iterator_category`, `INNERC`
  be `std::iterator_traits<ranges::iterator_t<InnerBase>>::iterator_category`,
  and `PATTERNC` be
  `std::iterator_traits<ranges::iterator_t<PatternBase>>::iterator_category`.
  `std::input_iterator_tag`, if
  `std::common_reference_t<ranges::range_reference_t<InnerBase>,
  ranges::range_reference_t<PatternBase>>` is not a reference type;
  `std::bidirectional_iterator_tag`, if: `OUTERC`, `INNERC`, and `PATTERNC` each
  model `std::derived_from<std::bidirectional_iterator_tag>` and
  `ranges::range_reference_t<Base>` and `PatternBase` each model `common_range`;
  `std::forward_iterator_tag`, if `OUTERC`, `INNERC`, and `PATTERNC` each model
  `std::derived_from<std::forward_iterator_tag>`; `std::input_iterator_tag`
  otherwise.
- **`value_type`** — `std::common_type_t<ranges::range_value_t<InnerBase>,
  ranges::range_value_t<PatternBase>>`
- **`difference_type`** — `std::common_type_t<ranges::range_difference_t<Base>,
  ranges::range_difference_t<InnerBase>,
  ranges::range_difference_t<PatternBase>>`

### Member functions

- **(constructor) (C++23)** — constructs an iterator (public member function)
- **operator* (C++23)** — accesses the element (public member function)
- **operator++operator++(int)operator--operator--(int) (C++23)** — advances or
  decrements the underlying iterator (public member function)

### Non-member functions

- **operator== (C++23)** — compares the underlying iterators (function)
- **iter_move (C++23)** — casts the result of dereferencing the underlying
  iterator to its associated rvalue reference type (function)
- **iter_swap (C++23)** — swaps the objects pointed to by two underlying
  iterators (function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/join_with_view/iterator*
