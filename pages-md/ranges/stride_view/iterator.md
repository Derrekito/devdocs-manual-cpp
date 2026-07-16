# std::ranges::stride_view<V>::*iterator*

```cpp
template< bool Const >
class /*iterator*/  // (since C++23) (exposition only*)
```

The return type of `stride_view::begin`, and of `stride_view::end` when the
underlying view `V` is a `common_range`.

The type `/*iterator*/<true>` is returned by the const-qualified overloads. The
type `/*iterator*/<false>` is returned by the non-const-qualified overloads.

### Member types

- **`Parent` (private)** — `const ranges::stride_view` if `Const` is `true`,
  otherwise `ranges::stride_view`. (exposition-only member type*)
- **`Base` (private)** — `const V` if `Const` is `true`, otherwise `V`.
  (exposition-only member type*)
- **`difference_type`** — `ranges::range_difference_t<Base>`
- **`value_type`** — `ranges::range_value_t<Base>`
- **`iterator_concept`** — `std::random_access_iterator_tag`, if `Base` models
  `random_access_range`. Otherwise, `std::bidirectional_iterator_tag`, if `Base`
  models `bidirectional_range`. Otherwise, `std::forward_iterator_tag`, if
  `Base` models `forward_range`. Otherwise, `std::input_iterator_tag`.
- **`iterator_category`** — Defined if and only if `Base` models
  `forward_range`. Let `C` denote the type
  iterator_traits<iterator_t<Base>>::iterator_category. Then:
  `std::random_access_iterator_tag`, if `C` models
  `std::derived_from<std::random_access_iterator_tag>`. Otherwise, `C`.

### Data members

- **`current_` (private)** — `ranges::iterator_t<Base>`, holds an iterator to
  the current element. (exposition-only member object*)
- **`end_` (private)** — `ranges::sentinel_t<Base>`, holds a sentinel to the
  end. (exposition-only member object*)
- **`stride_` (private)** — `ranges::range_difference_t<Base>`, holds the stride
  value. (exposition-only member object*)
- **`missing_` (private)** — `ranges::range_difference_t<Base>`, usually holds
  the result of `ranges::advance(current_, stride_, end_)`. (exposition-only
  member object*)

### Member functions

- **(constructor) (C++23)** — constructs an iterator (public member function)
- **base (C++23)** — returns an iterator to current element (public member
  function)
- **operator* (C++23)** — accesses the element (public member function)
- **operator[] (C++23)** — accesses an element by index (public member function)
- **operator++operator++(int)operator--operator--(int)operator+=operator-=
  (C++23)** — advances or decrements the underlying iterator (public member
  function)

### Non-member functions

- **operator==operator<operator>operator<=operator>=operator<=> (C++23)** —
  compares the underlying iterators (function)
- **operator+operator- (C++23)** — performs iterator arithmetic (function)
- **iter_move (C++23)** — casts the result of dereferencing the underlying
  iterator to its associated rvalue reference type (function)
- **iter_swap (C++23)** — swaps underlying pointed-to elements (function)

### Example

### References

- C++23 standard (ISO/IEC 14882:2023):

### See also

---
*Source: https://en.cppreference.com/w/cpp/ranges/stride_view/iterator*
