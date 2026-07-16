# std::ranges::adjacent_transform_view<V,F,N>::*iterator*<Const>::*iterator*

```cpp
/*iterator*/() = default;  // (1) (since C++23)
constexpr /*iterator*/( /*iterator*/<!Const> i )
    requires Const &&
        std::convertible_to</*inner-iterator*/<false>, /*inner-iterator*/<Const>>;  // (2) (since C++23)
private:
constexpr /*iterator*/( Parent& parent, /*inner-iterator*/<Const> inner );  // (3) (exposition only*)
```

Construct an iterator.

1) Default constructor. Value-initializes the underlying pointer `parent_` with
   `nullptr`, and default-initializes the underlying iterator `inner_`.

2) Conversion from `/*iterator*/<false>` to `/*iterator*/<true>`. Initializes
   the underlying pointer `parent_` with `i.parent_`, and move constructs the
   underlying iterator `inner_` with `std::move(i.inner_)`.

3) This iterator also has a private constructor which is used by
   `ranges::adjacent_transform_view::begin` and
   `ranges::adjacent_transform_view::end`. This constructor is not accessible to
   users. Initializes `parent_` with `std::addressof(parent)`, and move
   constructs `inner_` with `std::move(inner)`.

### Parameters

- **i** — an `/*iterator*/<false>`
- **parent** — an owning object of type `adjacent_transform_view`
- **inner** — an iterator of type `adjacent_transform_view::``inner_iterator`

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/adjacent_transform_view/iterator/iterator*
