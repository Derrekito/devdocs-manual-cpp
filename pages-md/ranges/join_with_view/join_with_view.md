# std::ranges::join_with_view<V,Pattern>::join_with_view

```cpp
join_with_view()
  requires std::default_initializable<V> &&
           std::default_initializable<Pattern> = default;  // (1) (since C++23)
constexpr explicit join_with_view( V base, Pattern pattern );  // (2) (since C++23)
template< ranges::input_range R >
  requires std::constructible_from<V, views::all_t<R>> &&
           std::constructible_from<Pattern,
               ranges::single_view<ranges::range_value_t<
                                      ranges::range_reference_t<V>>>>
constexpr explicit
    join_with_view( R&& r, ranges::range_value_t<
                               ranges::range_reference_t<V>> e );  // (3) (since C++23)
```

Constructs a `join_with_view`.

1) Default constructor. Value-initializes the underlying view and the pattern.

2) Initializes the underlying view with `std::move(base)` and the stored pattern
   with `std::move(pattern)`.

3) Initializes the underlying view with `views::all(std::forward<R>(r))` and the
   stored pattern with `views::single(std::move(e))`.

### Parameters

- **base** — a view of ranges to be flattened
- **pattern** — view to be used as the delimiter
- **e** — element to be used as the delimiter

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/join_with_view/join_with_view*
