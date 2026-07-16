# std::ranges::join_with_view<V,Pattern>::*sentinel*

```cpp
template< bool Const >
class /*sentinel*/  // (since C++23) (exposition only*)
```

The return type of `join_with_view::end` when either of the underlying ranges
(`V` or `ranges::range_reference_t<V>`) is not a `common_range`, or when the
parent `join_with_view` is not a `forward_range`.

If either `V` or `Pattern` is not a simple view, `Const` is true for sentinels
returned from the const overloads, and false otherwise. If `V` and `Pattern` are
simple views, `Const` is true.

### Member functions

- **(constructor)** — constructs a sentinel (public member function)

### Non-member functions

- **operator== (C++23)** — compares a sentinel with an iterator returned from
  `join_with_view::begin` (function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/join_with_view/sentinel*
