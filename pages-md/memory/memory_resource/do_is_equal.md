# std::pmr::memory_resource::do_is_equal

```cpp
virtual bool do_is_equal( const std::pmr::memory_resource& other ) const noexcept = 0;  // (since C++17)
```

Compares `*this` for equality with `other`.

Two `memory_resource`s compare equal if and only if memory allocated from one
`memory_resource` can be deallocated from the other and vice versa.

### Notes

The most-derived type of `other` may not match the most derived type of `*this`.
A derived class implementation therefore must typically check whether the most
derived types of `*this` and `other` match using `dynamic_cast`, and immediately
return `false` if the cast fails.

### See also

- **is_equal** — compare for equality with another `memory_resource` (public
  member function)

---
*Source: https://en.cppreference.com/w/cpp/memory/memory_resource/do_is_equal*
