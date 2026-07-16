# std::ranges::zip_transform_view<F,Views...>::*iterator*

```cpp
template< bool Const >
class /*iterator*/;  // (since C++23) (exposition only*)
```

The iterator type of a possibly const-qualified `zip_transform_view`, returned
by `zip_transform_view::begin` and in certain cases by
`zip_transform_view::end`.

The type `/*iterator*/<true>` or `/*iterator*/<false>` treats the underlying
views as const-qualified or non-const-qualified respectively.

### Member types

- **`Parent` (private)** — `zip_transform_view` if `Const` is `false`, `const
  zip_transform_view` otherwise. (exposition-only member type*)
- **`Base` (private)** — `InnerView` if `Const` is `false`, `const InnerView`
  otherwise. (exposition-only member type*)
- **`iterator_category` (optional)** — Let `/*maybe-const*/<Const, F>&` denote
  `const F&` if `Const` is `true`, `F&` otherwise. Let `/*maybe-const*/<Const,
  Views>` denote `const Views` if `Const` is `true`, `Views` otherwise. Let
  `/*POT*/` denote the pack of types `std::iterator_traits<std::iterator_t<
  /*maybe-const*/<Const, Views>>>::iterator_category...` If `/*Base*/` models
  `forward_range`, then `iterator_category` denotes: `std::input_iterator_tag`,
  if `std::invoke_result_t</*maybe-const*/<Const, F>&,
  ranges::range_reference_t</*maybe-const*/<Const, Views>>...>` Otherwise,
  `std::random_access_iterator_tag`, if is not a reference.
  `(std::derived_from</*POT*/, std::random_access_iterator_tag> && ...)` is
  `true`. Otherwise, `std::bidirectional_iterator_tag`, if
  `(std::derived_from</*POT*/, std::bidirectional_iterator_tag> && ...)` is
  `true`. Otherwise, `std::forward_iterator_tag`, if
  `(std::derived_from</*POT*/, std::forward_iterator_tag> && ...)` is `true`.
  Otherwise, `std::input_iterator_tag`. Not present if `/*Base*/` does not model
  `forward_range`.
- **`iterator_concept`** — `/*ziperator*/<Const>::iterator_concept`
- **`value_type`** — Let `/*RREF*/` be `ranges::range_reference_t<Views>...`,
  and `/*CRREF*/` be `ranges::range_reference_t<const Views>...`. Then:
  `std::remove_cvref_t<std::invoke_result_t<F&, /*RREF*/>>` if `Const` is
  `false`, `std::remove_cvref_t<std::invoke_result_t<const F&, /*CRREF*/>>`
  otherwise.
- **`difference_type`** — `range::range_difference_t</*Base*/>`

### Data members

- **`parent_` (private)** — A pointer `Parent*` to the parent object
  (exposition-only member object*)
- **`inner_` (private)** — An iterator of type `ziperator<Const>`.
  (exposition-only member type*)

### Member functions

- **(constructor) (C++23)** — constructs an iterator (public member function)
- **operator* (C++23)** — obtains the result of applying the invocable object to
  the underlying poined-to elements (public member function)
- **operator[] (C++23)** — obtains the result of applying the invocable object
  to the underlying elements at given offset (public member function)
- **operator++operator++(int)operator--operator--(int)operator+=operator-=
  (C++23)** — advances or decrements the underlying iterator (public member
  function)

### Non-member functions

- **operator==operator<=> (C++23)** — compares the underlying iterators
  (function)
- **operator+operator- (C++23)** — performs iterator arithmetic on underlying
  iterators (function)

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/zip_transform_view/iterator*
