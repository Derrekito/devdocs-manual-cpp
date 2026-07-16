# std::ranges::zip_transform_view<F,Views...>::*sentinel*<Const>::*sentinel*

```cpp
/*sentinel*/() = default;  // (1) (since C++23)
constexpr /*sentinel*/( /*sentinel*/<!Const> i )
    requires Const &&
             std::convertible_to</*zentinel*/<false>, /*zentinel*/<Const>>;  // (2) (since C++23)
constexpr explicit /*sentinel*/( /*zentinel*/<Const> inner );  // (3) (exposition only*)
```

Constructs a sentinel.

1) Default constructor. Default-initializes the underlying sentinel object
   `inner_`.

2) Conversion from `/*sentinel*/<false>` to `/*sentinel*/<true>`. Move
   constructs the underlying object `inner_` with `std::move(i.inner_)`.

3) Value-initializes the underlying object `inner_` with `inner`. This
   constructor is not accessible to users.

### Parameters

- **i** — a `/*sentinel*/<false>`
- **inner** — an underlying object of type `zentinel`<Const>

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/zip_transform_view/sentinel/sentinel*
