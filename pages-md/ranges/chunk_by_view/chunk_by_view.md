# std::ranges::chunk_by_view<V,Pred>::chunk_by_view

```cpp
chunk_by_view()
    requires std::default_initializable<V> &&
             std::default_initializable<Pred>
    = default;  // (1) (since C++23)
constexpr explicit chunk_by_view( V base, Pred pred );  // (2) (since C++23)
```

Constructs a `chunk_by_view`.

1) Default constructor. Value-initializes the underlying data members via their
respective default member initializers:

- the view `base_` via `= V()`,
- the binary predicate `pred_` via `= Pred()`.

2) Move constructs the underlying data members:

- the view `base_` with `std::move(base)`,
- the binary predicate `pred_` with `std::move(pred)`.

### Parameters

- **base** — the view to split
- **pred** — the function object (a binary predicate) used as a splitting
  criteria

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_by_view/chunk_by_view*
