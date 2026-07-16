# std::ranges::join_with_view<V,Pattern>::*sentinel*<Const>::*sentinel*

```cpp
/*sentinel*/() = default;  // (1) (since C++23)
constexpr /*sentinel*/( /*sentinel*/<!Const> i )
  requires Const &&
    std::convertible_to<ranges::sentinel_t<V>, ranges::sentinel_t<const V>>;  // (2) (since C++23)
```

Constructs a sentinel.

1) Default constructor. Value-initializes the underlying sentinel.

2) Conversion from `/*sentinel*/<false>` to `/*sentinel*/<true>`. Move
   constructs the underlying sentinel with the corresponding member of `i`.

This type also has a private constructor which is used by `join_with_view::end`.
This constructor is not accessible to users.

### Parameters

- **i** — a `/*sentinel*/<false>`

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/join_with_view/sentinel/sentinel*
