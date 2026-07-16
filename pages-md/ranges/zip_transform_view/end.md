# std::ranges::zip_transform_view<F,Views...>::end

```cpp
constexpr auto end();  // (1) (since C++23)
constexpr auto end() const
    requires ranges::range<const /*InnerView*/> &&
             std::regular_invocable<const F&,
                 ranges::range_reference_t<const Views>...>;  // (2) (since C++23)
```

Returns an iterator or a sentinel that compares equal to the end iterator of the
`zip_transform_view`.

Let `zip_` denote the underlying tuple of views:

1) Equivalent to: `if constexpr (ranges::common_range</*InnerView*/>) return
   /*iterator*/<false>(*this, zip_.end()); else return
   /*sentinel*/<false>(zip_.end());`

2) Equivalent to: `if constexpr (ranges::common_range<const /*InnerView*/>)
   return /*iterator*/<true>(*this, zip_.end()); else return
   /*sentinel*/<true>(zip_.end());`

### Parameters

(none)

### Return value

An iterator or sentinel representing the end of the `zip_transform_view`, as
described above.

### Example

### See also

- **begin (C++23)** — returns an iterator to the beginning (public member
  function)
- **ranges::end (C++20)** — returns a sentinel indicating the end of a range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/zip_transform_view/end*
