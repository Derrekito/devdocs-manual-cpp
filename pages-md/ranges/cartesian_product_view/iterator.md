# std::ranges::cartesian_product_view<First, Vs...>::*iterator*

```cpp
template< bool Const >
class /*iterator*/  // (since C++23) (exposition only*)
```

The return type of `cartesian_product_view::begin`, and of
`cartesian_product_view::end` when the underlying view `V` is a `common_range`.

The type `/*iterator*/<true>` is returned by the const-qualified overloads. The
type `/*iterator*/<false>` is returned by the non-const-qualified overloads.

### Member types

- **`iterator_category`** — `std::input_iterator_tag`
- **`iterator_concept`** — `std::random_access_iterator_tag`, if
  `/*cartesian-product-is-random-access*/<Const, First, Vs...>` is modeled,
  `std::bidirectional_iterator_tag`, if
  `/*cartesian-product-is-bidirectional*/<Const, First, Vs...>` is modeled,
  `std::forward_iterator_tag`, if `/*maybe-const*/<Const, First>` models
  `forward_range`, `std::input_iterator_tag` otherwise
- **`value_type`** — `std::tuple<ranges::range_value_t</*maybe-const*/<Const,
  First>>, ranges::range_value_t</*maybe-const*/<Const, Vs>>...>;`
- **`reference`** — `std::tuple<ranges::range_reference_t</*maybe-const*/<Const,
  First>>, ranges::range_reference_t</*maybe-const*/<Const, Vs>>...>;`
- **`difference_type`** — An implementation-defined *signed-integer-like* type
  (maybe the smallest of such types), which is sufficiently wide to store the
  product of the maximum sizes of all underlying ranges, if such type exists.
- **`Parent` (private)** — `/*maybe-const*/<Const, cartesian_product_view>`.
  (exposition-only member type*)

### Data members

- **`parent_` (private)** — A pointer of type `Parent` to the parent
  `cartesian_product_view`. (exposition-only member object*)
- **`current_` (private)** — A tuple of iterators to the current underlying
  elements of type `std::tuple<ranges::iterator_t</*maybe-const*/<Const,
  First>>, ranges::iterator_t</*maybe-const*/<Const, Vs>>...>`. (exposition-only
  member object*)

### Member functions

- **(constructor) (C++23)** — constructs an iterator (public member function)
- **operator* (C++23)** — accesses the element (public member function)
- **operator[] (C++23)** — accesses an element by index (public member function)
- **operator++operator++(int)operator--operator--(int)operator+=operator-=
  (C++23)** — advances or decrements the underlying iterator (public member
  function)
- ****next** (C++23)** — advances the iterator (exposition-only member
  function*)
- ****prev** (C++23)** — decrements the iterator (exposition-only member
  function*)
- ****distance_from** (C++23)** — returns the distance between two iterators
  (exposition-only member function*)

### Non-member functions

- **operator==operator<=> (C++23)** — compares the underlying iterators
  (function)
- **operator+operator- (C++23)** — performs iterator arithmetic (function)
- **iter_move (C++23)** — casts the result of dereferencing the underlying
  iterator to its associated rvalue reference type (function)
- **iter_swap (C++23)** — swaps underlying pointed-to elements (function)

### Example

### References

- C++23 standard (ISO/IEC 14882:2023):

### See also

---
*Source: https://en.cppreference.com/w/cpp/ranges/cartesian_product_view/iterator*
