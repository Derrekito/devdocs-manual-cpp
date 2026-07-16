# std::ranges::adjacent_transform_view<V,F,N>::*sentinel*

```cpp
template< bool Const >
class /*sentinel*/;  // (exposition only*)
```

The return type of `adjacent_transform_view::end` when the underlying view is
not a `common_range`.

The type `/*sentinel*/<true>` is returned by the const-qualified overload. The
type `/*sentinel*/<false>` is returned by the non-const-qualified overload.

### Data members

- **`inner_` (private)** — A sentinel of type `inner-sentinel`. (exposition-only
  member object*)

### Member functions

- **(constructor)** — constructs a sentinel (public member function)

### Non-member functions

- **operator== (C++23)** — compares a sentinel with an iterator returned from
  `adjacent_transform_view::begin` (function)
- **operator- (C++23)** — computes the distance between a sentinel and an
  iterator returned from `adjacent_transform_view::begin` (function)

### Example

### References

- C++23 standard (ISO/IEC 14882:2023):

### See also

---
*Source: https://en.cppreference.com/w/cpp/ranges/adjacent_transform_view/sentinel*
