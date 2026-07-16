# std::weak_ptr<T>::owner_equal

```cpp
template< class Y >
bool owner_equal( const std::weak_ptr<Y>& other ) const noexcept;  // (1) (since C++26)
template< class Y >
bool owner_equal( const std::shared_ptr<Y>& other ) const noexcept;  // (2) (since C++26)
```

Checks whether this `weak_ptr` and `other` share ownership or are both empty.
The comparison is such that two smart pointers compare equivalent only if they
are both empty or if they both own the same object, even if the values of the
pointers obtained by `get()` are different (e.g. because they point at different
subobjects within the same object).

The member function `owner_equal` is an equivalence relation such that
`!owner_before(other) && !other.owner_before(*this)` is `true` if and only if
`owner_equal(other)` is `true`.

This ordering is used to make shared and weak pointers usable as keys in
unordered associative containers, typically through `std::owner_equal`.

### Parameters

- **other** — the `std::shared_ptr` or `std::weak_ptr` to be compared

### Return value

`true` if `*this` and `other` share ownership or are both empty. Otherwise,
returns `false`.

### Notes

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_smart_ptr_owner_equality` | 202306L | (C++26) | Enabling the use of
      `std::weak_ptr` as keys in unordered associative containers

### Example

### See also

- **owner_equal (C++26)** — provides mixed-type owner-based equal comparisons of
  shared and weak pointers (class)

---
*Source: https://en.cppreference.com/w/cpp/memory/weak_ptr/owner_equal*
