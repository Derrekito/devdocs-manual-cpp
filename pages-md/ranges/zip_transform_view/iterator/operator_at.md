# std::ranges::zip_transform_view<F,Views...>::*iterator*<Const>::operator[]

```cpp
constexpr decltype(auto) operator[]( difference_type n ) const
    requires ranges::random_access_range<Base>;  // (since C++23)
```

Returns the element at specified relative location, after transformation.

Equivalent to

```cpp
return
    std::apply
    (
        [&]<class... Is>(const Is&... iters) -> decltype(auto)
        {
            return std::invoke(*parent_->fun_, iters[std::iter_difference_t<Is>(n)]...);
        },
        inner_.current_
    );
```

where `*parent_->fun_` is the transformation function of type `F` stored in the
parent `ranges::zip_transform_view`, and `current_` is the underlying tuple of
iterators into `Views...`.

### Parameters

- **n** — position relative to current location.

### Return value

The element which is the result of transformation (mapping).

### Notes

The behavior is undefined if the `parent_` pointer to parent
`ranges::zip_transform_view` is null (e.g. if `*this` is default constructed).

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/zip_transform_view/iterator/operator_at*
