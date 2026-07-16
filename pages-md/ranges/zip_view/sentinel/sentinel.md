# std::ranges::zip_view<Views...>::*sentinel*<Const>::*sentinel*

```cpp
/*sentinel*/() = default;  // (1) (since C++23)
constexpr /*sentinel*/( /*sentinel*/<!Const> i )
    requires Const &&
        (std::convertible_to<
            ranges::sentinel_t<Views>,
            ranges::sentinel_t</*maybe-const*/<Const, Views>>> && ...);  // (2) (since C++23)
```

Constructs a sentinel.

1) Default constructor. Value-initializes the underlying tuple of sentinels
   `end_`.

2) Conversion from `/*sentinel*/<false>` to `/*sentinel*/<true>`. Move
   constructs the underlying tuple of sentinels `end_` with `std::move(i.end_)`.

### Parameters

- **i** — a `/*sentinel*/<false>`

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/zip_view/sentinel/sentinel*
