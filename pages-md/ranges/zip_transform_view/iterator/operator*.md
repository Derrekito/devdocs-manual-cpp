# std::ranges::zip_transform_view<F,Views...>::*iterator*<Const>::operator*

```cpp
constexpr decltype(auto) operator*() const
    noexcept(/* see description */);  // (since C++23)
```

Returns the transformed element obtained by application of the invocable object
of type `F` to the underlying poined-to elements.

Equivalent to

```cpp
return
    std::apply
    (
        [&](auto const&... iters) -> decltype(auto)
        {
            return std::invoke(*parent_->fun_, *iters...);
        },
        inner_.current_
    );
```

where `*parent_->fun_` is the transformation function stored in the parent
`ranges::zip_transform_view`, and `current_` is the underlying tuple of
iterators into `Views...`.

### Parameters

(none)

### Return value

The element which is the result of transformation (mapping).

### Exceptions

`noexcept` specification:

`noexcept(std::invoke(*parent_->fun_, *std::get<INTS>(inner_.current_)...))`

where `INTS` is the pack of integers `0, 1, ..., (sizeof...(Views)-1)`.

### Notes

`operator->` is not provided.

The behavior is undefined if the `parent_` pointer to parent
`ranges::zip_transform_view` is null (e.g. if `*this` is default constructed).

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/zip_transform_view/iterator/operator**
