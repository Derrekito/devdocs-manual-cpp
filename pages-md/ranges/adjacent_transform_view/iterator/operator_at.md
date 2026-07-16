# std::ranges::adjacent_transform_view<V,F,N>::*iterator*<Const>::operator[]

```cpp
constexpr decltype(auto) operator[]( difference_type n ) const
    requires ranges::random_access_range<Base>;  // (since C++23)
```

Returns an element at specified relative location.

Let `parent_` and `inner_` be the data members of the iterator. Equivalent to:

```cpp
return apply([&](const auto&... iters) -> decltype(auto)
             {
                return invoke(*parent_->fun_, iters[n]...);
             },
             inner_.current_);
```

### Parameters

- **n** — position relative to current location

### Return value

The element at displacement `n` relative to the current location.

### Example

### See also

- **operator* (C++23)** — accesses the element (public member function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/adjacent_transform_view/iterator/operator_at*
