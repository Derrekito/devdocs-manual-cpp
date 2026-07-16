# std::ranges::adjacent_transform_view<V,F,N>::*sentinel*<Const>::*sentinel*

```cpp
/*sentinel*/() = default;  // (1) (since C++23)
constexpr /*sentinel*/( /*sentinel*/<!Const> i )
    requires Const &&
        std::convertible_to</*inner-sentinel*/<false>, /*inner-sentinel*/<Const>>;  // (2) (since C++23)
private:
constexpr explicit /*sentinel*/( /*inner-sentinel*/<Const> inner );  // (3) (exposition only*)
```

Construct a sentinel.

1) Default constructor. Default-initializes the underlying sentinel `inner_`.

2) Conversion from `/*sentinel*/<false>` to `/*sentinel*/<true>`. Move
   constructs the underlying sentinel `inner_` with `std::move(i.inner_)`.

3) This sentinel also has a private constructor which is used by
   `ranges::adjacent_transform_view::end`. This constructor is not accessible to
   users. Initializes the underlying sentinel `inner_` with `inner`.

### Parameters

- **i** — an `/*sentinel*/<false>`
- **inner** — a sentinel of type `adjacent_transform_view::``inner_sentinel`

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/adjacent_transform_view/sentinel/sentinel*
