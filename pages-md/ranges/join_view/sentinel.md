# std::ranges::join_view<V>::*sentinel*

```cpp
template< bool Const >
class /*sentinel*/  // (since C++20) (exposition only*)
```

The return type of `join_view::end` when either of the underlying ranges (`V` or
`ranges::range_reference_t<V>`) is not a `common_range`, or when the parent
`join_view` is not a `forward_range`.

If `V` is not a simple view, `Const` is true for sentinels returned from the
const overloads, and false otherwise. If `V` is a simple view, `Const` is true.

### Member types

- **`Parent` (private)** — `const ranges::join_view<V>` if `Const` is true,
  otherwise `ranges::join_view<V>` (exposition-only member type*)
- **`Base` (private)** — `const V` if `Const` is true, otherwise `V`
  (exposition-only member type*)

### Data members

- **`end_` (private)** — A sentinel of type `std::sentinel_t<Base>` obtained
  from (possibly const-qualified) `V`. (exposition-only member object*)

### Member functions

- **(constructor)** — constructs a sentinel (public member function)

### Non-member functions

- **operator== (C++20)** — compares a sentinel with an iterator returned from
  `join_view::begin` (function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/join_view/sentinel*
