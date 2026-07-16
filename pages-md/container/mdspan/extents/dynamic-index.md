# std::extents<IndexType,Extents...>::*dynamic-index*

```cpp
private:
    template< class OtherIndexType >
    static constexpr auto /*dynamic-index*/( rank_type i ) noexcept;  // (since C++23) (exposition only*)
```

Returns the number of dynamic extents below index `i`. If `i <= rank()` is
false, the behavior is undefined.

### Parameters

- **i** — the index

### Return value

The number of `Er` with `r < i` for which `Er` is a dynamic extent.

### Example

### See also

---
*Source: https://en.cppreference.com/w/cpp/container/mdspan/extents/dynamic-index*
