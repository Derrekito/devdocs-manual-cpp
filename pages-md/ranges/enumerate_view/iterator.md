# std::ranges::enumerate_view<V>::*iterator*

```cpp
template< bool Const >
class /* iterator */  // (since C++23) (exposition only*)
```

The return type of `enumerate_view::begin`, and of `enumerate_view::end` when
the underlying view `V` is a `common_range`.

The type /* iterator */<true> is returned by the const-qualified overloads. The
type /* iterator */<false> is returned by the non-const-qualified overloads.

### Member types

- **`Base` (private)** — const V if `Const` is `true`, otherwise `V`.
  (exposition-only member type*)
- **`iterator_category`** — `std::input_iterator_tag`
- **`iterator_concept`** — `std::random_access_iterator_tag`, if `Base` models
  `random_access_range`. Otherwise `std::bidirectional_iterator_tag`, if `Base`
  models `bidirectional_range`. Otherwise `std::forward_iterator_tag`, if `Base`
  models `forward_range`. Otherwise `std::input_iterator_tag`.
- **`difference_type`** — ranges::range_difference_t<Base>
- **`value_type`** — std::tuple<difference_type, ranges::range_value_t<Base>>
- **`reference-type` (private)** — std::tuple<difference_type,
  ranges::range_reference_t<Base>> (exposition-only member type*)

### Data members

- **`current_` (private)** — An iterator to the current element of type
  ranges::iterator_t<Base> (exposition-only member object*)
- **`pos_` (private)** — A current index of type `difference_type`
  (exposition-only member object*)

### Member functions

- **(constructor) (C++23)** — constructs an iterator (public member function)
- **base (C++23)** — returns an iterator to current element (public member
  function)
- **index (C++23)** — returns current index (public member function)
- **operator* (C++23)** — accesses the element (public member function)
- **operator[] (C++23)** — accesses an element by index (public member function)
- **operator++operator++(int)operator--operator--(int)operator+=operator-=
  (C++23)** — advances or decrements the underlying iterator (public member
  function)

### Non-member functions

- **operator==operator<=> (C++23)** — compares the underlying iterators
  (function)
- **operator+operator- (C++23)** — performs iterator arithmetic (function)
- **iter_move (C++23)** — casts the result of dereferencing the underlying
  iterator to its associated rvalue reference type (function)

### Example

### References

- C++23 standard (ISO/IEC 14882:2023):

---
*Source: https://en.cppreference.com/w/cpp/ranges/enumerate_view/iterator*
