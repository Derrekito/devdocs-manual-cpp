# std::ranges::zip_view<Views...>::end

```cpp
constexpr auto end()
    requires (!(/*simple-view*/<Views> && ...);  // (1) (since C++23)
constexpr auto end() const
    requires (ranges::range<const Views> && ...);  // (2) (since C++23)
template< class... Rs >
  concept /*zip-is-common*/ =
    (sizeof...(Rs) == 1 && (ranges::common_range<Rs> && ...))
    ||
    (!(ranges::bidirectional_range<Rs> && ...) && (ranges::common_range<Rs> && ...))
    ||
    ((ranges::random_access_range<Rs> && ...) && (ranges::sized_range<Rs> && ...));  // (3) (exposition only*)
```

Returns a sentinel or an iterator that compares equal to the end iterator of the
`std::zip_view`.

Let `views_` denote the underlying tuple of views.

1) Equivalent to:

- `return /*sentinel*/<false>(/*tuple-transform*/(ranges::end, views_));`,

if `/*zip-is-common*/<Views...>` evaluates to `false`. Otherwise,

- `return begin() + std::iter_difference_t</*iterator*/<false>>(size());`,

if `(ranges::random_access_range<Views> && ...)` evaluates to `true`. Otherwise,

- `return /*iterator*/<false>(/*tuple-transform*/(ranges::end, views_));`.

2) Equivalent to:

- `return /*sentinel*/<true>(/*tuple-transform*/(ranges::end, views_));`,

if `/*zip-is-common*/<const Views...>` evaluates to `false`. Otherwise,

- `return begin() + std::iter_difference_t</*iterator*/<true>>(size());`,

if `ranges::random_access_range<const Views> && ...` evaluates to `true`.
Otherwise,

- `return /*iterator*/<true>(/*tuple-transform*/(ranges::end, views_));`.

### Parameters

(none)

### Return value

An iterator or sentinel representing the end of the `zip_view`, as described
above.

### Notes

`ranges::range<const ranges::zip_view<Views...>>` is modeled if and only if for
every type `Vi` in `Views...`, `const Vi` models `range`.

### Example

### See also

- **begin (C++23)** — returns an iterator to the beginning (public member
  function)
- **ranges::end (C++20)** — returns a sentinel indicating the end of a range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/zip_view/end*
