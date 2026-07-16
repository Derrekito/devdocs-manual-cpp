# std::mdspan<T,Extents,LayoutPolicy,AccessorPolicy>::stride

```cpp
constexpr index_type stride( rank_type r ) const;  // (since C++23)
```

Returns the stride of the layout mapping `map_` in `r`th dimension. Equivalent
to `return map_.stride(r);`.

### Parameters

- **r** — the index of the dimension

### Return value

The stride.

### Example

### See also

---
*Source: https://en.cppreference.com/w/cpp/container/mdspan/stride*
