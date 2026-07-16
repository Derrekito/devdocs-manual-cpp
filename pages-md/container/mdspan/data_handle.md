# std::mdspan<T,Extents,LayoutPolicy,AccessorPolicy>::data_handle

```cpp
constexpr const data_handle_type& data_handle() const noexcept;  // (since C++23)
```

Returns the underlying data handle of `data_handle_type`. Equivalent to `return
ptr_;`.

### Parameters

(none)

### Return value

A const reference to the underlying data handle.

### Example

### See also

---
*Source: https://en.cppreference.com/w/cpp/container/mdspan/data_handle*
