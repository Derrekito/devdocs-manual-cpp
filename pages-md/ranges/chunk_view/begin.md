# std::ranges::chunk_view<V>::begin

```cpp
V models only input_range
constexpr __outer_iterator begin();  // (1) (since C++23)
V models forward_range
constexpr auto begin() requires (!__simple_view<V>);  // (2) (since C++23)
constexpr auto begin() const requires ranges::forward_range<const V>;  // (3) (since C++23)
```

Returns an iterator to the first element of the `chunk_view`.

Let `base_` denote the underlying view.

1) Available only if `V` models `input_range`. Equivalent to: current_ =
   ranges::begin(base_); remainder_ = n_; return __outer_iterator(*this);
   `current_`, `remainder_`, and `n_` are underlying exposition only data
   members of `chunk_view`.

2) Available if `V` models `forward_range`. Equivalent to: `return
   iterator<false>(this, ranges::begin(base_));`, where `iterator` is exposition
   only nested class.

3) Available if `V` models `forward_range`. Equivalent to: `return
   iterator<true>(this, ranges::begin(base_));`, where `iterator` is exposition
   only nested class.

### Parameters

(none)

### Return value

An iterator to the first element of the `chunk_view`, as described above.

### Example

### See also

- **end (C++23)** — returns an iterator or a sentinel to the end (public member
  function)
- **ranges::begin (C++20)** — returns an iterator to the beginning of a range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_view/begin*
