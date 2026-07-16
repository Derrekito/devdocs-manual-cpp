# std::ranges::adjacent_view<V,N>::*iterator*

```cpp
template< bool Const >
class /*iterator*/  // (since C++23) (exposition only*)
```

The return type of `adjacent_view::begin`, and of `adjacent_view::end` when the
underlying view `V` is a `common_range`.

The type `/*iterator*/<true>` is returned by the const-qualified overloads. The
type `/*iterator*/<false>` is returned by the non-const-qualified overloads.

### Member types

- **`Base` (private)** — `const V` if `Const` is `true`, otherwise `V`.
  (exposition-only member type*)
- **`iterator_category`** — `std::input_iterator_tag`
- **`iterator_concept`** — `std::random_access_iterator_tag`, if `Base` models
  `random_access_range`. Otherwise, `std::bidirectional_iterator_tag`, if `Base`
  models `bidirectional_range`. Otherwise, `std::forward_iterator_tag`.
- **`value_type`** — `std::tuple</*REPEAT*/(ranges::range_value_t<Base>,
  N)...>;`
- **`difference_type`** — `ranges::range_difference_t<Base>`

### Data members

- **`current_` (private)** — `std::array<ranges::iterator_t<Base>, N>`.
  (exposition-only member object*)

### Member functions

- **(constructor) (C++23)** — constructs an iterator (public member function)
- **operator* (C++23)** — accesses the element (public member function)
- **operator[] (C++23)** — accesses an element by index (public member function)
- **operator++operator++(int)operator--operator--(int)operator+=operator-=
  (C++23)** — advances or decrements the underlying iterators (public member
  function)

### Non-member functions

- **operator==operator<operator>operator<=operator>=operator<=> (C++23)** —
  compares the underlying iterators (function)
- **operator+operator- (C++23)** — performs iterator arithmetic (function)
- **iter_move (C++23)** — casts the result of dereferencing the underlying
  iterator to its associated rvalue reference type (function)
- **iter_swap (C++23)** — swaps the objects pointed to by two underlying
  iterators (function)

### Example

### References

- C++23 standard (ISO/IEC 14882:2023):

### See also

---
*Source: https://en.cppreference.com/w/cpp/ranges/adjacent_view/iterator*
