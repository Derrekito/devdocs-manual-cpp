# std::ranges::slide_view<V>::*sentinel*

```cpp
class /*sentinel*/;  // (since C++23) (exposition only*)
```

The return type of `slide_view::end` when the underlying view is not a
`common_range`.

The `/*sentinel*/` is used only when `/*slide-caches-first*/<V>` is true.

### Data members

- **`end_` (private)** — A sentinel of type `ranges::sentinel_t<V>`.
  (exposition-only member object*)

### Member functions

- **(constructor)** — constructs a sentinel (public member function)

### Non-member functions

- **operator== (C++23)** — compares a sentinel with an iterator returned from
  `slide_view::begin` (function)
- **operator- (C++23)** — computes the distance between a sentinel and an
  iterator returned from `slide_view::begin` (function)

### Example

### References

- C++23 standard (ISO/IEC 14882:2023):

### See also

---
*Source: https://en.cppreference.com/w/cpp/ranges/slide_view/sentinel*
