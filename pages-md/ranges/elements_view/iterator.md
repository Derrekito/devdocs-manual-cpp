# std::ranges::elements_view<V,N>::*iterator*

```cpp
template< bool Const >
class /*iterator*/;  // (exposition only*)
```

The return type of `elements_view::begin`, and of `elements_view::end` when the
underlying view is a `common_range`.

The type `/*iterator*/<true>` is returned by the const-qualified overloads. The
type `/*iterator*/<false>` is returned by the non-const-qualified overloads.

### Member types

- **`Base` (private)** — const V if `Const` is true, otherwise `V`.
  (exposition-only member type*)
- **`iterator_concept`** — Denotes: `std::random_access_iterator_tag`, if `Base`
  models `random_access_range`. Otherwise, `std::bidirectional_iterator_tag`, if
  `Base` models `bidirectional_range`. Otherwise, `std::forward_iterator_tag`,
  if `Base` models `forward_range`. Otherwise, `std::input_iterator_tag`.
- **`iterator_category`** — Not defined, if `Base` does not model
  `forward_range`. Otherwise, `std::input_iterator_tag`, if
  `std::get<N>(*current_)` is an rvalue. Otherwise, let `C` be the type
  `std::iterator_traits<std::iterator_t<Base>>::iterator_category`.
  `std::random_access_iterator_tag`, if `C` models
  `std::derived_from<std::random_access_iterator_tag>`. Otherwise, `C`.
- **`value_type`** — `std::remove_cvref_t<std::tuple_element_t<N,
  ranges::range_value_t<Base>>>`
- **`difference_type`** — `ranges::range_difference_t<Base>`

### Data members

- **`current_` (private)** — An iterator of type `ranges::iterator_t<Base>` to
  current element of underlying sequence. (exposition-only member object*)

### Member functions

- **(constructor) (C++20)** — constructs an iterator (public member function)
- **base (C++20)** — returns the underlying iterator (public member function)
- **operator* (C++20)** — accesses the `N`th tuple element (public member
  function)
- **operator[] (C++20)** — accesses an element by index (public member function)
- **operator++operator++(int)operator--operator--(int)operator+=operator-=
  (C++20)** — advances or decrements the underlying iterator (public member
  function)

### Non-member functions

- **operator==operator<operator>operator<=operator>=operator<=> (C++20)** —
  compares the underlying iterators (function)
- **operator+operator- (C++20)** — performs iterator arithmetic (function)

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2259R1 | C++20 | member `iterator_category` is always defined | defined only
      if `Base` models `forward_range`
  LWG 3555 | C++20 | the definition of `iterator_concept` ignores const | made
      to consider

### See also

- **iterator (C++20)** — the return type of `ranges::transform_view::begin`, and
  of `ranges::transform_view::end` when the underlying view is a `common_range`
  (private member class template)

---
*Source: https://en.cppreference.com/w/cpp/ranges/elements_view/iterator*
