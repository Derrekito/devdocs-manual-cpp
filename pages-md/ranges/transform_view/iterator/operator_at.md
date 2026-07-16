# std::ranges::transform_view<V,F>::*iterator*<Const>::operator[]

```cpp
constexpr decltype(auto) operator[]( difference_type n ) const
  requires ranges::random_access_range<Base>;  // (since C++20)
```

Returns the element at specified relative location, after transformation.

Effectively returns `std::invoke(*parent_->fun_, current_[n])`, where
`*parent_->fun_` is the transformation function stored in the parent
`transform_view`, and `current_` is the underlying iterator into `V`.

### Parameters

- **n** — position relative to current location.

### Return value

the transformed element

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/transform_view/iterator/operator_at*
