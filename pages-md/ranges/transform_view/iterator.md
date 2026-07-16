# std::ranges::transform_view<V,F>::*iterator*

```cpp
template< bool Const >
class /*iterator*/  // (since C++20) (exposition only*)
```

The return type of `transform_view::begin`, and of `transform_view::end` when
the underlying view is a `common_range`.

The type `/*iterator*/<true>` is returned by the const-qualified overloads. The
type `/*iterator*/<false>` is returned by the non-const-qualified overloads.

### Member types

- **`Parent` (private)** — `const ranges::transform_view<V, F>` if `Const` is
  `true`, otherwise `ranges::transform_view<V, F>`. (exposition-only member
  type*)
- **`Base` (private)** — `const V` if `Const` is `true`, otherwise `V`.
  (exposition-only member type*)
- **`iterator_concept`** — `std::random_access_iterator_tag` if `Base` models
  `random_access_range`, `std::bidirectional_iterator_tag` if `Base` models
  `bidirectional_range`, `std::forward_iterator_tag` if `Base` models
  `forward_range`, `std::input_iterator_tag` otherwise.
- **`iterator_category`** — Not defined if `Base` does not model
  `forward_range`. Otherwise, if `std::invoke_result_t<F&,
  ranges::range_reference_t<Base>>` is not a reference,
  `std::input_iterator_tag`. Else, let `C` be
  `std::iterator_traits<ranges::iterator_t<Base>>::iterator_category`. If `C` is
  `std::contiguous_iterator_tag`, the type is `std::random_access_iterator_tag`;
  otherwise, the type is `C`.
- **`value_type`** — `std::remove_cvref_t<std::invoke_result_t<F&,
  ranges::range_reference_t<Base>>>`
- **`difference_type`** — `ranges::range_difference_t<Base>`

### Data members

- **`current_` (private)** — An iterator into (possibly const-qualified) `V`.
  (exposition-only member object*)
- **`parent_` (private)** — A pointer to parent `transform_view`.
  (exposition-only member object*)

### Member functions

- **(constructor) (C++20)** — constructs an iterator (public member function)
- **base (C++20)** — returns the underlying iterator (public member function)
- **operator* (C++20)** — accesses the transformed element (public member
  function)
- **operator[] (C++20)** — accesses an element by index (public member function)
- **operator++operator++(int)operator--operator--(int)operator+=operator-=
  (C++20)** — advances or decrements the underlying iterator (public member
  function)

### Non-member functions

- **operator==operator<operator>operator<=operator>=operator<=> (C++20)** —
  compares the underlying iterators (function)
- **operator+operator- (C++20)** — performs iterator arithmetic (function)
- **iter_move (C++20)** — obtains an rvalue reference to the transformed element
  (function)

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  P2259R1 | C++20 | member `iterator_category` is always defined | defined only
      if `Base` models `forward_range`
  LWG 3555 | C++20 | the definition of `iterator_concept` ignores const | made
      to consider
  LWG 3798 | C++20 | `iterator_category` is input-only if transformation result
      is rvalue reference | may have a stronger category

---
*Source: https://en.cppreference.com/w/cpp/ranges/transform_view/iterator*
