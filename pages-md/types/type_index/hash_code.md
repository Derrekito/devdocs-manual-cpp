# std::type_index::hash_code

```cpp
std::size_t hash_code() const noexcept;  // (since C++11)
```

Returns the hash code of the associated `std::type_info` object. Equivalent to
calling `type_info::hash_code` directly.

### Parameters

(none)

### Return value

The hash code of the associated `std::type_info` object.

### Example

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 2144 | C++11 | `type_index::hash_code` was not required to be noexcept |
      required

### See also

- **std::hash<std::type_index> (C++11)** — hash support for `std::type_index`
  (class template specialization)

---
*Source: https://en.cppreference.com/w/cpp/types/type_index/hash_code*
