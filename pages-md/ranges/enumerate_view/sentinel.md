# std::ranges::enumerate_view<V>::*sentinel*

```cpp
template< bool Const >
class /*sentinel*/;  // (since C++23) (exposition only*)
```

The return type of `enumerate_view::end` when the underlying view is not a
`common_range`.

The type `/*sentinel*/<true>` is returned by the const-qualified overload. The
type `/*sentinel*/<false>` is returned by the non-const-qualified overload.

### Member types

- **`Base` (private)** — `const V` if `Const` is `true`, otherwise `V`.
  (exposition-only member type*)

### Data members

- **`end_` (private)** — A sentinel of type `ranges::sentinel_t<Base>`
  (exposition-only member object*)

### Member functions

- **(constructor)** — constructs a sentinel (public member function)
- **base (C++23)** — returns a sentinel indicating the end of a range (public
  member function)

### Non-member functions

- **operator== (C++23)** — compares a sentinel with an iterator returned from
  `enumerate_view::begin` (function)
- **operator- (C++23)** — computes the distance between a sentinel and an
  iterator returned from `enumerate_view::begin` (function)

### Example

### References

- C++23 standard (ISO/IEC 14882:2023):

### See also

---
*Source: https://en.cppreference.com/w/cpp/ranges/enumerate_view/sentinel*
