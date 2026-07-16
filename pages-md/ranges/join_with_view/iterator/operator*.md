# std::ranges::join_with_view<V,Pattern>::*iterator*<Const>::operator*

```cpp
constexpr decltype(auto) operator*() const;  // (since C++23)
```

Returns the current element in the joined view.

The return type is:

```cpp
std::common_reference_t<ranges::range_reference_t<InnerBase>,
                        ranges::range_reference_t<PatternBase>>
```

### Parameters

(none)

### Return value

The current element.

### Example

---
*Source: https://en.cppreference.com/w/cpp/ranges/join_with_view/iterator/operator**
