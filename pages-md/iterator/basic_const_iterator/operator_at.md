# std::basic_const_iterator<Iter>::operator[]

```cpp
constexpr std::iter_const_reference_t<Iter> operator[]( difference_type n ) const
     requires std::random_access_iterator<Iterator>;  // (since C++23)
```

Returns a reference to the element at specified relative location.

### Parameters

- **n** — position relative to current location

### Return value

A reference-to-const to the element at relative location, that is,
`static_cast<std::iter_const_reference_t<Iter>>(base()[n])`.

### Example

### See also

- **operator*operator->** — accesses the pointed-to element (public member
  function)

---
*Source: https://en.cppreference.com/w/cpp/iterator/basic_const_iterator/operator_at*
