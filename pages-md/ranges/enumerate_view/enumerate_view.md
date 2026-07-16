# std::ranges::enumerate_view<V>::enumerate_view

```cpp
enumerate_view() requires std::default_initializable<V> = default;  // (1) (since C++23)
constexpr explicit enumerate_view( V base );  // (2) (since C++23)
```

Constructs a `enumerate_view`.

1) Default constructor. Value-initializes the underlying view `base_`. After
   construction, `base()` returns a copy of `V()`.

2) Initializes the underlying view `base_` with `std::move(base)`.

### Parameters

- **base** — the underlying view

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/enumerate_view/enumerate_view*
