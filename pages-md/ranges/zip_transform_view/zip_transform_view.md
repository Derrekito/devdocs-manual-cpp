# std::ranges::zip_transform_view<F,Views...>::zip_transform_view

```cpp
zip_transform_view() = default;  // (1) (since C++23)
constexpr zip_transform_view( F fun, Views... views );  // (2) (since C++23)
```

1) Default constructor. Value-initializes the stored invocable object and all
adapted `view` objects.

The default constructor is deleted if

- `F` does not satisfy `default_initializable`, or
- `std::is_default_constructible_v` is `false` for at least one type in
  `Views...`.

2) Move constructs the stored invocable object from `fun` and every adapted
   `view` object from the corresponding view in `views...`.

### Parameters

- **f** — invocable object used for generation of elements of
  `zip_transform_view`
- **views** — view objects to adapt

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/zip_transform_view/zip_transform_view*
