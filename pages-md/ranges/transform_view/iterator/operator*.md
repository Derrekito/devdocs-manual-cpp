# std::ranges::transform_view<V,F>::*iterator*<Const>::operator*

```cpp
constexpr decltype(auto) operator*() const;  // (since C++20)
```

Returns the transformed element.

Effectively returns `std::invoke(*parent_->fun_, *current_)`, where
`*parent_->fun_` is the transformation function stored in the parent
`transform_view`, and `current_` is the underlying iterator into `V`.

### Parameters

(none)

### Return value

The transformed element.

### Notes

`operator->` is not provided.

The behavior is undefined if the pointer to parent `transform_view` is null
(e.g. if `*this` is default constructed).

If `*current_` is a prvalue, its lifetime ends before this function returns. If
the transformation function returns a reference or pointer to it, the result
would dangle.

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/transform_view/iterator/operator**
