# std::pmr::polymorphic_allocator<T>::allocate

```cpp
T* allocate( std::size_t n );  // (since C++17) (until C++20)
[[nodiscard]] T* allocate( std::size_t n );  // (since C++20)
```

Allocates storage for `n` objects of type `T` using the underlying memory
resource. Equivalent to `return static_cast<T*>(resource()->allocate(n *
sizeof(T), alignof(T)));`.

### Parameters

- **n** — the number of objects to allocate storage for

### Return value

A pointer to the allocated storage.

### Exceptions

Throws `std::bad_array_new_length` if `n >
std::numeric_limits<std::size_t>::max() / sizeof(T)`; may also throw any
exceptions thrown by the call to `resource()->allocate`.

### Defect reports

The following behavior-changing defect reports were applied retroactively to
previously published C++ standards.

  DR | Applied to | Behavior as published | Correct behavior
  LWG 3038 | C++17 | `allocate` might allocate storage of wrong size | throws
      `length_error` instead
  LWG 3237 | C++17 | the exception thrown by `allocate` was inconsistent with
      `std::allocator::allocate` | made consistent

### See also

- **allocate_bytes (C++20)** — allocate raw aligned memory from the underlying
  resource (public member function)
- **allocate_object (C++20)** — allocates raw memory suitable for an object or
  an array (public member function)
- **new_object (C++20)** — allocates and constructs an object (public member
  function)
- **allocate [static]** — allocates uninitialized storage using the allocator
  (public static member function of `std::allocator_traits<Alloc>`)
- **allocate** — allocates memory (public member function of
  `std::pmr::memory_resource`)

---
*Source: https://en.cppreference.com/w/cpp/memory/polymorphic_allocator/allocate*
