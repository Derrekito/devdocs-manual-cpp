# std::basic_const_iterator<Iter>::operator*

```cpp
constexpr std::iter_const_reference_t<Iter> operator*() const;  // (1) (since C++23)
constexpr const auto* operator->() const
    requires std::is_lvalue_reference_v<std::iter_reference_t<Iterator>> &&
        std::same_as<std::remove_cvref_t<std::iter_reference_t<Iterator>>,
                     value_type>;  // (2) (since C++23)
```

Returns a reference or pointer to the const version of the current element.

1) Returns `static_cast<std::iter_const_reference_t<Iter>>(*base())`.

2) If `Iter` models `contiguous_iterator`, returns `std::to_address(base())`;
   otherwise, returns `std::addressof(*base())`.

### Parameters

(none)

### Return value

1) Reference-to-const that refers to the current element. The result may be a
   prvalue (e.g. if `*base()` is a prvalue).

2) Pointer-to-const that points to the current element.

### Example

### See also

- **operator[]** — accesses an element by index (public member function)

---
*Source: https://en.cppreference.com/w/cpp/iterator/basic_const_iterator/operator**
