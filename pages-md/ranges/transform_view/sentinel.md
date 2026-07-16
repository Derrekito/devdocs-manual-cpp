# std::ranges::transform_view<V,F>::*sentinel*

```cpp
template< bool Const >
class /*sentinel*/  // (since C++20) (exposition only*)
```

The return type of `transform_view::end` when the underlying view is not a
`common_range`.

The type `/*sentinel*/<true>` is returned by the const-qualified overload. The
type `/*sentinel*/<false>` is returned by the non-const-qualified overload.

### Member types

- **`Parent` (private)** — `const transform_view<V, F>` if `Const` is `true`,
  otherwise `transform_view<V, F>`. (exposition-only member type*)
- **`Base` (private)** — `const V` if `Const` is `true`, otherwise `V`.
  (exposition-only member type*)

### Data members

- **`end_` (private)** — A sentinel of type `std::sentinel_t<Base>` obtained
  from (possibly const-qualified) `V`. (exposition-only member object*)

### Member functions

- **(constructor)** — constructs a sentinel (public member function)
- **base (C++20)** — returns the underlying sentinel (public member function)

### Non-member functions

- **operator== (C++20)** — compares a sentinel with an iterator returned from
  `transform_view::begin` (function)
- **operator- (C++20)** — computes the distance between a sentinel and an
  iterator returned from `transform_view::begin` (function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/transform_view/sentinel*
