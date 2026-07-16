# std::ranges::zip_transform_view<F,Views...>::*iterator*<Const>::*iterator*

```cpp
/*iterator*/iterator() = default;  // (1) (since C++23)
constexpr /*iterator*/( /*iterator*/<!Const> i )
    requires Const &&
             std::convertible_to</*ziperator*/<false>, /*ziperator*/<Const>>;  // (2) (since C++23)
constexpr /*iterator*/( Parent& parent, /*ziperator*/<Const> inner );  // (3) (exposition only*)
```

Construct an iterator.

1) Default constructor. Default-initializes the underlying iterators, and
   value-initializes the pointer to parent `ranges::zip_transform_view` with
   `nullptr`.

2) Conversion from `/*iterator*/<false>` to `/*iterator*/<true>`. Move
   constructs the underlying pointer to parent `parent_` with `i.parent_` and
   `inner_` with `std::move(i.inner_)`.

3) Initializes the pointer to parent `parent_` with `std::addressof(parent)`,
   and the underlying `inner_` iterator with `std::move(inner)`. This
   constructor is not accessible to users.

### Parameters

- **i** — an `/*iterator*/<false>`
- **parent** — a (possibly const-qualified) `ranges::zip_transform_view`
- **inner** — an iterator of type `ziperator``<Const>`

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/zip_transform_view/iterator/iterator*
