# std::layout_right

```cpp
struct layout_right;  // (since C++23)
```

`layout_right` is a LayoutMappingPolicy and trivial type which provides a layout
mapping where the rightmost extent has stride 1, and strides increase
right-to-left as the product of extents.

It is the default layout mapping policy used by `std::mdspan` if no
user-specified layout is provided.

### Member class templates

- **mapping** — a layout mapping of `layout_right` (public member class)

### See also

- **layout_left (C++23)** — column-major multidimensional array layout mapping
  policy; leftmost extent has stride `1` (class)
- **layout_stride (C++23)** — a layout mapping policy with user-defined strides
  (class)

---
*Source: https://en.cppreference.com/w/cpp/container/mdspan/layout_right*
