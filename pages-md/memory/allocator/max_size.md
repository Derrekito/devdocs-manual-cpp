# std::allocator<T>::max_size

```cpp
size_type max_size() const throw();  // (until C++11)
size_type max_size() const noexcept;  // (since C++11) (deprecated in C++17) (removed in C++20)
```

Returns the maximum theoretically possible value of `n`, for which the call
`allocate(n, 0)` could succeed.

In most implementations, this returns `std::numeric_limits<size_type>::max() /
sizeof(value_type)`.

### Parameters

(none)

### Return value

The maximum supported allocation size.

### See also

- **max_size [static]** — returns the maximum object size supported by the
  allocator (public static member function of `std::allocator_traits<Alloc>`)

---
*Source: https://en.cppreference.com/w/cpp/memory/allocator/max_size*
