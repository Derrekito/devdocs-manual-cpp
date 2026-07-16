# std::layout_left

```cpp
struct layout_left;  // (since C++23)
```

`layout_left` is a LayoutMappingPolicy and trivial type which provides a layout
mapping where the leftmost extent has stride 1, and strides increase
left-to-right as the product of extents.

### Member class templates

- **mapping** — a layout mapping of `layout_left` (public member class)

### See also

- **layout_right (C++23)** — row-major multidimensional array layout mapping
  policy; rightmost extent has stride `1` (class)
- **layout_stride (C++23)** — a layout mapping policy with user-defined strides
  (class)

---
*Source: https://en.cppreference.com/w/cpp/container/mdspan/layout_left*
