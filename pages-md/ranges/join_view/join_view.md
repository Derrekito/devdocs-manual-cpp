# std::ranges::join_view<V>::join_view

```cpp
join_view() requires std::default_initializable<V> = default;  // (1) (since C++20)
constexpr explicit join_view( V base );  // (2) (since C++20)
```

Constructs a `join_view`.

1) Default constructor. Value-initializes the underlying view. After
   construction, `base()` returns a copy of `V()`.

2) Initializes the underlying view with `std::move(base)`.

### Parameters

- **base** — a view of ranges to be flattened

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/join_view/join_view*
