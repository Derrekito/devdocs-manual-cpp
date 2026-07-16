# std::ranges::adjacent_view<V,N>::*sentinel*<Const>::*sentinel*

```cpp
/*sentinel*/() = default;  // (1) (since C++23)
constexpr /*sentinel*/( /*sentinel*/<!Const> i )
  requires Const &&
    std::convertible_to<ranges::sentinel_t<V>, ranges::sentinel_t<Base>>;  // (2) (since C++23)
```

Constructs a sentinel.

1) Default constructor. Value-initializes the underlying sentinel (denoted as
   `end_`) with `ranges::sentinel_t<Base>()`.

2) Conversion from `/*sentinel*/<false>` to `/*sentinel*/<true>`. Move
   constructs the underlying sentinel `end_` with the corresponding member of
   `i`.

This type also has a private constructor which is used by `adjacent_view::end`.
This constructor is not accessible to users.

### Parameters

- **i** — a `/*sentinel*/<false>`

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/adjacent_view/sentinel/sentinel*
