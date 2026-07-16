# std::ranges::transform_view<V,F>::*sentinel*<Const>::*sentinel*

```cpp
/*sentinel*/() = default;  // (1) (since C++20)
constexpr explicit /*sentinel*/( ranges::sentinel_t<Base> end );  // (2) (since C++20)
constexpr /*sentinel*/( /*sentinel*/<!Const> i )
  requires Const &&
    std::convertible_to<ranges::sentinel_t<V>, ranges::sentinel_t<Base>>;  // (3) (since C++20)
```

Constructs a sentinel.

1) Default constructor. Value-initializes the underlying sentinel.

2) Initializes the underlying sentinel with `end`.

3) Conversion from `/*sentinel*/<false>` to `/*sentinel*/<true>`. Move
   constructs corresponding members.

### Parameters

- **end** — a sentinel representing the end of (possibly const-qualified) `V`
- **i** — a `/*sentinel*/<false>`

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/transform_view/sentinel/sentinel*
