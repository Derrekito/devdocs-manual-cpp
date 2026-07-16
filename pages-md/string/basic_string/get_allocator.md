# std::basic_string<CharT,Traits,Allocator>::get_allocator

```cpp
allocator_type get_allocator() const;  // (until C++20)
constexpr allocator_type get_allocator() const noexcept;  // (since C++20)
```

Returns the allocator associated with the string.

### Parameters

(none)

### Return value

The associated allocator.

### Complexity

Constant.

### See also

- **allocator** — the default allocator (class template)
- **allocator_traits (C++11)** — provides information about allocator types
  (class template)
- **uses_allocator (C++11)** — checks if the specified type supports
  uses-allocator construction (class template)

---
*Source: https://en.cppreference.com/w/cpp/string/basic_string/get_allocator*
