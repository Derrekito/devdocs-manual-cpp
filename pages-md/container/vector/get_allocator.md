# std::vector<T,Allocator>::get_allocator

```cpp
allocator_type get_allocator() const;  // (until C++11)
allocator_type get_allocator() const noexcept;  // (since C++11) (until C++20)
constexpr allocator_type get_allocator() const noexcept;  // (since C++20)
```

Returns the allocator associated with the container.

### Parameters

(none)

### Return value

The associated allocator.

### Complexity

Constant.

---
*Source: https://en.cppreference.com/w/cpp/container/vector/get_allocator*
