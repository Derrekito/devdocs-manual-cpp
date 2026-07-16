# std::ranges::chunk_by_view<V,Pred>::end

```cpp
constexpr auto end();  // (since C++23)
```

Returns an iterator or a sentinel representing the end of the `chunk_by_view`.

Equivalent to:

```cpp
if constexpr (ranges::common_range<V>)
    return /*iterator*/(*this, ranges::end(base_), ranges::end(base_));
else
    return std::default_sentinel;
```

### Parameters

(none)

### Return value

An iterator to the element following the last element, or a sentinel which
compares equal to the end iterator.

### Notes

`end()` returns an iterator if and only if the underlying view is a
`common_range`: `chunk_by_view<V,Pred>` models `common_range` whenever `V` does.

### Example

### See also

- **begin (C++23)** — returns an iterator to the beginning (public member
  function)
- **ranges::end (C++20)** — returns a sentinel indicating the end of a range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/chunk_by_view/end*
