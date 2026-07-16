# std::extents<IndexType,Extents...>::*index-cast*

```cpp
template< class OtherIndexType >
static constexpr auto /*index-cast*/( OtherIndexType&& i ) noexcept;  // (since C++23) (exposition only*)
```

Casts the index `i` in type `OtherIndexType` into certain integral type. It is
equivalent to:

- `return i;`, if `OtherIndexType` is an integral type other than bool and
- `return static_cast<index_type>(i);` otherwise.

### Parameters

- **i** — the index to be cast

### Return value

Cast index.

### Notes

### Example

### See also

---
*Source: https://en.cppreference.com/w/cpp/container/mdspan/extents/index-cast*
