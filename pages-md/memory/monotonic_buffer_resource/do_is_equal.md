# std::pmr::monotonic_buffer_resource::do_is_equal

```cpp
virtual bool do_is_equal( const std::pmr::memory_resource& other ) const noexcept;  // (since C++17)
```

Compare `*this` with `other` for identity - memory allocated using a
`monotonic_buffer_resource` can only be deallocated using that same resource.

### Return value

`this == &other`

### Defect report

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3000 | C++17 | unnecessary `dynamic_cast` was performed | removed

### See also

- **do_is_equal [virtual]** — compare for equality with another
  `memory_resource` (virtual private member function of
  `std::pmr::memory_resource`)

---
*Source: https://en.cppreference.com/w/cpp/memory/monotonic_buffer_resource/do_is_equal*
