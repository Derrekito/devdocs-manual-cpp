# std::ranges::subrange<I,S,K>::begin

```cpp
constexpr I begin() const requires std::copyable<I>;  // (1) (since C++20)
[[nodiscard]] constexpr I begin() requires (!std::copyable<I>);  // (2) (since C++20)
```

Obtains the iterator to the first element of the `subrange`, or the end iterator
if the view is empty.

1) Returns a copy of the stored iterator if the iterator type is copyable.

2) Returns an iterator move-constructed from the stored iterator if the iterator
   type is not copyable.

### Parameters

(none)

### Return value

1) An iterator copy-constructed from the stored iterator.

2) An iterator move-constructed from the stored iterator.

### Notes

A call to (2) may leave the stored iterator in a valid but unspecified state,
depending on the behavior of the move constructor of `I`.

### Example

### See also

- **end (C++20)** — obtains the sentinel (public member function)
- **begincbegin (C++11)(C++14)** — returns an iterator to the beginning of a
  container or array (function template)
- **ranges::begin (C++20)** — returns an iterator to the beginning of a range
  (customization point object)

---
*Source: https://en.cppreference.com/w/cpp/ranges/subrange/begin*
