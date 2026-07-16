# std::ranges::adjacent_transform_view<V,F,N>::*iterator*<Const>::operator*

```cpp
constexpr decltype(auto) operator*() const noexcept(/* see below */);  // (since C++23)
```

Returns the current element in the `adjacent_transform_view`.

Let `parent_` and `inner_` be the data members of the iterator. Equivalent to:

```cpp
return apply([&](const auto&... iters) -> decltype(auto)
             {
                 return invoke(*parent_->fun_, *iters...);
             },
             inner_.current_);
```

### Parameters

(none)

### Return value

The current element.

### Exceptions

Let `Is` be the pack `0, 1, ..., (N - 1)`.

`noexcept` specification:

`noexcept(std::invoke(*parent_->fun_, *std::get<Is>(inner_.current_)...))`

### Example

### See also

- **operator[] (C++23)** — accesses an element by index (public member function)

---
*Source: https://en.cppreference.com/w/cpp/ranges/adjacent_transform_view/iterator/operator**
