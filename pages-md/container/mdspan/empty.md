# std::mdspan<T,Extents,LayoutPolicy,AccessorPolicy>::empty

```cpp
[[nodiscard]] constexpr bool empty() const noexcept;  // (since C++23)
```

Check if the `mdspan` is empty.

### Parameters

(none)

### Return value

`true` if the size of the multidimensional index space `extents()` is `​0​`,
otherwise `false`.

### Example

### See also

---
*Source: https://en.cppreference.com/w/cpp/container/mdspan/empty*
