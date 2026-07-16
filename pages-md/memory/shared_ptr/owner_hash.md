# std::shared_ptr<T>::owner_hash

```cpp
std::size_t owner_hash() const noexcept;  // (since C++26)
```

Returns an unspecified value such that for any object `other` where
`owner_equal(other)` is `true`, `owner_hash() == other.owner_hash()` is `true`.

This hashing is used to make shared and weak pointers usable as keys in
unordered associative containers, typically through `std::owner_hash`.

### Return value

A value that is identical for any `std::shared_ptr` or `std::weak_ptr` object
sharing the same ownership.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_smart_ptr_owner_equality` | 202306L | (C++26) | Enabling the use of
      `std::shared_ptr` as keys in unordered associative containers

### Example

### See also

- **owner_hash (C++26)** — provides owner-based hashing for shared and weak
  pointers (class)

---
*Source: https://en.cppreference.com/w/cpp/memory/shared_ptr/owner_hash*
