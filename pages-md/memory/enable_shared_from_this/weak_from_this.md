# std::enable_shared_from_this<T>::weak_from_this

```cpp
std::weak_ptr<T> weak_from_this() noexcept;  // (1) (since C++17)
std::weak_ptr<T const> weak_from_this() const noexcept;  // (2) (since C++17)
```

Returns a `std::weak_ptr<T>` that tracks ownership of `*this` by all existing
`std::shared_ptr` that refer to `*this`.

### Return value

`std::weak_ptr<T>` that shares ownership of `*this` with pre-existing
`std::shared_ptr`s.

### Notes

`weak_from_this` copies the exposition only `weak_ptr` member that is part of
`enable_shared_from_this`.

  Feature-test macro | Value | Std | Feature
  `__cpp_lib_enable_shared_from_this` | 201603L | (C++17) |
      `std::enable_shared_from_this::weak_from_this`

### Example

### See also

- **shared_ptr (C++11)** — smart pointer with shared object ownership semantics
  (class template)

---
*Source: https://en.cppreference.com/w/cpp/memory/enable_shared_from_this/weak_from_this*
