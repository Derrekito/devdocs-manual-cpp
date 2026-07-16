# std::ranges::join_view<V>::*sentinel*<Const>::*sentinel*

```cpp
/*sentinel*/() = default;  // (1) (since C++20)
constexpr explicit /*sentinel*/( Parent& parent );  // (2) (since C++20)
constexpr /*sentinel*/( /*sentinel*/<!Const> i )
  requires Const &&
    std::convertible_to<ranges::sentinel_t<V>, ranges::sentinel_t<Base>>;  // (3) (since C++20)
```

Constructs a sentinel.

1) Default constructor. Value-initializes the underlying sentinel.

2) Initializes the underlying sentinel `end_` with `ranges::end(parent.base_)`.

3) Conversion from `/*sentinel*/<false>` to `/*sentinel*/<true>`. Move
   constructs the underlying sentinel `end_` with `std::move(i.end_)`.

### Parameters

- **parent** — a (possibly const-qualified) `ranges::join_view`
- **i** — a `/*sentinel*/<false>`

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/join_view/sentinel/sentinel*
