# std::ranges::zip_view<Views...>::begin

```cpp
constexpr auto begin()
    requires (!(/*simple-view*/<Views> && ...));  // (1) (since C++23)
constexpr auto begin() const
    requires (ranges::range<const Views> && ...);  // (2) (since C++23)
```

Obtains the beginning iterator of `zip_view`.

1) Equivalent to `return /*iterator*/<false>(/*tuple-transform*/(ranges::begin,
   views_));`.

2) Equivalent to `return /*iterator*/<true>(/*tuple-transform*/(ranges::begin,
   views_));`.

### Parameters

(none)

### Return value

Iterator to the first element.

### Notes

`ranges::range<const ranges::zip_view<Views...>>` is modeled if and only if for
every type `Vi` in `Views...`, `const Vi` models `range`.

### Example

### See also

- **end (C++23)** — returns an iterator or a sentinel to the end (public member
  function)
- **ranges::begin (C++20)** — returns an iterator to the beginning of a range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/zip_view/begin*
