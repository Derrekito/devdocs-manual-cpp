# std::layout_stride

```cpp
struct layout_stride;  // (since C++23)
```

`layout_stride` is a LayoutMappingPolicy and trivial type which provides a
layout mapping where the strides are user-defined.

### Member class templates

- **mapping** — a layout mapping of `layout_stride` (public member class)

### See also

- **layout_left (C++23)** — column-major multidimensional array layout mapping
  policy; leftmost extent has stride `1` (class)
- **layout_right (C++23)** — row-major multidimensional array layout mapping
  policy; rightmost extent has stride `1` (class)

---
*Source: https://en.cppreference.com/w/cpp/container/mdspan/layout_stride*
