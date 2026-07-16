# std::ranges::zip_view<Views...>::*sentinel*

```cpp
template< bool Const >
class /*sentinel*/;  // (since C++23) (exposition only*)
```

The return type of `zip_view::end` when the underlying view is not a
`common_range`.

The type `/*sentinel*/<true>` or `/*sentinel*/<false>` treats the underlying
view as const-qualified or non-const-qualified respectively.

### Data members

- **`end_` (private)** — `std::tuple<ranges::sentinel_t<Views>...>` if `Const`
  is `false`, or `std::tuple<ranges::sentinel_t<const Views>...>` if `Const` is
  `true`. (exposition-only member object*)

### Member functions

- **(constructor)** — constructs a sentinel (public member function)

### Non-member functions

- **operator== (C++23)** — compares a sentinel with an iterator returned from
  `zip_view::begin` (function)
- **operator- (C++23)** — computes the distance between a sentinel and an
  iterator returned from `zip_view::begin` (function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/zip_view/sentinel*
