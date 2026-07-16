# std::mdspan<T,Extents,LayoutPolicy,AccessorPolicy>::size

```cpp
constexpr size_type size() const noexcept;  // (since C++23)
```

Returns the number of elements in `mdspan`.

Equivalent to `extents().fwd-prod-of-extents(rank())`.

If the size of the multidimensional index space `extents()` is not representable
as a value of type `size_type`, the behavior of the program is undefined.

### Parameters

(none)

### Return value

The number of elements.

### Example

### See also

---
*Source: https://en.cppreference.com/w/cpp/container/mdspan/size*
