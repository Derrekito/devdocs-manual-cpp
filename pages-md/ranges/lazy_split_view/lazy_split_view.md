# std::ranges::lazy_split_view<V,Pattern>::lazy_split_view

```cpp
lazy_split_view()
  requires std::default_initializable<V> &&
           std::default_initializable<Pattern> = default;  // (1) (since C++20)
constexpr explicit lazy_split_view( V base, Pattern pattern );  // (2) (since C++20)
template< ranges::input_range R >
  requires std::constructible_from<V, views::all_t<R>> &&
           std::constructible_from<Pattern, ranges::single_view<
                                                ranges::range_value_t<R>>>
constexpr explicit lazy_split_view( R&& r, ranges::range_value_t<R> e );  // (3) (since C++20)
```

Constructs a `lazy_split_view`.

1) Default constructor. Value-initializes the underlying view and the delimiter.

2) Initializes the underlying view with `std::move(base)` and the delimiter with
   `std::move(pattern)`.

3) Initializes the underlying view with `views::all(std::forward<R>(r))` and the
   delimiter with `ranges::single_view{std::move(e)}`.

### Parameters

- **base** — the underlying view (to be split)
- **pattern** — view to be used as the delimiter
- **e** — element to be used as the delimiter

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3714 (P2711R1) | C++20 | the multi-parameter constructor was not explicit
      | made explicit

---
*Source: https://en.cppreference.com/w/cpp/ranges/lazy_split_view/lazy_split_view*
