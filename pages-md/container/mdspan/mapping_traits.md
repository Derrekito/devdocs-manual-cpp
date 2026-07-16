# std::mdspan<T,Extents,LayoutPolicy,AccessorPolicy>::is_unique, std::mdspan<T,Extents,LayoutPolicy,AccessorPolicy>::is_exhaustive, std::mdspan<T,Extents,LayoutPolicy,AccessorPolicy>::is_strided, std::mdspan<T,Extents,LayoutPolicy,AccessorPolicy>::is_always_unique, std::mdspan<T,Extents,LayoutPolicy,AccessorPolicy>::is_always_exhaustive, std::mdspan<T,Extents,LayoutPolicy,AccessorPolicy>::is_always_strided

```cpp
constexpr bool is_unique() const;  // (1) (since C++23)
constexpr bool is_exhaustive() const;  // (2) (since C++23)
constexpr bool is_strided() const;  // (3) (since C++23)
static member functions
static constexpr bool is_always_unique();  // (4) (since C++23)
static constexpr bool is_always_exhaustive();  // (5) (since C++23)
static constexpr bool is_always_strided();  // (6) (since C++23)
```

Check if (1-3) the layout mapping `map_` or (4-6) its type `mapping_type`
satisfies some traits.

1-3) Let `func` be (1) `is_unique`, (2) `is_exhaustive`, or (3) `is_strided`,
   then it's equivalent to `return map_.func();`.

4-6) Let `func` be (4) `is_always_unique`, (5) `is_always_exhaustive`, or (6)
   `is_always_strided`, then it's equivalent to `return mapping_type::func();`.

### Parameters

(none)

### Return value

See above.

### Example

### See also

---
*Source: https://en.cppreference.com/w/cpp/container/mdspan/mapping_traits*
