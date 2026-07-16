# std::ranges::enumerate_view<V>::*sentinel*<Const>::*sentinel*

```cpp
/*sentinel*/() = default;  // (1) (since C++23)
constexpr /*sentinel*/( /*sentinel*/<!Const> i )
    requires Const &&
        std::convertible_to<ranges::sentinel_t<V>, ranges::sentinel_t<Base>>;  // (2) (since C++23)
private:
constexpr explicit /*sentinel*/( ranges::sentinel_t<Base> end );  // (3) (exposition only*)
```

Constructs a sentinel.

1) Default constructor. Value-initializes the underlying sentinel with
   `ranges::sentinel_t<Base>()`.

2) Conversion from `/*sentinel*/<false>` to `/*sentinel*/<true>`. Move
   constructs the underlying sentinel `end_` with `std::move(other.end_)`.

3) A private constructor which is used by `enumerate_view::end`. Move constructs
   the `end_` with `std::move(end)` This constructor is not accessible to users.

### Parameters

- **i** — a `/*sentinel*/<false>`

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/enumerate_view/sentinel/sentinel*
