# std::ranges::chunk_view<V>::*iterator*

```cpp
template< bool Const >
class /*iterator*/  // (since C++23) (exposition only*)
```

The return type of `chunk_view::begin`, and of `chunk_view::end` when the
underlying view `V` is a `common_range`.

Present only if `V` models `forward_range`.

The type `/*iterator*/<true>` is returned by the const-qualified overloads. The
type `/*iterator*/<false>` is returned by the non-const-qualified overloads.

### Member types

- **`Parent` (private)** — `const chunk_view` if `Const` is `true`, otherwise
  `chunk_view`. (exposition-only member type*)
- **`Base` (private)** — `const V` if `Const` is `true`, otherwise `V`.
  (exposition-only member type*)
- **`iterator_category`** — `std::input_iterator_tag`
- **`iterator_concept`** — `std::random_access_iterator_tag`, if `Base` models
  `random_access_range`. Otherwise, `std::bidirectional_iterator_tag`, if `Base`
  models `bidirectional_range`. Otherwise, `std::forward_iterator_tag`.
- **`value_type`** — `decltype(views::take(ranges::subrange(current_, end_),
  n_));`.
- **`difference_type`** — `ranges::range_difference_t<Base>`

### Data members

- **`current_` (private)** — `ranges::iterator_t<Base>`, holds an iterator to
  the first element of current chunk in the underlying sequence (i.e. the begin
  of a chunk). (exposition-only member object*)
- **`end_` (private)** — `ranges::sentinel_t<Base>`, holds a sentinel to the end
  of the underlying sequence. (exposition-only member object*)
- **`n_` (private)** — `ranges::range_difference_t<Base>`, holds initial number
  of elements in the chunk (the last chunk can be smaller). (exposition-only
  member object*)
- **`missing_` (private)** — `ranges::range_difference_t<Base>`, usually equals
  to `ranges::advance(current_, n_, end_);`. (exposition-only member object*)

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

### Example

### References

- C++23 standard (ISO/IEC 14882:2023):

### See also

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_view/iterator*
