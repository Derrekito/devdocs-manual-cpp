# std::pmr::operator==, std::pmr::operator!=

```cpp
bool operator==( const std::pmr::memory_resource& a,
                 const std::pmr::memory_resource& b ) noexcept;  // (1) (since C++17)
bool operator!=( const std::pmr::memory_resource& a,
                 const std::pmr::memory_resource& b ) noexcept;  // (2) (since C++17) (until C++20)
```

Compares the `memory_resource`s `a` and `b` for equality. Two `memory_resource`s
compare equal if and only if memory allocated from one `memory_resource` can be
deallocated from the other and vice versa.

The `!=` operator is synthesized from `operator==`.
*(since C++20)*

### Return value

1) `&a == &b || a.is_equal(b)`

2) `!(a == b)`

### See also

- **is_equal** — compare for equality with another `memory_resource` (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/memory_resource/operator_eq*
