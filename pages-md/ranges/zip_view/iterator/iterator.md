# std::ranges::zip_view<Views...>::*iterator*<Const>::*iterator*

```cpp
/*iterator*/() = default;  // (1) (since C++23)
constexpr /*iterator*/( /*iterator*/<!Const> i )
    requires Const &&
        (std::convertible_to<ranges::iterator_t<Views>,
                             ranges::iterator_t</*maybe-const*/<Const, Views>>> && ...);  // (2) (since C++23)
```

Construct an iterator.

1) Default constructor. Value-initializes the underlying tuple of iterators to
   their default values.

2) Conversion from `/*iterator*/<false>` to `/*iterator*/<true>`. Move
   constructs the underlying tuple of iterators `current_` with
   `std::move(i.current)`.

This iterator also has a private constructor which is used by `zip_view::begin`
and `zip_view::end`. This constructor is not accessible to users.

### Parameters

- **i** — an `/*iterator*/<false>`

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/zip_view/iterator/iterator*
